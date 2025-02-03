# Event Module

The `Event Module` manages all event-related operations in the Tracery application. It provides functionality for creating, searching, and managing [`events`](../data/EventEntity.md), as well as handling user participation in [`events`](../data/EventEntity.md).

## Core Components

### Event Service

The [`EventService`] handles event-related business logic:

```typescript
@Injectable()
export class EventService {
  // Find all events with user relationships
  async findAll(): Promise<EventDto[]> {
    const events = await this.eventRepository.find({
      relations: ["users"],
    });
    return plainToInstance(EventDto, events);
  }

  // Search events with pagination and fuzzy search
  async searchEvents(page: number, search?: string) {
    // Uses Levenshtein distance for fuzzy search
    // Returns paginated results
  }

  // Add user to event
  async addEvent(userId: number, eventId: number) {
    // Adds event to user's events list
    // Prevents duplicate entries
  }
}
```

## Event Controller

The [`EventController`] exposes REST endpoints:

```typescript
@Controller("event")
export class EventController {
  @Get()
  async searchEvents(
    @Query("page") page: number = 1,
    @Query("search") search?: string
  ) {
    return this.eventService.searchEvents(page, search);
  }

  @Post("add")
  @UseGuards(AuthGuard)
  async addEvent(
    @Request() request: AuthorizedRequest,
    @Body("eventId") eventId: number
  ) {
    // Protected endpoint for adding events
  }
}
```

## Key Features

### Event Management

- Creating and retrieving events
- Managing event details (title, description, location, date)
- Handling event-user relationships

### Search Functionality

- Fuzzy search using [`Levenshtein`](../levenshtein.md) distance
- Searches through event titles and dates
- Paginated results with configurable page size

### User Participation

- Adding users to events
- Removing users from events
- Preventing duplicate event participation

### Data Protection

- Route protection using _AuthGuard_
- Validation of user permissions
- Safe data transformation using DTO

## API Endpoints

## API Endpoints

### Search Events

```typescript
GET /event?page=1&search=query
// Returns paginated list of events matching search
```

**Request Body:**

- None

**Responses:**

- `200 OK`: Returns a list of events matching the search criteria.
- `400 Bad Request`: Invalid query parameters.

### Add Event Participation

```typescript
POST / event / add;
Body: {
  eventId: number;
}
// Requires authentication
// Adds current user to event
```

**Request Body:**

- `eventId` (number): The ID of the event to add the user to.

**Responses:**

- `200 OK`: Event added successfully.
- `400 Bad Request`: Invalid event ID or user already added.
- `401 Unauthorized`: User not authenticated.

### Remove Event Participation

```typescript
DELETE / event / remove;
Body: {
  eventId: number;
}
// Requires authentication
// Removes current user from event
```

**Request Body:**

- `eventId` (number): The ID of the event to remove the user from.

**Responses:**

- `200 OK`: Event removed successfully.
- `400 Bad Request`: Invalid event ID or user not part of the event.
- `401 Unauthorized`: User not authenticated.

### Get Event Details

```typescript
GET /event/:id
// Returns detailed event information
// Includes participant list
```

**Request Body:**

- None

**Responses:**

- `200 OK`: Returns detailed information about the event.
- `404 Not Found`: Event not found.
- `400 Bad Request`: Invalid event ID.

## Module Configuration

The Event module is configured in `event.module.ts`:

```typescript
@Module({
  imports: [TypeOrmModule.forFeature([EventEntity, UserEntity])],
  controllers: [EventController],
  providers: [EventService],
})
export class EventModule {}
```

## Usage Example

```typescript
// Adding a user to an event
@Post('add')
@UseGuards(AuthGuard)
async addEvent(
  @Request() request: AuthorizedRequest,
  @Body('eventId') eventId: number,
) {
  const userId = request.parsedToken.sub;
  const result = await this.eventService.addEvent(userId, eventId);
  if (result) {
    return { message: 'Event added successfully' };
  }
  throw new BadRequestException('Could not add event');
}
```

The Event module provides the core functionality for event management and user participation in the Tracery application.
