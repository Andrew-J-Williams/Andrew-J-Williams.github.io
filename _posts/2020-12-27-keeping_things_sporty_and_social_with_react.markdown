---
layout: post
title:      "Keeping Things Sporty and Social with React"
date:       2020-12-27 05:44:46 +0000
permalink:  keeping_things_sporty_and_social_with_react
---

It's been a long, challenging, and rewarding journey at Flatiron School. The push to the finish line has been filled with obstacles like COVID-19, challenges at work, hours of debugging, and many (yes, many) visits to Stack Overflow. Despite all the distractions and potential discouragement, I stayed focused and powered through my final project! Ever since I finished my JavaScript application, the wheels were already turning in my head on what my next project would look like, and I had a very strong vision of what I wanted: *create a social network for people who want to connect and join rec leagues*. While the current version doesn't contain nearly as many features found in sites like Facebook, Twitter, or Instagram, I hope to continue to build out this application even after graduation so it can see its full potential.

## I. Planning and Inspiration
I've always loved competing and playing sports, it's one of my absolute favorite things to do. Ever since I moved to Houston, it's been a blast meeting new people and challenging them to various games. All this jumping, diving, and running got me thinking creatively: what if there was a place people could find and form sports teams in their specific cities? I wanted to create an application that connected people through rec leages, making it easier to plan, communicate, and compete in whatever sport they are passionate about. It was this idea that led to the creation of Let's Rec!

The application would consist of 4 essential models on the Rails backend: (1) Posts, (2) Teams, (3) Users, and (4) UserTeams, with both Posts and UserTeams acting as joining tables to establish the association between Users and Teams. A Sessions controller is also present to authenticate a user's information when loggin in and out of the application. Similiar to my previous project, Explore Gettysburg, all the information retrieved from the backend is rendered in JSON so that later it can be converted into a JavaScript object for use on the frontend. I created some seed data consisting of users, teams, some userteam connections, and a few posts in order to make sure the correct information was displaying from the server side.

