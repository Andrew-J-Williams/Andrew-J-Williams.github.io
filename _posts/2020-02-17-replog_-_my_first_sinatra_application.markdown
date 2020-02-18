---
layout: post
title:      "RepLog - My First Sinatra Application"
date:       2020-02-17 19:51:15 -0500
permalink:  replog_-_my_first_sinatra_application
---


I work in the hardwood flooring industry, and one of the most demanding parts of my job is generating new business for our company. In order to grow, you have to be able to keep track of your clients and their flooring needs. Technology has played a crucial role in allowing us to keep track of this information, and for my first Sinatra project I wanted to create a web-based application capable of storing unique customer information for numerous users. Thus, RepLog was born: a simple yet effective CMS for keeping one's business contacts in one place. 

## I. Design and Planning 

Before creating any sort of repository, I took the time to breakdown the fundamental jobs my application needed to perform in order to properly work. The steps were as follows:

1. Program loads a main page with the option to log in or create an account.
2. The user can access either option and provide the appropriate information to access their account. 
3. Once logged in, the user will be able to view a list of customers specific to their account.
4. Users will also have the option to create new customers or edit/delete existing customers.
5. Finally, once their work is done, the user can log out with the confidence their information is saved. 

Once these basics were solidified, I started constructing the MVC structure of my Sinatra program. Below was the final result:

