---
title: Training Program Week 8
weight: 1
date: 2026-10-19
draft: false
---

## Week 8 - Reviewing API Flows, Expanding Security Tests and Integrating Google Calendar.

**What did I add this week?**

This week I continued improving the REST API by reviewing the existing API flows and checking that the different resources work together correctly. I looked at how users, profiles, training programs, training calendars, training sessions, exercise logs and set logs depend on each other.

I also worked on the security and integration tests. I added Cucumber step definitions for set log security, covering scenarios where a user creates or retrieves their own set logs and attempts to access resources belonging to another user. The test setup creates users, logs them in, obtains JWTs and creates the related profiles, calendars, training programs and training sessions needed for the scenarios.

I reviewed the API routes and how authentication is applied to them, including which endpoints must remain public and which require a valid JWT. I also continued checking the flow from creating a training program and scheduling a training session to recording exercises and sets.

Another change was updating the normal Hibernate configuration from `create` to `update`. This allows existing database data to remain in place when the application restarts, while the test EntityManagerFactory continues to use `create` for test setup.

Finally, I continued implementing Google Calendar integration. I wired the OAuth service, calendar service, connection DAO and controllers into the application and added routes for starting the connection, handling the callback, and creating, updating and deleting Google Calendar events.

**Which new technology did I use?**

I continued using Cucumber, Gherkin, Rest Assured and JUnit to test the API through HTTP requests. The tests use the test server on port `7071` and exercise the API in a way that resembles how a client would use it.

I continued working with JWT authentication and authorization, particularly how tokens are passed between requests and how resources should be restricted to their owners.

For Google Calendar, I used Google's OAuth 2.0 flow and the Google API client libraries. The application redirects the user to Google for authorization and then receives an authorization code through the callback. The application must exchange that code for tokens before it can access the user's calendar.

I also continued using Hibernate/JPA and PostgreSQL, adjusting the database configuration to make local development easier without recreating the database schema on every normal application startup.

**Why did I structure it this way?**

I wanted to make sure that the API works as a connected system rather than as a collection of individual endpoints. For example, a training session depends on a training calendar and a training program, while exercise logs depend on training sessions and set logs depend on exercise logs. Testing these relationships helps verify that the complete flow works correctly.

I also wanted the security tests to create their own users and related data. This makes it possible to test ownership by using separate JWTs and checking whether one user can access another user's resources.

The DAO, service and controller structure remains important because it separates database access, application logic and HTTP handling. I followed the same structure for Google Calendar so that the external integration fits into the existing application.

For database configuration, using `update` during normal startup prevents the application from unnecessarily recreating the schema and losing local development data. The test configuration remains separate so that tests can use a clean database setup.

**What worked well?**

The API's main resource flows are in place, and I expanded the test setup to cover more of the relationships between users, calendars, training sessions, exercise logs and set logs.

I also made progress on the security tests for set logs, including setting up separate users and JWTs to test access to resources owned by different users.

The database configuration now supports preserving existing data during normal application restarts.

For Google Calendar, I successfully reached Google's authorization pages and completed the consent flow in the browser. The application generates the authorization URL and receives the callback, which means the initial redirect and consent stages are working far enough to reach the token exchange.

**What was difficult or confusing?**

One challenge was reviewing the complete API flow and making sure that the test data is created in the correct order. Since the resources depend on each other, a test for a set log needs a user, a profile, a training calendar, a training program, a training session and an exercise log before the set log can be created.

Another challenge was making sure that authentication and authorization are handled correctly throughout the API. A valid JWT identifies the user, but the application must also verify that the user is allowed to access the requested resource.

The main unresolved problem is Google OAuth. After completing Google's consent flow, the callback fails when exchanging the authorization code for tokens. Google's token endpoint returns HTTP `401 Unauthorized`, and the application returns HTTP 500. I added diagnostic logging, but the response body is currently reported as `null`, so the exact cause has not yet been confirmed.

**What would I improve next time?**

First, I want to finish reviewing the remaining API flows and security rules, particularly access to user-owned resources such as training programs, calendars, sessions, notes and logs. I also want to make sure that the existing tests cover the important successful and rejected requests without adding unnecessary complexity.

Next, I want to resolve the Google OAuth problem by checking that the client ID and client secret belong to the same OAuth client and that the registered redirect URI matches the application's configuration.

Once the Google connection works, I will verify that the connection is saved correctly for the authenticated user and that the application can create, update and delete events in Google Calendar.

After that, I want to verify token refreshing and complete the end-to-end integration. The main goal is to ensure that the API's existing training calendar functionality and the external Google Calendar integration work reliably together.
