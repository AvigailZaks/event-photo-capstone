# Event Title Correction — Requirements

## Facts
- The `title` field is defined as a string in `types.ts`.
- The sample event title is "Cohen Wedding" in `memory-store.ts`.
- `getClientGallery` and `getGallery` return `event.title` in their gallery responses (`event-service.ts`).
- `GET /api/events` requires the `x-photographer-id` header and calls `events.listEvents` (`app.ts`).
- The test for `GET /api/events` verifies that `studio-amber` receives one event with ID `event-100` (`tests/event-api.test.ts`).
- The test for submitting a gallery selection checks for HTTP 201 and status "submitted"; it does not check the event title (`tests/event-api.test.ts`).

## Assumptions
- The event title should be editable after the event has been created.

## Open Questions
1. Who is allowed to change the event title: the photographer, the client, or both?
2. Can the title be changed at any time, or only before the gallery is published?
3. Can the user choose any title, or are there restrictions on its length or content?
4. Should the new title appear immediately in both the client's gallery and the photographer's view?
5. What should happen if someone tries to change the title of a non-existent event or an event they are not authorized to edit?

## Requirements Analysis
### 1. Functional Requirements
- The system should allow an event title to be corrected and display the updated title in the gallery.

### 2. Non-Functional Requirements
- The title update should be reliable and should not unintentionally modify other event data.

### 3. Actors / Users
- Potential actors are the photographer and the client. The authorized user must be confirmed.

### 4. Use Cases
- A user corrects an event title.
- The system validates the new title and displays the updated title in the gallery.
- The system rejects an update if the user is not authorized.

### 5. Constraints
- The current application uses demo identity headers rather than real authentication.
- The implementation must follow the existing event data model and API structure.

### 6. Assumptions
- The event title should be editable after the event has been created.
- The updated title should appear in the relevant gallery.

### 7. Acceptance Criteria
- Given an existing event and an authorized user, when the user changes the title to a valid value, then the gallery displays the updated title.
- Given an existing event and an unauthorized user, when they attempt to change the title, then the system rejects the request.
- Given an existing event, when an authorized user submits an empty title, then the system rejects the update.
- Given a non-existent event, when a user attempts to change its title, then the system returns an appropriate error response.

### 8. Ambiguity
- It is not yet clear who is allowed to edit an event title.
- It is not yet clear what makes a title valid.

### 9. Missing Information
- The required permissions, title validation rules, and expected error responses have not been specified.

### 10. Scope
- In scope: correcting an event title and displaying the updated value in the gallery.
- Out of scope unless explicitly requested: changing photos, services, client selections, or other event details.

## Questions by Requirements Lens

### Actor and Permissions
- Who is allowed to change an event title?
- Can the client change the title, or only the photographer?

### Functional Requirements and Scope
- Should the new title appear in both the photographer's view and the client's gallery?
- Is the task limited to correcting the title, or should other event details also be editable?

### Ambiguity and Missing Information
- What makes a title valid?
- What should happen if the event does not exist?

### Constraints
- Should title changes be allowed after the gallery has been published?

### Assumptions and Acceptance
- Is it acceptable for the title to update immediately after a successful change?
- How should we verify that the updated title appears correctly in the gallery?

## AI Review — Gemini

### Suggestions Accepted
- Clarify who is authorized to change an event title, because the current API uses demo identity headers and does not implement real authentication.
- Define title validation rules before implementation, because the current requirements do not specify what makes a title valid.

### Suggestions Deferred
- Decide whether an audit trail is needed. This is not essential to clarify the basic title-correction behavior.
- Decide whether active gallery viewers need real-time updates. The current request does not specify this behavior.

### Suggestions Not Adopted Yet
- Choosing a specific HTTP method and route is an implementation decision. The required behavior and permissions should be clarified first.
- Frontend behavior such as automatic re-rendering cannot be confirmed from the repository facts reviewed so far.
