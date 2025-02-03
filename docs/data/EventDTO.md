# EventDTO

The `EventDTO` is strictly connected to the [`Event Entity`](EventEntity.md)!

## Format

```typescript
// Importing the Transform decorator from the class-transformer library
// This decorator is used to transform the values of class properties during the serialization and deserialization process

import { Transform } from "class-transformer";

// Importing the UserDto class from the user.dto file
// This class is a Data Transfer Object (DTO) used to define the structure of user-related data
import { UserDto } from "./user.dto";

export class EventDto {
  id: number;
  title: string;
  description: string;
  location: string;
  date: Date;

  @Transform(({ value }) => {
    return value.map((user) => {
      const { password, friends, ...safeUser } = user;
      return safeUser;
    });
  })
  users: UserDto[];
}
```

### Properties

- `id`: Unique identifier for the event.
- `title`: Event name/title.
- `description`: Detailed event description.
- `location`: Physical location where the event takes place.
- `date`: Date and time of the event.
- `users`: List of users participating in the event.

### Key Relationships

- `users`: Many-to-many relationship with [`User DTO`](UserDTO.md), representing the users associated with the event. Sensitive information like passwords and friends list are excluded from the user objects.

### Key Features

- Represents the structure of an event object.
- Ensures sensitive user data is excluded from API responses.
- Facilitates secure and efficient data transfer between client and server.
