---
layout: post
title:      "Sharing a Sinatra Experience"
date:       2021-02-10 05:15:39 +0000
permalink:  sharing_a_sinatra_experience
---


As I neared the end of my Sinatra project, one of the most exciting parts was refactoring my code. In the beginning of my project, I was concerned about how I would receive certain pieces of user data in a consistent manner from all users. To ensure standardization of the necessary data in new user entries, I decided to use dropdown menus in several fields of my form field. This meant that users could only choose the best match from the options given, rather than input information in any form they liked. This, in turn, allowed my tables to have uniform, matching entries in their cells. 

As I neared the end of my project, I was happy to see that the dropdown menus seemed to be working. On the user side (what could be seen in my browser through the Shotgun gem), my app was looking clean and organized. However, looking behind the scenes in the views, my html.erb view files were long, and filled with repetitive code that was very easy to get lost in.

![]([Imgur](https://imgur.com/KiUB08Y))

As you can see in the above image, the only thing that changes in each option of the top `<select>` field (`fertilization_schedule[value]`) is the value itself.  Luckily, with the use of ERB and the ever helpful Ruby `each` method, the code was able to be cleaned a bit.

![]([Imgur](https://imgur.com/lGUjYXJ))

For the first `<select>` field in `fertilization_schedule[value]`, I created a range from 1 to 10 and iterated over each of those numbers. I used the ERB tag `<%= %>` to place the current value of `i` inside the value attribute and also to have it render in the html itself. This ERB tag (`<%= %>`  ) works very similarly to string interpolation; the value of the evaluated Ruby code inside the tag is rendered as html in the view. The code containing the parts of the `each` method that should be evaluated as Ruby code but NOT rendered as html were placed inside this ERB tag `<% %>` , which evaluates Ruby code but does not output the result.

I was able to follow the same process with my `fertilization_schedule[frequency]` `<select>` field. In this case, I first created an array containing the values I wanted to output: `["day", "week", "month", "season"]`. I then iterated over this array, once again using ERB `<%= %>` to "interpolate" Ruby values I wanted to have rendered and `<% %>` to signify code that should merely run as Ruby for the proper functioning of the code. 

![]([Imgur](https://imgur.com/35gq3LW))

In the end, the user side of the experience remained the same. The dropdown menus continued to function properly. They served both to guide users about what data to input, and regulated the data that the app received. Behind the scenes however, the app now runs in a slightly more dynamically coded and responsive manner.
