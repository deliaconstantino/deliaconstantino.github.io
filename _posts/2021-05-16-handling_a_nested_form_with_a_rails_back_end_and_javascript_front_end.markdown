---
layout: post
title:      "Handling a Nested Form with a Rails Back End  and JavaScript Front End "
date:       2021-05-16 17:35:22 +0000
permalink:  handling_a_nested_form_with_a_rails_back_end_and_javascript_front_end
---


For my fourth project with the Flatiron program I created a single page application that used a Rails API for the backend server with a JavaScript, HTML, and CSS frontend to handle page rendering and styling. The basic premise of the app was that a user could create journal entries using a timer and add a keyword to their entry. One aspect of building this app that proved to be a great learning experience was implementing a nested form. I wanted the seamless user experience of creating a new journal entry and adding a keyword with one click. I had previously implemented a nested form in a pure Rails app. Doing so from a JavaScript fetch call, however, added a new challenge.

I coded my project with a many-to-many relationship between entries and keywords, using a join table for the two models called `entry_keywords`. For clarity, those associations looked like:

```ruby
class Entry < ApplicationRecord
  has_many :entry_keywords, dependent: :destroy
  has_many :keywords, through: :entry_keywords
end

class Keyword < ApplicationRecord
  has_many :entry_keywords, dependent: :destroy
  has_many :entries, through: :entry_keywords
end

class EntryKeyword < ApplicationRecord
  belongs_to :entry
  belongs_to :keyword
end
```

### MVP Goals

For the minimum viable product (MVP), I decided to allow a user to assign one keyword to a new entry, rather than many at once. This could be an existing keyword or a new keyword. If the user selected an existing keyword, it would be found in the database and an association between it and the new entry would be created. If the user created a new keyword, that keyword would be instantiated as a keyword, persisted, AND assigned an association with the new journal entry.  As the bulk of the application is concerned with creating, filtering, and reading journal entries, I decided that the entry model should be the main model for my form. This also meant that form processing would be handled by the `entries_controller ` on the backend.

### Rails Back End Set-Up

First I added the `accepts_nested_attributes_for` macro to the entry model. My `entry.rb` file now looked like:

```ruby
class Entry < ApplicationRecord
  has_many :entry_keywords, dependent: :destroy
  has_many :keywords, through: :entry_keywords
  accepts_nested_attributes_for :keywords
	end
```
	
This macro is a class method provided by ActiveRecord that will create a new method on your main model, `<othermodelname>_attributes=` . This will allow you to create an object of one type through the class of the main object.

For clarity, we can add the model names back in and see what's actually happening.

The `entry_controller` `create` action, will receive the params that are send back from the front end. As the front end uses JavaScript, rather than a Rails nested form, we'll have to make sure to code the params to include what's needed.  A correct version of params, with the `keyword_attributes` tag looks like:

```ruby
pry>params
=>#"entry"=>{"body"=>"sample entry", "time_interval"=>1.0, "keywords_attributes"=>{"name"=>"nature"}}
```

To get params to look like this, the correct information needs to be passed into the fetch call.

### Passing Params from Front End

Let's say the values we want to pass back are encapsulated in the variables listed below. To ensure that they end up in our params hash just the way our backend is expecting, assign those values to an JavaScript object (named `data`) that imitates the expected params hash.  Then pass that `data` object as a JSONified string into the `body` key of the configuration object. Finally, pass that `configObj` as the second argument into the fetch call. This particular configuration object informs the fetch call that is sending a POST request, and also passes the necessary data about entry and keyword to the backend params:

```javascript
//info to pass to params
const body = "sample entry";
const timeInterval = 0.16666666666666666;
const keywordAttributes = {name: "nature"}

// imitate params hash with JavaScript object
const data = {
body: "sample entry",
time_interval: 1.0,
keywords_attributes: {name: "nature"}
};

//create configuration object
	const configObj = {
	method: "POST",
	headers: {
		"Content-Type": "application/json"
	}
	body: JSON.stringify(data),
}

fetch("url_to_backend_controller_action", configObj)
```

### Back End Form Handling

The above fetch call will reach an endpoint on the backend. In this example, the backend endpoint will be the `create` action in the `entries_controller`.

