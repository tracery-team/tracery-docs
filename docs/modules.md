# Module Structure

The NestJS application is organized into modules that encapsulate related functionality. Each module is a cohesive block of code dedicated to a specific domain, feature, or functionality in the application.

## Module Hierarchy

```
/modules
    /auth                       # Authentication and authorization
        /auth.controller.ts
        /auth.guard.ts
        /auth.module.ts
        /auth.service.ts
        /login.dto.ts
        /signup.dto.ts
    /event                      # Event management
        /event.controller.ts
        /event.module.ts
        /event.service.ts
    /profile                    # User profile management
        /profile.controller.ts
        /profile.module.ts
        /profile.service.ts
    /user                       # User management
        /user.controller.ts
        /user.module.ts
        /user.service.ts
```

## Module Details

### Auth Module

The [`Auth Module`](modules/AuthModule.md) handles authentication and authorization processes. It provides:

- User signup and login functionality
- JWT token generation and validation
- Route protection with [`AuthGuard`]
- Validation of credentials using [`SignUpDto`] and [`LoginDto`]

### Event Module

The [`Event Module`](modules/EventModule.md) manages all event-related operations through [`EventService`]:

- Creating and deleting events
- Searching events with pagination
- Managing event participants
- Retrieving event details
- Fuzzy search functionality using [`Levenshtein`](levenshtein.md) distance

### Profile Module

The [`Profile Module`](modules/ProfileModule.md) handles user profile operations via [`ProfileService`]:

- Retrieving user profile information
- Managing personal user data
- Protected by [`AuthGuard`] to ensure authentication
- Accessing relationships (friends, events)

### User Module

The [`User Module`](modules/UserModule.md) manages user-related operations through [`UserService`]:

- User search functionality
- Friend management (add/remove)
- User data retrieval
- Relationship management
- Pagination and search features

## General information

Each module follows NestJS's modular architecture pattern with:

- Controllers for handling HTTP requests
- Services for business logic
- Module classes for configuration and dependency injection
- DTOs for data validation where applicable
