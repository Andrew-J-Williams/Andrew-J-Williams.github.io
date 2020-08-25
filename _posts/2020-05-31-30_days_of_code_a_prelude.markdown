---
layout: post
title:      "From the Back to the Front: Exploring History with Javascript"
date:       2020-05-31 23:34:25 -0400
permalink:  30_days_of_code_a_prelude
---


I may have bit off a little more than I could chew with my 4th project if I'm being completely honest. That being said, I can say with certainty that this project was by far the most satisfying to complete to date. Given the requirements for the project, I was strongly encouraged to make an application around something I would be excited to talk about. I didn't have to think very hard on that one, and in an effort to challenge myself, I decided to create an SPA that focused on telling one of the most important stories of the Civil War: the Battle of Gettysburg. Sure, it may have been an obvious choice when looking at critical events from the war, but being a history buff (as well as having a wife who majored in history) I just couldn't help myself.

## I. Project Planning

I knew from the very early stages of planning my project that I wanted my application to have 3 central features:
1. A map with various markers that users could click to unveil information concerning that location.
2. A discussion section where users could post information/debate among each other concerning various events.
3. An interactive, critical thinking section that would give users the choice to make important battle decisions.

This was definitely a first for me: while my previous projects took inspiration from my daily work experience, I would be in uncharted water making an application with a relatively custom layout and features. As a result, my application class structure initially looked like this:

