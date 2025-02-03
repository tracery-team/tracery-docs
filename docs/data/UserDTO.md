# UserDTO

The `UserDTO` is strictly connected to the [`User Entity`](UserEntity.md)!

## Format

```typescript
//Importing necessary decorators and classes from external libraries and local files.
// `Exclude` and `Transform` are decorators from the `class-transformer` library used for transforming and excluding properties during serialization and deserialization.
import { Exclude, Transform } from "class-transformer";
// `EventDto` is a Data Transfer Object (DTO) imported from a local file, likely used to define the structure of event data.
import { EventDto } from "./event.dto";

export class UserDto {
  id: number;
  nickname: string;
  firstName: string;
  lastName: string;
  email: string;

  @Exclude()
  password: string;

  @Transform(({ value }) => {
    const { friends, password, ...safeUser } = value;
    return safeUser;
  })
  friends: UserDto[];

  @Transform(({ value }) => {
    const { password, friends, ...safeEventData } = value;
    return safeEventData;
  })
  events: EventDto[];
}
```

### Properties

- `id`: Unique identifier for the user.
- `nickname`: User's unique nickname/username.
- `firstName`: User's first name.
- `lastName`: User's last name.
- `email`: User's unique email address.
- `password`: Hashed password (excluded from API responses).
- `friends`: List of user's friends.
- `events`: List of events the user participates in.

### Key Relationships

- `friends`: Many-to-many self-referential relationship with other [`UserDto`](UserDTO.md) objects, representing the user's friends. Sensitive information is excluded.
- `events`: Many-to-many relationship with [`EventDto`](EventDTO.md), representing the events the user participates in. Sensitive information is excluded.

### Key Features

- Represents the structure of a user object.
- Ensures sensitive data like passwords and friends list are excluded from API responses.
- Facilitates secure and efficient data transfer between client and server.
- Maintains clean data structure for frontend consumption.
