# User Module

The `User Module` manages user-related operations in the Tracery application. It provides functionality for user management, friend relationships, and user search capabilities.

## Core Components

### User Service

The [`UserService`] handles user-related business logic:

```typescript
@Injectable()
export class UserService {
  // Find all users with relationships
  async findAll(): Promise<UserDto[]> {
    const users = await this.userRepository.find({
      relations: ["friends", "events"],
    });
    return users;
  }

  // Search users with pagination and fuzzy search
  async searchFriends(page: number, search?: string) {
    // Uses Levenshtein distance for nickname/email search
    // Returns paginated results
  }

  // Manage friend relationships
  async addFriend(userId: number, friendId: number) {
    // Adds bidirectional friend relationship
    // Prevents duplicate entries
  }
}
```

### User Controller

The [`UserController`] exposes REST endpoints:

```typescript
@Controller("user")
export class UserController {
  @Get("/info/:id")
  async getUser(@Param("id") id: number) {
    // Returns user information
  }

  @Post("add-friend")
  @UseGuards(AuthGuard)
  async addFriend(
    @Request() request: AuthorizedRequest,
    @Body("friendId") friendId: number
  ) {
    // Protected endpoint for adding friends
  }
}
```

## Key Features

### User Management

- User information retrieval
- User search functionality
- Data transformation using _UserDto_

### Friend System

- Adding/removing friends
- Bidirectional friend relationships
- Friend search with pagination

### Search Functionality

- Fuzzy search using `levenshtein` distance
- Searches through nicknames and emails
- Paginated results with configurable _PAGE_SIZE_

### Data Protection

- Route protection using _AuthGuard_
- Safe data transformation using DTOs
- Error handling for invalid operations

## API Endpoints

### Get User Information

```typescript
GET /user/info/:id
// Returns: UserDto with relationships
```

**Request Body:**

- None

**Responses:**

- `200 OK`: Returns the user information along with relationships.
- `400 Bad Request`: Invalid user ID.
- `404 Not Found`: User not found.

### Search Friends

```typescript
GET /user/friends?page=1&search=query
// Returns: Paginated list of users
```

**Request Body:**

- None

**Responses:**

- `200 OK`: Returns a paginated list of users matching the search criteria.
- `400 Bad Request`: Invalid query parameters.

### Add Friend

```typescript
POST / user / add - friend;
Body: {
  friendId: number;
}
// Requires authentication
```

**Request Body:**

- `friendId` (number): The ID of the friend to add.

**Responses:**

- `200 OK`: Friend added successfully.
- `400 Bad Request`: Invalid friend ID or friend already added.
- `401 Unauthorized`: User not authenticated.

### Remove Friend

```typescript
DELETE / user / remove - friend;
Body: {
  friendId: number;
}
// Requires authentication
```

**Request Body:**

- `friendId` (number): The ID of the friend to remove.

**Responses:**

- `200 OK`: Friend removed successfully.
- `400 Bad Request`: Invalid friend ID or friend not found.
- `401 Unauthorized`: User not authenticated.

## Module Configuration

The User module is configured in `user.module.ts`:

```typescript
@Module({
  imports: [TypeOrmModule.forFeature([UserEntity])],
  controllers: [UserController],
  providers: [UserService],
  exports: [UserService],
})
export class UserModule {}
```

## Usage Example

```typescript
// Adding a friend with bidirectional relationship
@Post('add-friend')
@UseGuards(AuthGuard)
async addFriend(
  @Request() request: AuthorizedRequest,
  @Body('friendId') friendId: number,
) {
  const userId = request.parsedToken.sub
  const result = await this.userService.addFriend(userId, friendId) &&
                 await this.userService.addFriend(friendId, userId)

  if (result) {
    return { message: 'Friend added successfully' }
  }
  throw new BadRequestException('Could not add friend')
}
```

The User module provides essential functionality for user management and social features in the Tracery application.