Remember params?

```ruby
pry>params
=>#"entry"=>{"body"=>"sample entry", "time_interval"=>0.16666666666666666, "keywords_attributes"=>{"name"=>"nature"}}
```

When the Entry controller encounters the `keywords_attributes` line in the params hash, it will be spurred to call a method with a matching name. In this case, add that matching method (`keywords_attributes=`) to the Entry model:

```ruby
def keywords_attributes=(keyword_params)
	name = keyword_params[:name].downcase.strip
	if !name.empty?
		keyword = Keyword.find_or_create_by(name: name)
		self.keywords << keyword unless self.keywords.include?(keyword)
	end
end
```

(Side note: As this is a many-to-many relationship, the form could ALSO have been coded to accept multiple keyword instances.  In that case, params would have needed to include another field for the ids of existing keywords as well as the `keywords_attributes` field. However, for simplicity, the form currently only allows adding one keyword to an entry. This means the back end can simply use the name attribute, whether finding or creating a keyword.)

Here `keywords_attributes=` is called with an argument of the `keywords_attributes` from the params hash. As `keywords_attributes` is a key in the params pointing to the value of `{"name"=>"nature"}` , it may be simpler to say that the Entry model method `keywords_attributes=` is called with an argument of that value: `{"name"=>"nature"}` .

So on encountering `keywords_attributes` in params, this method call is made:
`keywords_attributes=({"name"=>"nature"})`.

And the method does four things:

1. First it grabs the specific information that we are looking for, (the value at the key of name) and also sanitizes that input with `.downcase` and `.strip`. (We want to always compare apples to apples, so we will always save keyword names as all lowercase. Anytime we check for them in the database, we'll do so with a lowercase. In addition, we don't want to save random white space or empty strings to our database so be sure to `.strip` that away.)

2. Next, check whether the string is empty to verify whether to do steps 3 and 4. 

(Side note: in Ruby an empty string `""` evaluates to false while a string of just space characters `"    "` evaluates to true .) 

So if the string is empty, it evaluates to false and nothing else happens in this method. In that case, no keyword is assigned to the new entry that is being created. But if the string has some value, continue on to the next step.

3. Here call the ActiveRecord macro `find_or_create_by` and pass in for both the database column name to look in (`name`) and the value to check for (the local variable `name` from step 1). `find_or_create_by` finds and then returns the first instance that matches the query. If it does not find one, it will instead instantiate a new instance with the information that was passed in and return that new instance. The return value of this method, a new or previously existing instance of the Keyword model, is assigned to a local varibale, `keyword`.

4. Finally, while still inside the `if` block, add the `keyword` instance that was returned in step 3 to the collection of keywords on this particular journal entry.

Then the `keywords_attributes=` method ends and you are back in the `entries_controller`.

Once the entry is instantiated, render it to the frontend using json, and include the keyword. This looks like:

```ruby
def create
    entry = Entry.new(entry_params)

    if entry.save
      render json: entry, include: [:keywords]
    else
      render json: entry.errors.full_messages, status: :unprocessable_entity
    end
  end
```

### Return to Front End

Finally, returning to the front end, check the console logged response after parsing it's JSON:

```javascript
fetch("backend/url", configPostObject)
	.then(response => response.json())
	.then(json => console.log(json))
```

Open up the browser tools and you can see that something like this has been console logged:

```javascript
// entry instance:
{id: 145, body: "sample entry", time_interval: 1, created_at: "2021-05-16T01:22:44.802Z", updated_at: "2021-05-16T01:22:44.802Z", …}
body: "sample entry"
created_at: "2021-05-16T01:22:44.802Z"
id: 145

// keyword array

keywords: Array(1)
0: {id: 4, name: "nature", created_at: "2021-05-12T17:22:45.771Z", updated_at: "2021-05-12T17:22:45.771Z"}
length: 1
__proto__: Array(0)
time_interval: 1
updated_at: "2021-05-16T01:22:44.802Z"
__proto__: Object
```

As the console logged object shows, the entry and keyword association were successfully persisted and returned to the front end. This means the nested form was effective for both entry and keyword information. The app successfully coordinated between a Rails backend and JavaScript frontend to handle a nested form with one click!
