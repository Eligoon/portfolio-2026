---
title: Training Program Week 9
weight: 1
date: 2026-10-26
draft: false
---

## Week 9 - Deploy the troops

**What did I add this week?**

This week I continued developing the exercise functionality in the REST API. I created an initial library of 40 predefined exercises and connected them to the newly created body parts in the database. Each exercise has a name, description and one or more associated body parts. The body parts also have colors, which can be used by the frontend when displaying exercises and training information when that time comes.

I implemented seeders for body parts and exercises so that the predefined data is inserted automatically when the application starts. The seeders check whether entries already exist before inserting them, allowing the initial library to be created without duplicating entries every time the application restarts.

I also improved the exercise API responses. Previously, the exercise DTO only contained the exercise ID, name and description. I extended it to include a list of body parts, including each body part's ID, name and color. This gives the frontend the information it needs without requiring separate requests for every exercise's body parts.

During this work, I encountered a Hibernate lazy-loading problem. The exercise's body parts were loaded lazily, but the EntityManager was closed before the service converted the exercise into a DTO. I fixed this in the DAO queries by fetching the associated body parts together with the exercise. I applied this to both the query returning the available exercises and the query retrieving an individual exercise.

I also continued verifying the distinction between predefined exercises and custom exercises. Predefined exercises are shared between users, while custom exercises belong to their creator. The intention is that users can access the shared library and their own custom exercises without being able to modify another user's private exercises.

Another important part of the week was verifying and deploying the backend. I inspected the Docker Compose configuration and Caddyfile on my DigitalOcean Droplet. This helped me identify that the API is served through the subdomain `api.oniscidea.dk`, with Caddy forwarding requests to the Java application on port `7070`.

The application runs on a DigitalOcean Droplet using Docker, with the application image stored in Docker Hub and the source code hosted on GitHub. I reviewed the Docker Compose configuration to understand how PostgreSQL, Caddy and Watchtower work together. Caddy acts as a reverse proxy, handling HTTPS requests for `api.oniscidea.dk` and forwarding them to the Javalin application on port `7070`, while Watchtower checks for updated container images.

I then tested the deployed API and confirmed that the body-parts endpoint returns all 14 predefined body parts. I also logged in to the deployed application, obtained a fresh JWT and successfully retrieved the exercise library using the authenticated exercises endpoint.

**Which new technology did I use?**

I continued using Hibernate/JPA to manage relationships between exercises and body parts. In particular, I used JPQL fetch joins to load related entities before the EntityManager is closed, avoiding lazy-loading exceptions when building DTOs.

I continued using DTOs to control the structure of the API responses. The updated `ExerciseDTO` now includes a list of `BodyPartDTO` objects, allowing the API to return structured exercise and body-part information in a single response.

I also worked with database seeders to populate the initial exercise library and body-part data. This separates the predefined application data from manual database setup and makes it easier to initialize the database consistently.

For deployment, I deployed my Digital Ocean droplet through Ubuntu, and made a Docker Compose setup, the Caddy reverse proxy and Watchtower. I also tested the production API using PowerShell and `Invoke-RestMethod`, including sending a JWT in the Authorization header.

**Why did I structure it this way?**

I wanted to create a reusable exercise library that the application can use when users plan their training. By connecting exercises to body parts, the API can provide the frontend with information that can be used for filtering, categorization and visual presentation.

I used seeders because predefined data should be available automatically when the application is initialized.

I extended the DTO rather than returning the Hibernate entity directly. This keeps the API response separate from the internal database model and gives the frontend the information it needs in a proper format.

The fetch joins were necessary because the service converts entities into DTOs after the DAO has completed its database operation. Fetching the body parts as part of the query ensures that the required relationship is loaded before the EntityManager closes.

For deployment, I wanted to verify that the backend works outside my local development environment. Testing the live API with a valid JWT confirmed that the deployed application can authenticate a user and return protected exercise data.

**What worked well?**

The initial exercise library was successfully created with 40 predefined exercises linked to the 14 body parts. The database contains the expected relationships between exercises and body parts.

Both exercise endpoints now return the body-part information in their responses. The lazy-loading issue was resolved by updating the DAO queries, and all 43 automated tests passed after these changes.

I also confirmed that the deployed backend is reachable over HTTPS at `https://api.oniscidea.dk`. The body-parts endpoint returned all 14 body parts, and the authenticated exercise endpoint returned 40 exercises after logging in and obtaining a fresh JWT.

This provided a useful check that the deployment, authentication, database seed data and exercise API work together in the production environment.

**What was difficult or confusing?**

One challenge was the lazy-loading behavior in Hibernate. The exercise endpoints initially returned HTTP 500 errors because the body-part collection was accessed after the EntityManager had closed. I needed to understand where the data was being loaded and update the DAO queries so that the relationship was fetched in time.

Another challenge was distinguishing between the different parts of the deployment. GitHub stores the source code, Docker Hub stores the container image, and the DigitalOcean Droplet runs the application using Docker. Caddy handles incoming HTTPS requests and forwards them to the Java backend, while Watchtower can update the running container when a new image is available.

I also initially confused the Google OAuth configuration with the hosting configuration. The Google web application controls OAuth permissions and redirect settings; it does not host the Java backend. Understanding this distinction helped me identify the actual production API URL.

**What would I improve next time?**

Next, I want to complete the final review of the backend before hand-in. I will focus on verifying that the main API flows work together correctly, checking error handling and input validation, and confirming that the deployed application continues to work after a restart.

I also want to review the deployment configuration, particularly environment variables, database exposure and the handling of credentials. The Google Calendar integration still needs to be checked against the production domain to ensure that its OAuth redirect URI and token handling work correctly.

Finally, I want to review the README and ensure that the project can be built, configured and started using the documented instructions. The goal is to finish with a backend that is not only functional but also straightforward for another developer or examiner to understand.