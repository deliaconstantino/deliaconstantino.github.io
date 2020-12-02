---
layout: post
title:      "'Decoding My First CLI Data Gem'"
date:       2020-12-02 00:02:57 +0000
permalink:  decoding_my_first_cli_data_gem
---


I just finished coding my first Flatiron Project: CLI Data Gem. Throughout this process, I struggled most with deciding on a topic and structuring the code. To help make my choice, I started by talking with a friend about things that interested me — travel, design, food/recipes, etc. 

Eventually, I decided it'd be cool to make a gem about road trips through the US. I've always wanted to go on the kind of road trip where you leisurely drive across country, maybe even on a few backroads instead of highways, and stop a number of times at all the notable sites — big and small — along the way. Well, I still haven't gone on that road trip (definitely want to!), but I figured writing a data gem on the idea would be a GREAT way to vicariously live out that idea. And when I eventually have the time to do it in person, I'll have a head start on planning.

Now, with an idea established, I needed to find a viable website for scraping the necessary data. I did a few Google searches, looking for a website that satisfied certain requirements:

a) The website needed to provide a list of road trips as well as their names and some information — this information was easily apparent from a quick glance at the webpage.

b) The website needed to be scrapable — I used the Flatiron-provided ScraperChecker tool to verify that using Nokogiri and open-uri WOULD return a Nokogiri object.

c) The final thing to check was whether the website had their HTML nested in an organized way. By 'inspecting elements' (hovering over various parts of the webpage, then right-clicking and selecting "inspect" on the dropdown menu) I was able to access this. After reviewing a few webpages with less-than-scrapable data, I settled on [roadtripusa.com](http://roadtripusa.com) . This one had an organized way of laying out the information for each specific road trip. They had a number of `<div/>` tags nested inside one another, including a set of `<div/>` tags with a class of `".col-md-4"`. Using the 'elements' pane to look through the HTML, these particular class descriptors appeared to be unique from the rest of the site. They also contained all of my app's necessary information, nested deeper within their tags.

After a good bit (several hours!) of playing around in binding.pry, I had successfully narrowed down which css selectors to use in order to scrape and assign specific bits of data. But here, things got dicey again. I realized I was VERY used to solving "labs" with tests, and therefore passing each subsequent test to help grow my understanding of how each part worked. This process of planning a CLI from a blank state and designing object relationships myself, was brand new. So I went back to the drawing board (literally) and used the suggested [draw.io](http://draw.io) tool to map out my object relationships on paper with symbols and arrows. I was amazed at how much this process helped synthesize my own understanding of the class relationships. Using this tool, I was able to follow the arrows to plug in pseudo-code marking which methods called on methods in other classes and how all three of my classes interacted. 

As I worked through getting these classes to output the information I wanted, I alternated between running my program from the terminal with the bin call and using binding.pry to experiment with specific data. Binding.pry was especially helpful for a couple of `#each` methods that were used in assigning the scraped data. By using binding.pry, I was able to go through each iteration, one at a time, to see each one's specific output — and fix any that were not correct. Finally, running the bin file from the terminal was my last step to make sure that each route had all of it's information as well as verify that all of the CLI loops were looping as desired. 

My biggest takeaways were related to process and mindset. I learned to not see a blank page or open-ended project as frightening. Going forward, I'd like to think of it as an opportunity, rather than an abyss. I was reminded of the power of taking a step back when stuck. Approaching the problem from a different angle provided clarity and helped my understanding grow.
