# UserEntity

The `User Entity` is strictly connected to the [`User DTO`](UserDTO.md)

## Format

```typescript
@Entity()
export class UserEntity {
  @PrimaryGeneratedColumn()
  id: number;

  @Column({ unique: true })
  nickname: string;

  @Column()
  firstName: string;

  @Column()
  lastName: string;

  @Column({ unique: true })
  email: string;

  @Column()
  password: string;

  @ManyToMany(() => UserEntity, (user) => user.friends)
  @JoinTable()
  friends: UserEntity[];

  @ManyToMany(() => EventEntity, (event) => event.users)
  @JoinTable()
  events: EventEntity[];
}
```

### Properties

- `id`: Unique identifier for the user.
- `nickname`: User's unique nickname/username.
- `firstName`: User's first name.
- `lastName`: User's last name.
- `email`: User's unique email address.
- `password`: Hashed password (never exposed in API responses).
- `friends`: Many-to-many self-referential relationship for friends.
- `events`: Many-to-many relationship with events the user participates in.

### Key Relationships

It contains the essential user information and manages relationships with friends and events.

The [`UserEntity`] has many-to-many relationships with:

1. friends (self-referential relationship to other users)

```typescript
@ManyToMany(() => UserEntity, user => user.friends)
@JoinTable()
friends: UserEntity[]

// This code defines a many-to-many relationship between the current entity and the UserEntity.
// The @ManyToMany decorator specifies that each instance of the current entity can have many UserEntity instances as friends, and vice versa.
// The @JoinTable decorator indicates that a join table should be created to manage this relationship.
```

2. events (bidirectional relationship with [`EventEntity`](EventEntity.md))

```typescript
  @ManyToMany(() => EventEntity, event => event.users)
  @JoinTable()
  events: EventEntity[]

// This code defines a many-to-many relationship between the current entity and the EventEntity.
// The @ManyToMany decorator specifies the relationship, and the @JoinTable decorator indicates that a join table should be created to manage the relationship.
// The 'events' property will hold an array of EventEntity instances associated with the current entity.
```

It also includes the password hashing logic with bcrypt.

```typescript
// The @BeforeInsert and @BeforeUpdate decorators are used to run the hashPassword method before inserting or updating the entity in the database.
@BeforeInsert()
@BeforeUpdate()
async hashPassword() {
  // Generate a salt with 10 rounds for hashing the password
  const salt = await bcrypt.genSalt(10)
  // Hash the password with the generated salt and assign it to the password field
  this.password = await bcrypt.hash(this.password, salt)
}

// This method validates the input password by comparing it with the hashed password stored in the database.
validatePassword(inputPassword: string): Promise<boolean> {
  // Compare the input password with the stored hashed password
  return bcrypt.compare(inputPassword, this.password)
}
```

### Key Features

- User management with unique identifiers, nicknames, and email addresses.
- Password hashing using bcrypt for enhanced security.
- Many-to-many relationships with friends and events.
- Automatic password hashing before saving to the database.
- Validation method for comparing input passwords with stored hashed passwords.
