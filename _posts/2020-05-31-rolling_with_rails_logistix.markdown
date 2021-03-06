---
layout: post
title:      "Rolling with Rails: Logistix"
date:       2020-05-31 18:17:07 -0400
permalink:  rolling_with_rails_logistix
---


Following in the footsteps of my pervious projects, I found inspiration in one of the most chaotic and stressful aspects of my job: freight management. The city of Houston, our region of operation, is a large and sparwling metro area, covering nearly 9,444 sq. miles. Those who call the city their home may commute a long distance for their job, but when it comes to getting the materials they need for their personal lives, they tend to stay within a small area of town. Thus delivery service becomes a crucial tool for our company in reaching the needs of those who may live anywhere from 50-70 miles from us. Keeping track of all these outbound orders, however, presents a challenge within itself.

Whenever I set out to create an application, I always think of making programs that are not only relevant to my own life, but could also be used by many other persons or businesses in their endeavors. From that start Logistix was born: an applciation design to organize and track a person's shipments. It would allow for a user to have a unique profile compromised of only their shipments, displaying ones in-transit as well as ones that reached their destination. If the user is using the application in a business setting, say with other coworkers, they would also be able to look through a collection of shipments in order to obtain limited information on their status. The assumption behind the program is that the index of all shipments belongs to just one company or group of people, and every company utilizing the program would have a different index tailored to their freight needs.

## I. Designing and Planning

Originally when I started planning my models for the project, I wanted to include 5 models: Users, Shipments, Services, Carriers, and Customers. The idea was that both Users had many Carriers and Carriers had many Users *through* Shipments. But as time went on, it became more apparent to me that for this application it made more sense for Services to have many Users through Shipments and vice versa. Whenever you go to UPS, FedEx, or any freight carrier, you don't necessarily have a personal account with them. Sure, some businesses may set up accounts that allow them access to special perks offered by these carriers, but more often than not an interaction with these carriers involves the purchase of a *service*, such as Ground or Priority, to send a package/shipment. Thus it became more apparent that while Carriers have many Services, the many-to-many relationship was taking place between the User and the Service through Shipments.

With this new concept in mind, I altered the relationships and did away with the Customer model altogether. While it is true that a User can have many Customers and that each Customer belongs (or is assigned) to a User, it felt as though it would be an unnecessary addition for the project given the time restraint I had imposed on myself. It would be a nice feature to implement when I revisit the program in my free time, but I settled on organizing all shipments under the name of the customer instead, treating the name as an attribute within the Shipment model. My final version of the application stuck with 4 models: Users, Shipments, Services, and Carriers.

## II. Coding the Basic Framework

My framework was in place and I got to coding. On a sidenote, if you are a fellow student struggling to know where and how to begin your project, I highly recommend checking out the videos found on Learn Instruct, they were immensely helpful for me when I started my Rails project. Also, a tip for saving time, using ***rails g resource*** is a huge time saver when it comes to creating an MVC application. Below is an example of a command I used when making my application:

`rails g resoucre Carrier name:string phone:string pending_shipments:integer delivered_shipments:integer`

This simple action creates a migration, a model, a controller, routes, and more for our Carrier class. It's an incredible tool! Just pay attention to the details you are entering, you might find yourself in a world of hurt if you left something out or inputed something incorrectly. I utilized the resource generator to create the other classes as well. Furthermore, I had to create a Sessions controller and View files in order to enable sign-up, log in and log out fuctionality.

From there the task became relationship connections, of which the following were developed:

* **User** - has_many :shipments, has_many :services, through: :shipments (1st has_many through:)
* **Service** - has_many :shipments, has_many :users, through: :shipments, belongs_to :carrier (2nd has_many through:)
* **Shipment** - belongs_to :user, belongs_to :service (this is our joins table that is user submittable) 
* **Carrier** - has_many :services

While the rails generator helped provide the routes for all my generated classes, I had to create my own routes when working with my Sessions controller. The most fundamental part of my application would be the user's ability to access it, thus it was crucial to take care of these routes and views before delving into the creation of any other part. The routes for my Sessions control ultimately looked like this:

