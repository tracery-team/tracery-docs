# Profile Module

The `Profile Module` handles user profile operations in the Tracery application. It provides functionality for retrieving and managing [`user`](../data/UserEntity.md) profile information while ensuring proper authentication.

## Core Components

### Profile Service

The [`ProfileService`] handles profile-related business logic:

```typescript
@Injectable()
export class ProfileService {
  // Retrieve user profile with relationships
  async getUserInfo(userId: number): Promise<UserEntity | null> {
    const user = await this.userRepository.findOne({
      where: { id: userId },
      relations: ["friends", "events"],
    });
    return plainToInstance(UserEntity, user);
  }
}
```

### Profile Controller

The [`ProfileController`] exposes profile-related endpoints:

```typescript
@Controller("profile")
@UseGuards(AuthGuard) // Ensures all routes are protected
export class ProfileController {
  @Get("/")
  async mainInfo(@Request() request: AuthorizedRequest) {
    const { sub } = request.parsedToken;
    const user = await this.profileService.getUserInfo(sub);
    // Returns full user profile or 404
  }
}
```

## Key Features

### 1. Authentication Protection

- All routes are protected by _AuthGuard_
- Requires valid _JWT token_ for access
- Automatically extracts user ID from token

### Profile Information

- Retrieves complete user profile
- Includes related data:
- - Friend relationships
- - Event participations
- Returns data in DTO format for security

### Error Handling

- Returns 404 for non-existent users
- Handles authentication errors
- Provides clear error messages

## Module Configuration

The Profile module is configured in `profile.module.ts`:

```typescript
@Module({
  imports: [TypeOrmModule.forFeature([UserEntity])],
  controllers: [ProfileController],
  providers: [ProfileService],
})
export class ProfileModule {}
```

## API Endpoints

### Get Profile Information

```http
GET /profile
Authorization: Bearer <jwt_token>
```

**Request Body:**

No request body is required for this endpoint.

**Responses:**

- **200 OK**

  ```json
  {
    "id": 1,
    "username": "johndoe",
    "email": "johndoe@example.com",
    "friends": [
      {
        "id": 2,
        "username": "janedoe"
      }
    ],
    "events": [
      {
        "id": 1,
        "name": "Event 1"
      }
    ]
  }
  ```

- **401 Unauthorized**

  ```json
  {
    "statusCode": 401,
    "message": "Unauthorized"
  }
  ```

- **404 Not Found**

  ```json
  {
    "statusCode": 404,
    "message": "User not found"
  }
  ```

## Data Flow

1. Client makes authenticated request to _/profile_
2. _AuthGuard_ validates JWT token
3. User ID extracted from token payload
4. _ProfileService_ retrieves user data
5. Data transformed through _UserDto_
6. Response sent to client

## Usage Example

```typescript
// Making an authenticated profile request
@Get('/')
@UseGuards(AuthGuard)
async mainInfo(@Request() request: AuthorizedRequest) {
  const userId = request.parsedToken.sub;
  const profile = await this.profileService.getUserInfo(userId);
  if (!profile) {
    throw new NotFoundException();
  }
  return profile;
}
```

The Profile module provides essential functionality for retrieving and managing user profile information while maintaining security through authentication.
