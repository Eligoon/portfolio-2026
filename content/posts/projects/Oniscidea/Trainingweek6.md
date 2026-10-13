---
title: Training Program Week 6
weight: 1
date: 2026-10-05
draft: false
---

## Week 6 - Testing the API.

**What did I add this week?**

This week I continued working on the REST API and focused on getting the different parts of the application working together. I reviewed the controllers and worked on the main application setup so that I could start the application in tests without changing the normal configuration.

I also set up automated API testing using Cucumber, Gherkin, Rest Assured and Testcontainers. This allows me to test the API by sending actual HTTP requests to the application while using a temporary PostgreSQL database.

I created Cucumber scenarios for creating users, profiles, training programs, training calendars and training sessions. All of these tests are now passing.

**Which new technology did I use?**

I started using Cucumber and Gherkin to write tests in a format that describes what the application should do in plain language. I used Rest Assured to send HTTP requests and check the responses from the API.

I also used Testcontainers to run PostgreSQL in a container during testing. This means the tests can use a real PostgreSQL database instead of relying on a separate manually configured test database.

I updated the Hibernate test configuration so that I could connect the test application to the database created by Testcontainers.

**Why did I structure it this way?**

I wanted to test the application through the same HTTP endpoints that a client would use instead of only testing individual methods. This helps verify that the controllers, services, DAOs, Hibernate and database work together correctly.

Using Testcontainers also makes the testing setup more independent from my normal development database. Cucumber lets me describe the expected behaviour in a readable way, which makes it easier to understand what each test is checking.

**What worked well?**

The biggest success was getting the Cucumber API tests working with Rest Assured, Javalin, Hibernate and PostgreSQL. I now have passing scenarios for creating users, profiles, training programs, training calendars and training sessions.

I also managed to resolve some issues along the way. One issue was that the training calendar endpoint used a different route than I initially expected. Another was handling Java date and time values in JSON and adjusting the test to correctly check the date returned by the API.

It was useful to see how the different parts of the application work together in a real request rather than only checking them separately.

**What was difficult or confusing?**

Getting the testing environment set up was more complicated than writing the individual scenarios. I had to make sure that Testcontainers, Hibernate and Javalin all used the same test database and that the test application could start and stop correctly.

I also had to understand how JSON responses represent Java date and time values, since the date was returned as an array rather than the string format I expected.

**What would I improve next time?**

Next, I want to continue adding tests for the more important relationships in the application, especially exercises connected to training programs and the logging of completed training sessions.

I also want to review the security part of the application when we cover it in the course. After that, I can continue checking the Google Calendar integration and clean up the project before finishing the documentation.
