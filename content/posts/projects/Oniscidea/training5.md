---

title: Training Program Week 5
weight: 1
date: 2026-09-28
draft: false

---

## Week 5 - Building the API.

**What did I add this week?**

This week I started building the actual REST API for the project. I created DAOs, services and controllers for the different parts of the training system, including users, profiles, training programs, exercises, training sessions, calendars, exercise logs, set logs, notes and body parts.

I also started working on the relationships between the entities, for example connecting exercises to training programs and exercises to body parts. I set up the routes in Javalin so that the different parts of the application can be accessed through API endpoints.

I also began setting up the Google Calendar integration. I updated the Google Calendar connection entity and DAO, as well as services for Google OAuth and interacting with the Google Calendar API.

**Which new technology did I use?**

I worked more with Javalin for creating the REST API and used DAO, service and controller layers to structure the application.

I also started using the Google Calendar API and Google OAuth for connecting the application to a user's Google Calendar. I used Google's Java libraries to handle authentication and calendar events.

**Why did I structure it this way?**

I wanted to separate the different responsibilities of the application instead of putting everything into the entities or controllers. The DAOs handle the database, the services contain the actual logic, and the controllers handle the HTTP requests.

This also makes it easier to add more functionality later without having one large class responsible for everything.

For Google Calendar, I wanted to keep the local database as the main part of the application while using Google Calendar as an external service that the application can communicate with.

**What worked well?**

I feel like the project has started to become an actual application rather than just a collection of entities. Having the different routes, services and controllers makes it much easier to see how the different pieces connect.

I also liked being able to build the project one resource at a time. Once I had one working structure, I could use the same general idea for the other resources and slowly build the API up.

**What was difficult or confusing?**

There are a lot of different layers and classes now, and sometimes it can be difficult to remember which responsibility belongs where. I also found the relationships between the entities more complicated once I started actually implementing them instead of just planning them.

The Google Calendar integration was also more confusing because it introduced OAuth, access tokens, refresh tokens and callback URLs, which are quite different from the normal CRUD functionality I had been working with.

**What would I improve next time?**

I would like to spend more time understanding the flow between the controller, service and DAO instead of just getting the code to work. I also want to understand the Google OAuth process better, especially how the authorization flow and state work, so that I understand what is happening instead of just following the structure.

I would also like to become more comfortable with how the different entities and their relationships are handled by Hibernate.