![](https://i.imgur.com/JVk0nPQ.jpeg)

I created a "User" class to represent the person interacting with my site, and I planned for them to have an account that would grant them access to additional features on the site. To set the stage for my 3 modules mentioned above, I created the "Scenarios", "Events", and "Comments" classes to handle the information assocaited with the different parts of the program. I included a "User_Choices" class to capture the data entered by the user when interacting with the critical thinking section, mainly for keeping a running tally of which users selected a certain option. While all these classes would remain in a somewhat similar form by the end of my project, I did find myself adding a "Reply" class about halfway through in order to add greater functionality to my dicussion section. After all, what good would I comment section be if you couldn't respond directly to another user's comment?

Despite taking about 2-3 days of planning after work, I would discover as I got further into my project just how important it is to have a plan going forward, even when you feel like you are so ready and excited to put your ideas into code. While I had my basic structure in place, I found that many flags would pop up along the way, mostly revolving around relatively small portions of the bigger sections. Sure, I had a grand vision for what each section of my application was supposed to do, but when it came to the finer details on just how I would make those sections work, I found myself repeatedly hitting roadblocks during my coding sessions. If there was obvious takeaway I had from the first week of coding, it was reminding myself to slow down and put myself in the user's shoes, making sure small glitches wouldn't come back to haunt me after I wrote a myriad of functions.

## Coding and Creating Connections

I spent most of my first week setting up my folders for the frontend and backend of the project. I utilized my knowledge of Rails to generate all of my classes, models, and controllers. I wanted to make sure the connections were working early on, so I mainly utilized my index file in the frontend to display the data for each class as I confirmed they were working appropriately. Really the biggest difference in keeping Rails primarily for the API on the backend was remembering to render all the information for each class in **JSON (aka JavaScript Object Notation)**, which a syntax for storing and exchanging data. JSON gives developers a way to store information and access it in a relatively easy manner, and it plays an essential part in displaying information in Javascript applications.

Take for example this section of code from my comments controller:

![](https://i.imgur.com/PUJ9it6.png)

As you can see, unlike an application written primarily in Rails, you will notice the *'render json'* code towards the bottom of each method. This code will take the information contained in our *'@comment'* variable and "JSON-ify" it, allowing our Javascript frontend to utilize the information later on when we want to display our historical knowledge to a user (or in this case, one of our user's comments). In order to do this, we use the *'fetch'* to pass in a particular url and extract our JSON information from the backend API. You can see this at work in my 'fetchComments' function from my project's frontend: 

![](https://i.imgur.com/vkBigPG.png)

I tried followed the advice given in the project planning section to work 'vertically' and not 'horizontally'. The basic idea here is to envision each feature of your program as a column on a grid, with the rows representing all the steps one must take to get this feature working. You choose to focus on getting each part of the column up and running before deciding to tackle multiple columns at once. For example, with my comments class, I chose to focus on getting all the steps needed for my 'show' method working before I decided to tackle my 'create' method. Had I chosen to jump back and forth between coding certain features for each column at once, I could have experienced layers of errors as well as unneccessary lines of code.

## Challenges and Triumps

While I stressed the importance of working 'vertically' versus 'horizontally' in the last section, I certainly didn't follow these rules perfectly. In many cases, I felt that strong temptation to knock out multiple features at once, especially when it felt as though the code was becoming repetitive across different classes. For example, when I realized later on in my project that it would be useful to include a 'Reply' class to work in 'belongs_to' relationship with my 'Comment' class, I tried to make up for time by simply copying and pasting large sections of code from my comment sections and modifying certain key words. However, when it came time to run my tests to make sure data was properly displaying, I found myself bogged down in debugging simple mistakes that would make a world of difference when fetching replies. One example was forgetting to code a way of setting a reply's 'comment_id' to equal the id number associated with the parent comment. So come when it came time to display a comment's replies, since no comment_ids were present, no replies displayed!

The problem mentioned above was just one of the many challenges I encountered while building out my Reply functionality. There's just certain features associated with comments and replies you don't think about until you actually see your program in action. Like how will you differentiate between comments/replies you post versus what other users post? How will your program know how to reply a reply associated with another reply under a specific comment? Many of these small questions really slowed down the coding process, and quite frankly, mentally wore me out. But thankfully, choosing to think positive, take scheduled mental breaks, and spending time with my wife and son gave me the energy and motivation to push through and knock out each section.

One of a my favorite parts of the project was getting all the markers on my mini-map assigned to specific events. At first it was a challenge: how to I coordinate each marker to display the assigned event when the user clicks it? I was able to accomplish this task by creating a variable for each marker and then putting them all in an array, which I also defined as a variable: 

![](https://i.imgur.com/ycGix11.png)

I then created several functions that would work together to display the appropriate information:

![](https://i.imgur.com/QLRenTU.png)

Here's a brief breakdown of what is happening: first, my 'assignMarkers' function takes in the argument of an array (in this case the array of markers I defined earlier in the program) and takes each element of that array and passes it into my 'markerSelect' function. This function is tasked with adding an event listener to each marker that, when clicked, executes my 'fetchInformation' function which, once again, takes in the marker as an argument. Now 'fetchInformation' is where most of the heavy lifting takes place, it's where we take the index of the particular marker (adding 1 to it so we don't have a 'Number 0' marker) and retrieve our particular event associated with that index number, using a 'clearContainers' function before hand to remove any information from our containers that was associated with a different event. Finally, I included a conditional to check and see if the user was logged in, just to make sure certain information remained hidden. 

While it may not have been the most complicated process I had to code for the project, it was certainly one of the most satisfying to get working!

## CSS Styling and Final Touches

As I coded the logic for each feature of the program, I had an incredibly enjoyable experience figuring out how I was going to design the look of my application. Since I was going to tackle a subject from the Civil War, I wanted my application to have a formal look and a specific color scheme. I took inspiration from the [American Battlefield Trust's](https://www.battlefields.org/) website and sought to emulate the look and feel. This was the final result:

![](https://i.imgur.com/WV0nWZ4.png)

While it's far from perfect, I really enjoyed keeping the different features of the program packed within their own little boxes, displaying information in a way that hopefully doesn't overhwhelm the user too much. Albeit a small feature that not many people may notice, I was really proud of the transition I was able to incorporate when the user logs into their account. The login box transforms upon the user clicking the 'Log In' button, changing colors to reveal a message that welcomes them by their specific username. I spent more time then I probably should have adding small details with css, but making a visually appealing application brings me just as much joy as coding logic behind the scenes does!

If you want to view the repository for this project, follow the link below:
[](https://github.com/Andrew-J-Williams/Explore-Gettysburg)

