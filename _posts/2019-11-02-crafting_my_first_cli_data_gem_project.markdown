---
layout: post
title:      "Crafting My First CLI Data Gem Project"
date:       2019-11-02 15:25:37 -0400
permalink:  crafting_my_first_cli_data_gem_project
---


When it came time to design a project based on my knowledge Ruby and object orientation, I did not have to look very far for inspiration. As of this post I am currently working full-time as an assistant manager at a hardwood flooring store, and my time with the company gave me the idea to design a program centered around my experience. I wanted to make a very simplistic yet informative application that showcased the kinds of products I'm tasked with presenting and selling on a daily basis.

At first I was a little overly ambitious in determining what I wanted my program to do. My work had given me so many ideas: it could provide information regarding specific flooring products, it could handle advanced calculations such as total square footage plus waste, and it could provide detailed information regarding each store location. I spent so much time fantasizing about what I program would do that I got behind on actually coding any of it. One of the many lessons I learned through the development process was learning to focus and be efficient with my time. Whether you have a plethora of ideas for your first project or not, I found that sticking to a basic, clean structure was the best way to complete my application. Once you have the essential structure in place, then you can have a little fun and build advanced functionality. 

In designing my project, I found that good 'ole-fashioned pen and paper mock-ups helped visualize the structure of my program. The structure went something like this: 

1. Greet the user and display relevant information
2. Off the user one of two choices on what information they want to view
3. Return the information based on the user's input
4. Offer the user a chance to return to the starting point and view more infromation or close the program

Once I had a clear vision of what I wanted my project to do, I broke down what classes would make up my code:

1. CLI - a file responsible for creating the 'path' the user will take to viewing the information
2. City - an object containing a name, inventory list, and a contact list
3. Products - an object containing a name,  a city name, a quantity, and a price
4. Contact - an object containing an address, a city name, and a phone number
5. Site Scraper - a file that will scrape information and utilize other classes in the program

Having the necessary structure in place for my project, I proceeded to create my project repository on Github (titled 'Flooring Gem') and got to coding. I designed my 'City' class to retreive information from both the 'Products' and 'Contact' classes. I created the attributes of 'contact' and 'inventory' and set them equal to empty arrays, then, utilizing the code from my 'Site Scraper' class in conjunction with 'Products' and 'Contact', I pushed the relevant data into each attribute's array. For example, when my program initializes a 'City' object, it will appear as follows:

#<FlooringGem::City:0x00000000026e8dc0 @name="Houston", @contact=[], @inventory=[]>

But after utilizing my *'.check_inventory'* method on this object, the following information appears when I check my object again:

#<FlooringGem::City:0x00000000026e8dc0 @name="Houston", @contact=[], **@inventory=[#<FlooringGem::Products:0x0000000002603450 @name="5″ x 3/4″ Hickory Boulder Handscraped", @city=#<FlooringGem::City:0x00000000026e8dc0 ...>, @quantity="701 sqft", @price="$6.49 sq/ft">]**

As can be seen above, the 'inventory' attribute now possess the attributes of my 'Products' class, which is also specifically tied to this 'City' object as can be seen at the '@city' attribute. I designed my 'Products' and 'Contact' classes to receive the argument of the city object whenever either was initiliazed. This is the beauty of object orientation, it allows many classes to work together to accomplish certain goals! The same thing can be seen if we use the *'.get_contact_info'* method on the city object, returning the following: 

#<FlooringGem::City:0x00000000026e8dc0 @name="Houston", **@contact=[#<FlooringGem::Contact:0x0000000002ff5fd0 @address="565 Garden Oaks Blvd Ho
uston, TX 77018", @city=#<FlooringGem::City:0x00000000026e8dc0 ...>, @phone=" (713) 333-5999">],** @inventory=[#<FlooringGem::Products:0x0000000002603450 @name="5″ x 3/4″ Hickory Boulder Handscraped", @city=#<FlooringGem::City:0x00000000026e8dc0 ...>, @quantity="701 sqft", @price="$6.49 sq/ft">]

Establishing these object relationships helped capture specific data regarding each flooring store's location. It allowed my program to create a collection of unique locations, each with specific information unique to that shop, that I could iterate through and display to the user. In the end, the program is relatively simple and does not do too many complex things. If the user wishes to view the contact information for the Houston shop, after they've followed the on-screen commands the following will output to the terminal:

***Address: 565 Garden Oaks Blvd Houston, TX 77018
Phone: (713) 333-5999**

This information is retrieved from our city object by use of the '.each' method. 

Despite the program's relative simplicity, I absolutely loved working through the process from start to completion. I definitely had my moments of frustration whenever the error messages and failures appeared, but yet another lesson I learned through the process was staying both positive and persistent. It gave me a chance to do my research, review previous lessons, and gain a greater understanding of why my code was running the way it was. Is my final product perfect? Deffinitely not, but I am certainly open to receiving constructive criticism for how I can improve my program! This definitely will not be the last time I visist my 'Flooring Gem' repo, I want to come back and see what kind of additional features I can add as I continue to learn new things.

If you want to check out my project and clone it for use, check out the link below:
https://github.com/Andrew-J-Williams/flooring_gem/tree/master/lib/flooring_gem

