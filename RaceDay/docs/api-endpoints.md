# RaceDay API Endpoint Plan

## Roles

* **Public** – no authentication required
* **Participant** – authenticated event participant
* **Organiser** – authenticated event organiser

## Authentication

| HTTP Method | Route                | Description                                      | Role Required | Request Body (if any)                             | Expected Response                                |
| ----------- | -------------------- | ------------------------------------------------ | ------------- | ------------------------------------------------- | ------------------------------------------------ |
| POST        | `/api/auth/register` | Register a new RaceDay participant account       | Public        | Email, Password, FirstName, LastName, PhoneNumber | 201 Created with user details and confirmation   |
| POST        | `/api/auth/login`    | Authenticate a user and generate an access token | Public        | Email, Password                                   | 200 OK with access token, UserID, name, and role |

## User Profile

| HTTP Method | Route                         | Description                           | Role Required           | Request Body (if any)                                                             | Expected Response           |
| ----------- | ----------------------------- | ------------------------------------- | ----------------------- | --------------------------------------------------------------------------------- | --------------------------- |
| GET         | `/api/users/{userId}`         | Retrieve a user's account information | Participant / Organiser | None                                                                              | 200 OK with user details    |
| GET         | `/api/users/{userId}/profile` | Retrieve a user's profile             | Participant / Organiser | None                                                                              | 200 OK with profile details |
| PUT         | `/api/users/{userId}/profile` | Create or update a user's profile     | Participant / Organiser | DateOfBirth, Gender, EmergencyContactName, EmergencyContactPhone, ProfileImageUrl | 200 OK with updated profile |

## Events

| HTTP Method | Route                   | Description                                          | Role Required | Request Body (if any)                                                                  | Expected Response                   |
| ----------- | ----------------------- | ---------------------------------------------------- | ------------- | -------------------------------------------------------------------------------------- | ----------------------------------- |
| GET         | `/api/events`           | Retrieve all RaceDay events                          | Public        | None; optional filters                                                                 | 200 OK with list of events          |
| GET         | `/api/events/upcoming`  | Retrieve upcoming events                             | Public        | None                                                                                   | 200 OK with list of upcoming events |
| GET         | `/api/events/{eventId}` | Retrieve details for a specific event                | Public        | None                                                                                   | 200 OK with event details           |
| POST        | `/api/events`           | Create a new road running, walking, or cycling event | Organiser     | EventName, Description, EventType, EventDate, StartTime, Venue, City, Province, Status | 201 Created with created event      |
| PUT         | `/api/events/{eventId}` | Update an existing event                             | Organiser     | EventName, Description, EventType, EventDate, StartTime, Venue, City, Province, Status | 200 OK with updated event           |
| DELETE      | `/api/events/{eventId}` | Delete an event                                      | Organiser     | None                                                                                   | 204 No Content                      |

## Categories

| HTTP Method | Route                          | Description                   | Role Required | Request Body (if any)                               | Expected Response                 |
| ----------- | ------------------------------ | ----------------------------- | ------------- | --------------------------------------------------- | --------------------------------- |
| GET         | `/api/categories`              | Retrieve all event categories | Public        | None                                                | 200 OK with list of categories    |
| GET         | `/api/categories/{categoryId}` | Retrieve a specific category  | Public        | None                                                | 200 OK with category details      |
| POST        | `/api/categories`              | Create a new race category    | Organiser     | CategoryName, DistanceKM, CategoryType, Description | 201 Created with created category |
| PUT         | `/api/categories/{categoryId}` | Update a race category        | Organiser     | CategoryName, DistanceKM, CategoryType, Description | 200 OK with updated category      |
| DELETE      | `/api/categories/{categoryId}` | Delete a race category        | Organiser     | None                                                | 204 No Content                    |

## Event Categories

| HTTP Method | Route                                                | Description                                | Role Required | Request Body (if any)                     | Expected Response                  |
| ----------- | ---------------------------------------------------- | ------------------------------------------ | ------------- | ----------------------------------------- | ---------------------------------- |
| GET         | `/api/events/{eventId}/categories`                   | Retrieve categories available for an event | Public        | None                                      | 200 OK with event category list    |
| POST        | `/api/events/{eventId}/categories`                   | Add a category to an event                 | Organiser     | CategoryID, EntryFee, MaximumParticipants | 201 Created with event category    |
| PUT         | `/api/events/{eventId}/categories/{eventCategoryId}` | Update an event category                   | Organiser     | EntryFee, MaximumParticipants             | 200 OK with updated event category |
| DELETE      | `/api/events/{eventId}/categories/{eventCategoryId}` | Remove a category from an event            | Organiser     | None                                      | 204 No Content                     |

