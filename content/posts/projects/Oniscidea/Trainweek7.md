---
title: Training Program Week 7
weight: 1
date: 2026-10-13
draft: false
---

## Week 7 - Implementing JWT Authentication and Authorization.

**What did I add this week?**

This week I focused on implementing JWT authentication and authorization in the REST API. I created a `JwtService` to generate and validate JSON Web Tokens and connected it to the existing login functionality. When a user successfully logs in, the API now returns a token along with the user's basic information.

I also created JWT authentication middleware for Javalin. This middleware checks whether a request contains a valid bearer token before allowing access to protected endpoints.

After implementing authentication, I added authorization checks for profiles. Users can retrieve, update and delete their own profiles, but they receive a forbidden response if they try to access or modify a profile belonging to another user.

I also expanded the Cucumber security tests to cover these scenarios. All 15 Cucumber scenarios are now passing.

**Which new technology did I use?**

I used the JJWT library to generate and validate JSON Web Tokens. The tokens contain the user's ID and email, and they expire after one hour. I also configured a secret key in my local configuration file to sign and validate the tokens.

I continued using Cucumber, Gherkin, Rest Assured and Testcontainers to test the API. I added scenarios for missing tokens, invalid tokens, accessing a user's own profile, and attempting to access, update or delete another user's profile.

**Why did I structure it this way?**

I wanted to separate authentication from authorization. Authentication verifies that a request has a valid token, while authorization checks whether the authenticated user is allowed to access a specific resource.

The JWT middleware handles token validation, while the profile service checks whether the requested profile belongs to the authenticated user. This keeps the ownership checks in the service layer and prevents the application from relying only on the client to enforce access restrictions.

I also kept the JWT secret outside the source code so that it does not need to be committed to version control.

**What worked well?**

The biggest success was getting JWT authentication integrated with the existing login endpoint and Javalin application. A user can log in, receive a token and use it to access protected profile endpoints.

I also successfully implemented ownership checks for retrieving, updating and deleting profiles. The automated tests verify that requests without a token or with an invalid token are rejected, and that users cannot access or modify another user's profile.

All 15 Cucumber scenarios pass, including the new security tests. The tests continue to run against PostgreSQL using Testcontainers.

**What was difficult or confusing?**

One challenge was understanding the difference between authentication and authorization. Validating a token confirms which user is making the request, but it does not automatically mean that the user should have access to every resource.

I had to make sure that the authenticated user's ID was available to the profile service so it could check ownership before performing operations. I also needed to extend the existing tests to verify both successful access and rejected requests.

**What would I improve next time?**

The security implementation is still incomplete because only the individual profile endpoints currently have authentication and ownership checks.

Next, I want to extend these checks to training programs, training calendars, notes and other user-owned resources. I also need to review profile creation and the endpoint that returns all profiles to ensure that users cannot access other users' data through those routes.

After that, I want to add more automated security tests and verify the Google Calendar integration end to end.