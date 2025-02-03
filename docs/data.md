# Data Structure

The data inside the project is divided into two `Data (Entity) Models` and two `DTOs (Data Transfer Objects)` that exist in the API Layer.

```
/src
    /data
        /event.dto.ts
        /user.dto.ts
        /event.entity.ts
        /user.entity.ts
```

The two Entity Models exist in the Database Layer. They are the [`UserEntity`](data/UserEntity.md) and the [`EventEntity`](data/EventEntity.md).

The two DTOs in the API Layer are the [`EventDto`](data/EventDTO.md) and [`UserDto`](data/UserDTO.md).

### Entity Models

`Entity Models` represent the structure of the data stored in the database. They define the schema of the database tables and include properties that map to the columns in those tables. They handle data persistence logic (like password hashing) and contain sensitive information. Each entity corresponds to a single table in the database.

### Database Layer

The `Database Layer` is responsible for managing the data storage and retrieval operations. It interacts with the database to perform _CRUD (Create, Read, Update, Delete)_ operations on the data. This layer ensures that the data is stored in a structured and efficient manner, and provides an interface for the application to access and manipulate the data.

### Data Transfer Objects (DTO)

The `DTOs` serve several important purposes in the application. These classes act as filters for sensitive data as they prevent exposing sensitive information like password hashes, ensuring that only the necessary data is exposed and transferred, enhancing security and performance.

```typescript
@Exclude()
password: string  // Password is excluded from API responses
```

For example, the `EventDto` class represents the structure of an event object, including its id, title, description, location, date, and associated users. The `users` property is transformed to exclude sensitive information such as passwords and friends list from the user objects.

### API Layer

The `API Layer` handles the communication between the client and the server. It uses DTOs to transfer data securely and efficiently, ensuring that sensitive information is not exposed. This layer processes incoming requests, applies business logic, and returns appropriate responses.

- Handles client-server communication
- Uses DTOs for secure data transfer
- Processes requests and applies business logic
- Returns appropriate responses

###

The architecture follows a clear separation between database entities and DTOs to ensure security and proper data encapsulation when sending responses through the API.

## User Entity

The [`UserEntity`](data/UserEntity.md) is the core user model with database mappins using TypeORM decorators.

## Event Entity

The [`EventEntity`](data/EventEntity.md) represents events in the system and maps to the events table in the database. It manages event information and participant relationships.

## Event DTO

The [`EventDto`](data/EventDTO.md) is a Data Transfer Object that represents an event. It contains information about the event such as its ID, title, description, location, date, and associated users.

## User DTO

The [`UserDto`](data/UserDTO.md) is a Data Transfer Object that represents a user. It contains information about the user such as their ID, nickname, first name, last name, email, friends, and events.

## Data Storage

- Events and users are stored in the _PostgreSQL_ database
- Uses _TypeORM_ decorators for database mapping
- Enforces unique event titles and user emails to prevent duplicates

## User Relationships

- Maintains many-to-many relationship with events
- Bidirectional mapping with _EventEntity_
- Allows tracking event participants and user events

## Event Relationships

- Each event can have multiple participants
- Maintains many-to-many relationship with users
- Uses join tables to manage event-user associations
- Allows querying events by participants and vice versa
- Ensures referential integrity between events and users

## Data Transfer

- API responses use _EventDto_ and _UserDto_
- Sensitive user data is filtered from participant lists
- Maintains clean data structure for frontend consumption
- Ensures secure and efficient data transfer between client and server
