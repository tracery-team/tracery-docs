# EventEntity

The `Event Entity` is strictly connected to the [`Event DTO`](EventDTO.md)!

## Format

```typescript
@Entity()
export class EventEntity {
  @PrimaryGeneratedColumn()
  id: number;

  @Column({ unique: true })
  title: string;

  @Column()
  description: string;

  @Column()
  location: string;

  @Column({ type: "timestamptz" })
  date: Date;

  @ManyToMany(() => UserEntity, (user) => user.events)
  users: UserEntity[];
}
```

### Properties

- `id`: Unique identifier for the event.
- `title`: Event name/title (must be unique).
- `description`: Detailed event description.
- `location`: Physical location where the event takes place.
- `date`: Date and time of the event (stored with timezone).
- `users`: Many-to-many relationship with participating users.

### Key Relationships

1. Friends: Self-referential many-to-many relationship allowing [`users`](UserEntity.md) to connect with other users
2. Events: Many-to-many relationship with events, tracking which events a user participates in

```typescript
/**
 * Many-to-Many relationship with the UserEntity.
 * This property represents the users associated with the events. **/
 @type {UserEntity[]}
  @ManyToMany(() => UserEntity, user => user.events)
  users: UserEntity[]
```

The [`EventEntity`] forms the foundation of the application's social features and event participation system.

### Key Features

- Events are stored in the PostgreSQL database.
- Uses TypeORM decorators for database mapping.
- Enforces unique event titles to prevent duplicates.
- Maintains many-to-many relationship with users.
- API responses use EventDto, filtering sensitive user data.
- Allows users to create, join, and track events.
