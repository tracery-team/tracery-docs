# Auth Module

The `Auth Module` handles user authentication and authorization in the Tracery application. It provides functionality for user registration, login, and route protection using `JWT tokens`.

`JWT (JSON Web Tokens)` are a compact, URL-safe means of representing claims to be transferred between two parties. They are commonly used for authentication and authorization purposes. A JWT consists of three parts: a header, a payload, and a signature. The header typically consists of the token type and the signing algorithm. The payload contains the claims, which are statements about an entity (typically, the user) and additional data. The signature is used to verify that the sender of the JWT is who it says it is and to ensure that the message wasn't changed along the way.

A `Data Transfer Object (DTO)`, for example [`UserDTO`](../data/UserDTO.md) or [`EventDTO`](../data/EventDTO.md) is a simple object that is used to transfer data between different parts of an application. DTOs are typically used to encapsulate data and send it from one subsystem of an application to another, such as from the client to the server or between different layers of an application. They help in ensuring that only the necessary data is exposed and transferred, promoting a clear separation of concerns and improving the maintainability of the code.

## Core Components

### Authentication Service

The [`AuthService`] handles the business logic for authentication:

```typescript
@Injectable()
export class AuthService {
  // User registration
  async signUp(userData: Partial<UserEntity>): Promise<SignupStatus> {
    // Creates new user in database
    // Returns SUCCESS, DUPLICATE_ERR, or UNKNOWN_ERR
  }

  // User login with JWT generation
  async login(login: string, password: string): Promise<TokenJWT | null> {
    // Validates credentials and generates JWT token
  }
}
```

### JWT Protection

The [`AuthGuard`] protects routes requiring authentication:

```typescript
@Injectable()
export class AuthGuard implements CanActivate {
  async canActivate(context: ExecutionContext): Promise<boolean> {
    // Extracts JWT token from request header
    // Validates token and adds user data to request
    // Throws UnauthorizedException if invalid
  }
}
```

### Data Transfer Objects

1. `SignUpDto` - Validates registration data:

```typescript
export class SignUpDto {
  @IsNotEmpty()
  @Matches(/^[_a-zA-Z][_a-zA-Z0-9]*$/)
  @Length(4, 25)
  nickname: string;

  @IsEmail()
  email: string;

  @IsStrongPassword()
  password: string;
  // Additional fields...
}
```

2. `LoginDto` - Validates login credentials:

```typescript
export class LoginDto {
  @IsNotEmpty()
  login: string; // Can be email or nickname

  @IsNotEmpty()
  password: string;
}
```

## Key Features

### User Registration

- Validates registration data using _SignUpDto_
- Checks for duplicate usernames/emails
- Automatically hashes passwords

### Authentication

- Supports login via email or nickname
- Validates password using bcrypt
- Generates _JWT token_ on successful login

### Route Protection

- Guards sensitive routes using _@UseGuards(AuthGuard)_
- Validates JWT tokens automatically
- Adds user context to requests

## API Endpoints

### POST /auth/login

Handles user login.

- **Request Body**: `LoginDto`
  - `login` (string): User's email or nickname.
  - `password` (string): User's password.
- **Response**:
  - `200 OK`: `{ access_token: string }` - An object containing the access token.
  - `401 Unauthorized`: Invalid credentials.

### POST /auth/sign-up

Handles user sign-up.

- **Request Body**: `SignUpDto`
  - `nickname` (string): User's nickname.
  - `email` (string): User's email.
  - `password` (string): User's password.
- **Response**:
  - `201 Created`: `{ message: string }` - An object containing a success message.
  - `400 Bad Request`: Validation errors or duplicate username/email.

```typescript
@Controller('auth')
export class AuthController {
  @Post('login')
  async login(@Body() loginDto: LoginDto)
  // Returns: { access_token: string }

  @Post('sign-up')
  async signUp(@Body() signUpDto: SignUpDto)
  // Returns: { message: string }
}
```

## Module Configuration

The Profile module is configured in `auth.module.ts`:

```typescript
@Module({
  imports: [
    TypeOrmModule.forFeature([UserEntity]),
    JwtModule.register({
      global: true,
      secret: process.env.JWT_SECRET
    })
  ],
  controllers: [AuthController],
  providers: [AuthService]
})
```

## Usage Example

```typescript
@Controller("profile")
@UseGuards(AuthGuard) // Requires valid JWT
export class ProfileController {
  @Get()
  async getProfile(@Request() req: AuthorizedRequest) {
    const userId = req.parsedToken.sub;
    return this.profileService.getProfile(userId);
  }
}
```

The Auth module provides the foundation for secure user authentication and authorization throughout the application.