![Sinatra Structure](https://i.imgur.com/MeHygJf.png)


Given the requirements of the project, my application would have a user-customers relationship, where a user *has many* customers and a customer *belongs to* a singular user. Our user database will have 3 attributes (all of which are necessary for identifying a particular user's account): a username, an email, and a password. The customer database, in contrast, contains 6 attributes: name, address, phone number, the date the connection was made, a short note, and finally a user id. The "user_id" attribute will play a crucial role in connecting our two databases together, tying each customer to a particular user. 

Following this setup, I knew I would need 2 models for my user and customer classes. From there I also knew my two classes would need their own controllers, both designed to handle the routes specific to each as well as inheriting basic Sinatra functionality from my third, main controller: the application controller. As for my views files, I created a basic view called "index" that would essentially act as my homepage for the application, just like with any web-based application that has a log in screen. I kept the user views relatively simple with just 2 files: one for existing users to sign in and one for new users wanting to create an account. The customer views would contain the most files with 4 in total: a file to view a specific customer's profile, a view for creating a customer, a view for editing an existing customer, and another "index" view for displaying the current user's profile, complete with a personalized customer list and the option to either create a new customer or logout. 

With the basic framework built out, the final yet critical component was organizing where my css files would be stored. I've always loved the artistic side of desinging websites, and for me one of the most make or break thing about any application is how it is presented to the user. For this, my final view file titled "layout" exists for the purpose of establishing a general HTML framework that will be used across each page of the application. I then made a "public" folder for storing my css files, where each element received specific design alterations. I probably spent more time on the aesthetics of my program than I should have, but it was a tremendously satisfying feeling to see different elements transform into something far more engaging than just the default settings. 

## II. Coding the Basic Framework

With my basic structure already planned out, the job of coding became exponentially simpler. While it has always been continually stressed in different lessons, there's just no subsitute for planning *before* you actually code. In the end you will save more time and avoid the headaches/heartaches that come with disorganization. Knowing this, it should also go without saying that before any specific elements of your application are coded that your program's configuration and environment files are created and connected. This includes making a config folder, a gemfile, and a rakefile. Since nailing down the initial stages of a project can be daunting, I gathered tremendous knowledge from reading this article by Flatiron School that guides you through the process:  [How to Build a Sinatra Web App in 10 Steps](https://flatironschool.com/blog/how-to-build-a-sinatra-web-app-in-10-steps).

As mentioned earlier, once my environment was setup and functional, the task of coding each part of the program was relatively straighforward. That being said, there were two aspects of the project that proved challenging: (1) authenticating a user's sign in information and (2) determining if a certain user was signed in. For the first challenge, I had learned in previous lessons that the following code could be used to determine whether or not a user had actually entered information in the sign up form or just left it blank:

```
if params[:username] == "" || params[:email] == "" || params[:password] == ""
redirect to '/signup'
```

While the code above does work, the amount of repetition does not make for the most clean and efficient formatting. Taking some time to do outside research, I discovered a secondary solution that involved the use of Active Record Validations. These validations essentially take a look at the information the user is inputting and evaluate the state of the object before inserting it into the database. As a result, I was able to code the following validation into my user model versus the user controller: 

![Validation in ActiveRecord](https://i.imgur.com/PNoe4yO.png)

There are two validations taking place: first, the program is checking the user's inputs for username, email, and password to see if there are valid values (hence the use of "presence"). The program will then evaluate the inputs as either true (if values are present) or false (if no values are present). Beyond the "presence" evaluator, the "uniqueness" evaluator is also used to determine if the username and email inputted by the user is new or if it already exists in the database. If either or both already exist, then the program will prompt the user to enter different information in. Thus when it comes to time for the controller to process the sign up request, we can use "@user.save" in our conditional statement to see if the program recognizes unique and valid values: 

![User Controller Example](https://i.imgur.com/SWTKULc.png)

As for determinig whether a certain user was signed in or not, I implemented two helper methods in my Application Controller. Here is the code I used:

```
helpers do

    def logged_in?
      !!current_user
    end

    def current_user
      @current_user ||= User.find_by(:id => session[:user_id]) if session[:user_id]
    end
		
end
```

The "current_user" method first checks to see if "@current_user" has a value or is undefined. If it has an assigned value, then the program has no need to evaluate the second part of the equation following the conditional assignment operator("||="), returning "@current_user" as its final value. However, if the value of "@current_user" is undefined, then the program searches the user database for a user id equal to the one stored in the session, if there is a session value present to begin with, and sets "@current_user" equal to that result. We then take the result of the "current_user" method and plug it into our "logged_in?" method to determine if that user is logged in to our application. We do this by using the Double Bang operator (!!), which converts the "current_user" value to boolean, returning true or false depending on if session information is present. 

## III. Testing the First Draft

After I completed all the appropriate coding, I used "shotgun" in my terminal to run my application. From there I ran through all the steps of the program, ensuring the controllers were properly connected with the correct view files. It's a good thing I did, because I very quickly learned a lesson in being detail-oriented. While the logic of my code often made sense, silly typos and incorrect values created errors in the testing process. There were definitely some times I was in disbelief, spending 10-15 minutes beating my brain over an error so small and obvious that I couldn't help but be a little embarrassed. 

But with this period of frustration came immense satisfaction, though the two feelings seem far from mutual. While getting stuck can be discouraging, taking time to reflect on what you've learned as well as just taking a break can be one of the most mentally healthy things any coding student can do. If you get too sucked in and discouraged on a particular error, you may find yourself stuck in a continuous cycle of frustration and stagnancy. Trust me, I am a living example of this sometimes. Only when you take a break or ask for help do you begin to think differently, and I found doing both were crucial for finishing this assignment. 

## IV. Adding a Little Flair

Now that I had successfully completed my testing of the basic application, the time I had gained from planning ahead gave me the opportunity to create two css style sheets. While the final product for now is not perfect, I believe giving the application a simplistic and clean appearance cuts down on confusion from the user's perspective. Below is a screenshot of my applications homepage:

![RepLog Index Page](https://i.imgur.com/0lfTXzb.png)

One of my favorite parts of the css design was creating buttons that were interactive when the user hovered their cussor over them. Though a small element, the simple animation gave my application life and immediately made the interface more intersting to navigate. I also loved incorporating some touches from my industry, including the herringbone pattern in the background and the little tree logo on the main page. I can't wait to explore more options and design techniques for building future applications!

## Conclusion

Using Ruby and Sinatra to create a web-based application has been a fun challenge. I love being able to use "shotgun" to view changes to my application in real time, providing visual insight into what changes need to take place in the code itself. Understanding the relationships involved in an MVC application opens the possibility of creating other programs that solve real world problems. For me, RepLog tackles the issue of customer assignment among multiple employees. I know my project could still benefit from adding more features, so I hope to revisit it in the future once I have built up my knowledge of web design.

If you want to check out my project for yourself, simply follow this link to my repository:
[RepLog](https://github.com/Andrew-J-Williams/replog)