## Event Enrolments

| HTTP Method | Route                              | Description                                                | Role Required           | Request Body (if any)          | Expected Response                                  |
| ----------- | ---------------------------------- | ---------------------------------------------------------- | ----------------------- | ------------------------------ | -------------------------------------------------- |
| POST        | `/api/enrolments`                  | Enrol the authenticated participant into an event category | Participant             | EventCategoryID                | 201 Created with enrolment details and race number |
| GET         | `/api/enrolments/{enrolmentId}`    | Retrieve a specific enrolment                              | Participant / Organiser | None                           | 200 OK with enrolment details                      |
| GET         | `/api/users/{userId}/enrolments`   | Retrieve a participant's enrolment history                 | Participant / Organiser | None                           | 200 OK with list of enrolments                     |
| GET         | `/api/events/{eventId}/enrolments` | Retrieve all enrolments for an event                       | Organiser               | None                           | 200 OK with list of event enrolments               |
| PUT         | `/api/enrolments/{enrolmentId}`    | Update enrolment or payment status                         | Participant / Organiser | PaymentStatus, EnrolmentStatus | 200 OK with updated enrolment                      |
| DELETE      | `/api/enrolments/{enrolmentId}`    | Cancel an enrolment                                        | Participant / Organiser | None                           | 204 No Content                                     |

## Results

| HTTP Method | Route                                  | Description                                  | Role Required           | Request Body (if any)                                              | Expected Response               |
| ----------- | -------------------------------------- | -------------------------------------------- | ----------------------- | ------------------------------------------------------------------ | ------------------------------- |
| GET         | `/api/results/{resultId}`              | Retrieve a specific race result              | Participant / Organiser | None                                                               | 200 OK with result details      |
| GET         | `/api/enrolments/{enrolmentId}/result` | Retrieve the result linked to an enrolment   | Participant / Organiser | None                                                               | 200 OK with result              |
| GET         | `/api/users/{userId}/results`          | Retrieve a participant's performance history | Participant / Organiser | None                                                               | 200 OK with result history      |
| GET         | `/api/events/{eventId}/results`        | Retrieve results for an event                | Organiser               | None                                                               | 200 OK with event results       |
| POST        | `/api/results`                         | Record a participant's race result           | Organiser               | EnrolmentID, FinishTime, OverallPosition, CategoryPosition, Status | 201 Created with created result |
| PUT         | `/api/results/{resultId}`              | Update a race result                         | Organiser               | FinishTime, OverallPosition, CategoryPosition, Status              | 200 OK with updated result      |
| DELETE      | `/api/results/{resultId}`              | Delete a race result                         | Organiser               | None                                                               | 204 No Content                  |

## Routes

| HTTP Method | Route                         | Description                                 | Role Required | Request Body (if any)                                          | Expected Response              |
| ----------- | ----------------------------- | ------------------------------------------- | ------------- | -------------------------------------------------------------- | ------------------------------ |
| GET         | `/api/events/{eventId}/route` | Retrieve the route information for an event | Public        | None                                                           | 200 OK with route details      |
| POST        | `/api/events/{eventId}/route` | Add a route to an event                     | Organiser     | RouteName, DistanceKM, ElevationGain, RouteMapUrl, Description | 201 Created with route details |
| PUT         | `/api/events/{eventId}/route` | Update an event route                       | Organiser     | RouteName, DistanceKM, ElevationGain, RouteMapUrl, Description | 200 OK with updated route      |
| DELETE      | `/api/events/{eventId}/route` | Delete an event route                       | Organiser     | None                                                           | 204 No Content                 |

## Weather

| HTTP Method | Route                           | Description                               | Role Required | Request Body (if any) | Expected Response                                                                                           |
| ----------- | ------------------------------- | ----------------------------------------- | ------------- | --------------------- | ----------------------------------------------------------------------------------------------------------- |
| GET         | `/api/events/{eventId}/weather` | Retrieve weather information for an event | Public        | None                  | 200 OK with temperature, feels-like temperature, humidity, wind speed, weather condition, and recorded time |

## Standard Responses

| Situation                  | Expected Response |
| -------------------------- | ----------------- |
| Successful retrieval       | 200 OK            |
| Successful creation        | 201 Created       |
| Successful update          | 200 OK            |
| Successful deletion        | 204 No Content    |
| Invalid request            | 400 Bad Request   |
| Authentication failure     | 401 Unauthorized  |
| Insufficient permissions   | 403 Forbidden     |
| Record not found           | 404 Not Found     |
| Duplicate/conflicting data | 409 Conflict      |
