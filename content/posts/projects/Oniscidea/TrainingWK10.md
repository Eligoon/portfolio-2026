---
title: Training Program Week 10
weight: 1
date: 2026-10-29
draft: false
---

## Week 10 - Getting everything in sync with the Exam

**What did I add this week?**

This week I focused on fixing and verifying the Google Calendar integration in the backend, improving JWT security, and preparing the project documentation for the final hand-in.

An important part of the week was resolving problems with synchronizing training sessions and Google Calendar events, the same for training alendars and the Google Calendars themselves. Then there were problems of the same nature with the update and delete operations.

I reviewed the `TrainingSessionService` and `GoogleCalendarService` implementations and updated the synchronization logic. When a training session is updated, the existing Google Calendar event should be updated instead of creating a duplicate. When a training session is deleted, its corresponding Google Calendar event should also be deleted. I also worked on making sure that the correct Google Calendar ID is used when interacting with events.

After making these changes, I rebuilt and deployed the application and tested the functionality against the production API at `https://api.oniscidea.dk`. I created a test training session through the API and confirmed that the response included a Google event ID and that the event appeared in Google Calendar.

I then updated the training session through the API, changing its time from 10:00–10:45 to 11:00–11:45 on November 6, 2026. The API returned the updated session, and I confirmed that the existing event moved to the new time in Google Calendar. The Google event ID remained unchanged, demonstrating that the existing event had been updated.

Finally, I deleted the test session through the API. I confirmed that the event disappeared from Google Calendar and that retrieving the deleted session returned HTTP 404 with the message `Training session not found`. This verified the create, update and delete flows in the deployed application.

I also worked on a few missing JWT security fixes. This involved reviewing the authentication-related code and making improvements to the handling of authenticated requests. I continued testing the deployed API using PowerShell and authenticated requests with a JWT in the Authorization header.

Another important task was preparing the documentation for the backend exam. I finished the README and worked on completing the project overview, architecture description, data model documentation, API documentation and user stories.

**Which new technology did I use?**

This week I continued working with the Google Calendar API and the Google OAuth integration. The work involved coordinating the training session service with the Google Calendar service so that database operations and external calendar events remain synchronized.

I also continued using Javalin to expose the REST API and PowerShell with `Invoke-RestMethod` to test the deployed endpoints. This allowed me to verify the actual HTTP responses and the resulting changes in Google Calendar rather than relying only on local development tests.

For authentication, I continued working with JWT-based authentication and the request authentication flow. This helped me review how protected endpoints identify authenticated users and how authentication is applied to incoming API requests.

**Why did I structure it this way?**

I wanted to ensure that Google Calendar synchronization works as part of the normal training session workflow. Users should not have to manage the same training event separately in the application and in Google Calendar.

For that reason, the training session service coordinates the application-level operation, while the Google Calendar service handles communication with Google's API. This separation keeps the integration logic in the service layer rather than placing it directly in the controller.

I also wanted to verify the integration in the deployed environment. A successful build does not necessarily mean that communication with an external API works correctly, so testing the live application gave me evidence that the services, database and Google Calendar integration work together.

The JWT security work was important because the API contains protected operations and user-specific data. Authentication and authorization need to be handled consistently so that requests are associated with the correct user.

Finally, I wanted the README to explain the system clearly to another developer or examiner. Documenting the architecture, data model and API makes the project easier to understand and maintain.

**What worked well?**

The application built and deployed successfully after the changes.

The Google Calendar integration passed the three main functional tests in production:

- Creating a training session created a corresponding Google Calendar event.
- Updating the session changed the existing event's time without changing its Google event ID.
- Deleting the session removed the corresponding event from Google Calendar and deleted the session from the application.

These tests provided concrete evidence that the core synchronization flow works in the deployed application.

**What was difficult or confusing?**

One challenge was coordinating the Google Calendar ID and event ID across different parts of the application. The service methods needed to use the correct calendar when updating or deleting an event. The controller's existing routes also had to remain compatible with the service method signatures.

Another challenge was verifying that the changes worked beyond compilation. The application could build and deploy successfully while still having problems with the external calendar integration. Testing the create, update and delete operations separately helped identify whether each operation behaved correctly.

I also had to distinguish between changes that were implemented and verified and work that still needed to be completed. For the final hand-in, it is important to have both a working application and documentation that describes its functionality.

**What would I improve next time?**

There is not a next time as this is the final log before the backend exam, but if I could.

I would improve the automated test coverage for the Google Calendar integration, including failure scenarios where a Google Calendar operation succeeds or fails independently of a database operation. This would help identify possible inconsistencies between the application and the external calendar service.

I wanted to implement the planned RIR calculation functionality. The intended rules have been worked out: increase the suggested weight by 2.5 kg when the actual RIR is higher than the target, decrease it by 5 kg when the actual RIR is at least two below the target, and otherwise keep the weight unchanged. The calculation will need to be implemented in a Java calculation class and integrated as an optional feature for users creating training sessions. This remains future work rather than a completed feature.