![](https://i.imgur.com/kw1RWxY.png)

One point to note is that I chose to create a dyanmic route for my Omniauth functionality, allowing my pogram the ability to accept multiple sources for log in if I user chooses to create an account through a 3rd party. For now, the only 3rd party my application allows for log in is Google, but as mentioned before, I hope to spend more time in the future adding more sources to my applcation. 

Another crucial feature I chose to implement were several helper methods defined in the Application controller. You can see them in the image below and well as the comments describing their functionality:

![](https://i.imgur.com/aA05az3.png)

The purpose of utilzing helper methods is to cut down on the logic that takes place in the Sessions controller, keeping our applications DRY and efficient. These helper methods are used as security measures throughout the application; keeping logged out users from accessing someone's private page or logged in users from viewing information not tied specifically to their own account. If you check out some of the controllers in my application, you will notice a bit of code called ***before_action***, which in the context of the program looks like this:

```
class ServicesController < ApplicationController
    before_action :redirect_if_not_logged_in
		
...

end
```

The before_action is a filter that in this case runs a method before the execution of a controller action. In cojunction with this helper method, if the program determines the current user does not have an active session on the application, they are redirected back to the log in screen. It doesn't matter how many pages they attempt to access through this controller; unless they are logged in they will not be able to view site information. 

## III. Testing and Breaking

I've found that coding is a constant process of breaking and repairing, and just because the program is deemed as "broken" in the testing process does not make anyone's efforts a failure. On the contrary, with Ruby on Rails you often find solutions and directions on where to proceed when you run your server. Everytime I created a method in whatever controller I happened to be working in, I would always run my server, hit refresh, and check to see if the code I wrote was producing the result I needed. If I had just chosen to completely code out the components of my application and had waited until the end to see if it worked, chances are incredibly high that my errors would have compounded themselves and I would have been bogged down in debugging for hours on end. 

One frustrating error I ran into was attempting to implement Omniauth in my program. After I had gone through the process of entering all the necessary code for a Google Omniauth, along with generating the client ID and secret, I kept receiving an error that my log in was invalid. This puzzled me immensley, I knew that I was testing the application with my own Google account, and that after doing some review/watching some walkthroughs I had followed all the correct steps in setting up Omniauth functinality. I then had a moment of realization: perhaps one of my validations under the User model is creating an issue with the log in. Sure enough, I had originally set up a validation for the password attribute to have a maximum amount of characters, and my Google password was just over the limit. After modifying this validation, I tried again and found success!

## Design and Aesthetics

Moving away from the coding aspect of the project, I found inspiration for the look of my application by exploring the websites my company typically uses for delivery service: FedEx and Dugan. After going for a warmer tone with my previous project, I decided to go for a more earthy vibe and went heavy on greens and blacks. I took the color scheme from Dugan and the general interface from FedEx and merged the two together. Below are some screenshots of where the project is currently at, I refrain from saying finished because yet again I still want to sweeten things up in my spare time to get the site where I want it to be. Below is the log in screen that greets users before they access their accounts:

![](https://i.imgur.com/D1CJRBS.png)

Once they have signed up or logged in, they will be greated by their homepage:

![](https://i.imgur.com/G8m8ymJ.png)

One minor design feature I really loved with this application was adding the tiny Google logo on the link to log in with a Google account. It adds a little authenticity to the program as well as some sleekness. I also kept the transition animations from my last project in this one because I just love the how interactive the application can be when you hold the cursor over different objects.

## Conclusion

Making the jump from Ruby to Ruby on Rails was a bit of a challenge, and to say I am a master of all things Rails would be an overstatement. I can say for certain that Rails does add that "magic" element to the coding process: it takes essential Ruby knowledge and packages it in a form that allows the programmer to create applications more efficiently. I was able to get the basic components of my project up and running far faster than I was when I was working on my Ruby project. I can now better understand why the curriculum was formatted the way it was, giving an opportunity to understand how the different parts work under the hood. So far this has been my favorite project, I can't wait to revisit it and implement more features like a search bar and a customer database!