![](https://i.imgur.com/GC53nc9.png)

I will say I kept these models relatively simple for the scope of the project, keeping the total number of attributes for each model low with the exception of the Teams model. I didn't want to deal with an overwhelming amount of information when establishing the connection between the frontend and the backend, this way I could move on to developing different portions of the application at a steady pace without having to worry about where to arrange the data. But for the future, I definitely want to expand upon these models and add more attributes that capture more detail on both the users and the teams they are apart of. This desire to expand upon the project in multiple areas will be a running theme throughout the course of this post (so be prepared to hear about it a lot!).

## II. Utilizing React on the Frontend

Unlike my last project, which consisted of a frontend coded almost entirely in vanilla JavaScript, my final project required that I create my application under a React framework. We can define a JavaScript frameworks as collections of JavaScript code libraries that give developers the pre-written code needed for repeated tasks or features. Scott Morris in his [blog post](https://skillcrush.com/blog/what-is-a-javascript-framework/#whatis) on Skillcrush makes a great analogy for JavaScript frameworks through the comparison of coding to building a house: while you could set out and build a house and its parts completely from scratch, it would be an incredibly ineffient process and you would waste a tremendous amount of time. Instead, it would make more sense to build this house by purchasing materials that are already ready for construction as well as utilize schematics that show you where each part can serve its best purpose. In the same way, JavaScript frameworks like React allow developers to focus more of their time on enhancing the experience of the end user.

One example of how React differs from vanilla JavaScript comes in the way files are organized. If you take a look at the image below of my src folder, you will see 4 distinct groups: (1) actions, (2) components, (3) containers, and (4) reducers. Each one of these different file types allows developers to breakdown their applications into smaller, reusable parts.

![](https://i.imgur.com/yPRpUhb.png)

### Containers

React applications are built on components, and one of the most important kinds is the *container component*. Containers are often referred to as functional components, meaning they deal with handling date and state, acting as "parental" figures in the sense they can pass this data on to other components known as *presentational components* (which we will get to later). In my program, an example of a container component would be my Teams.js file. The primary responsibility of this file is to display information relating to the teams a user is a member of or teams a user may wish to join. Since there is a lot of moving parts in containers, we will break down the Teams container into smaller sections to better understand the job of each part...

#### Presentational Components

As mentioned earlier, presentational components are like children to the parental container components. We can see our presentational components in the Teams container below:

![](https://i.imgur.com/ToTMveD.png)

Presentation components, as the name suggests, primarily exist for the purpose of displaying the data retrieved by the functional component (in this case our Teams.js container). While the functional component are responsible for gathering and dispersing data, presentational components are the opposite in that they are simply given the data in the form of props with no knowledge of how to retrieve this information themselves. They are more concerned with how things look for the end user, hence the name "presentational". 

Let's take a closer look at one of these presentational components, specifically the one named TeamsPage:

![](https://i.imgur.com/e46ZWst.png)

If you look closely at the return statement towards the top, you can see that the application maps through some team data. This data has been passed down in the form of a prop from the Teams container, and we are able to access this information by calling **this.props.teams**. Besides iterating through this information for the purpose of displaying it a specific way, this component is not contacting the backend to retrieve additional data. It simply incorporates the information its given into an easy-to-read web page (along with the help of some css styling) for the end user. Speaking of our end user, the TeamsPage component will render the following for them when they visist the appropriate link (note: this is only one of the three presentational components rendered on the Teams.js page!):

![](https://i.imgur.com/ohjy5Io.png)

#### Actions and Reducers

While my application uses the React framework on the frontend, it also takes advantage of another JavaScript library known as Redux, which takes on the challenge of managing an application's state. In order to retrieve the right information, our React-Redux applications utilize actions and reducers. Actions are JavaScript objects that have a "type" field and are responsible for "describing" the events that occur within our applications (the [Redux website](https://redux.js.org/tutorials/fundamentals/part-3-state-actions-reducers) has a tutorials section that describes this information in great detail!). Reducers, on the other hand, are functions that take actions as arguments to manipulate the current state to create a new one. Together, actions and reducers allow our applications to retrieve and modify data from our API backend.

The Teams.js file imports an action called **fetchTeams**, which can be seen below from the file with the same name:

![](https://i.imgur.com/dSxJvna.png)

Here we can see that fetchTeams is making a fetch request to retreive all Team objects stored in our backend, utilizing a dispatch function for the purpose of (1) persisting state changes and (2) updating the HTML to reflect these changes. You will also notice that the dispatch function also includes the type **FETCH_TEAMS**, which originates within the reducer called teamsReducer as seen below:

![](https://i.imgur.com/T32ttpC.png)

Passing the type of FETCH_TEAMS into our dispatch function calls our teamsReducer and passes the action object to it. This reducer also contains a state containing mostly empty values, and this state gets passed into the reducer along with the action object. Since the type used retrieved all of the Teams from the backend, this information is used to set the value of manyTeams to be an array containing all the Team objects. Thus, the state is now updated to reflect this information.

But how does the information contained in the state get displayed in the frontend? Well, lets go back to the Teams.js file and look towards the bottom:

![](https://i.imgur.com/SbkVWc8.png)

React-Redux utilizes the **connect** function to connect (duh) a React component with the Redux store, and in this case our component is Teams. Connect accepts two arguments: mapPropsToState and the fetchTeams action from earlier. MapStateToProps is used to select the part of the data from the store (in this case the manyTeams array found in the teamsReducer) that the connected component will use. The fetchTeams argument is used to dispatch the specific action to the store, allowing the program to call action as a prop that can be executed inside the component. Together through connect, the Teams container is able to retrieve the array of information the presentation components so desperately want to display. 

### React and Repeat

The cycle that I just attempted to describe above is what makes React such a great framework for building applications. While the components of the program display different data, each follows the same pattern or flow when it comes to retrieving and displaying information. Once we understand what information we want to display, it just becomes a matter of building and connecting the appropriate actions, reducers, and components with each other. The components we create can be reused over and over to display information given our specific parameters. Put simply, React is pretty cool!

## III. Challenges and Triumphs

While React was a tremendous tool in creating Let's Rec, I definitely created (yes, all on my own) a number of bugs that tested my patience and challenged me to remain persistent. One of the biggest challenges I faced early on in the coding process was with my Teams.js file: whenever I clicked on a team to view its information, if I tried clicking the back button on the browser to view the Teams page again, an error message would display saying my program could not map through an undefined value. I found that upon refreshing the page, however, that the Teams container would successfully display the information that was not appearing before. The same problem would occur if I clicked the forward button on the browser as well for the TeamPage.js file.

Seeing as how a page refresh was the remedy to the problem, I decided to code a function called onLoad() in both my Teams.js and TeamPage.js files. The function can be seen below:

```
onLoad(){
        let myValue = true

        if (!localStorage.pageVisit){
            localStorage.setItem("pageVisit", myValue);
            window.location.reload();
        } else if (localStorage.pageVisit === "true"){
            console.log("You've been here!")
            window.location.reload();
            localStorage.pageVisit = false
        } 
    }
```

I would first define a variable called 'myValue' with the value of true. Since I was using localStorage to store information regarding the user logged in, I decided to utilized it for the purpose keeping track of when the user visited either Teams.js or TeamPage.js. In the case of Teams.js above, if the program checked localStorage and could not find an item called 'pageVisit', then the function would use setItem to create it and assign it the value of myValue, finishing off with a page reload. If the program did find pageVisit in localStorage and found its value equal to true, then the page would be reloaded and the value of pageVisit changed to false. The function found in TeamPage.js is essentially the same with the only exceptions being the value of myValue is inversed throughout (meaning it starts off as false and is changed to true later on).

To simplify the process that is happening, think of the function as a light switch that turns on one light while turning off a second. Each time the user switches between Teams and TeamPage, the application reloads the page so that the data will display properly without fear of a dreaded error message popping up. The function is set up intentionally with the browser back/forward arrows in mind. Being completely honest, while this function fixes the presentational problem, I'm not entirely sure if the reason its even returning an undefined value is due to something I have coded or not. If anyone reading this post wants to take a look at my repository and see if there's a bug I've missed, feel free to leave some feedback after following the link at the end of this article! I'm always open to constructive criticism and new insight.

## IV. CSS and Appearance

Finally, as any consistent reader of my blog will know, the part I had the most fun with was designing the way my application would look. Seeing as how I was going for a social media experience, I took heavy inspiration from Facebook, Twitter, and Instagram. I went for a minimalist design focused on modules that displayed particular information. For example, visiting a specific team's page would look like this:

![](https://i.imgur.com/vqueIE7.png)

One of my favorite features of the design is the little animations I incorporated into the icons in the header. Whenever a user hovers over an icon to select it, the image will rotate at a 30 degree angle, returning to it's original position after the user has taken their cursor away from it. I also love how almost all the buttons in the application change colors when the user hovers their cursor over them. Finally, I incorporated a conditional in the TeamPage.js file that checks the location of a team and returns an image of that specific location's skyline. Sure, these style choices may only exist to make the page "pretty", but in my opinion it goes a long way to enhance the end user's experience when interacting with the program's information.

Overall, I had a blast with my final project, and it's bittersweet seeing the finish line in sight. I have to admit, however, the application is pretty barebones even though it meets the project requirements put forth by Flatiron. That being said, this project has so much potential for future development and I can't wait to apply new knowledge towards making it even better. I want to add more features like a calendar, a score keeping system, a notification alert system and so on. I had the idea for this project ever since I finished my last one, and I plan on perfecting it well after I have graduated.

If you want to check out the repository for this project, follow this link: [Let's Rec!](https://github.com/Andrew-J-Williams/Lets-Rec)












