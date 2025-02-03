# Backend

## Backend Layer and Data

The backend layer of the project is responsible for handling the logic of the events with the users, as well as users with users and users with events. Through the use of **Entity Models** ([here](data/EventEntity.md) and [here](data/UserEntity.md)) as well as **DTOs (Data Transfer Objects)** ([here](data/EventDTO.md) and [here](data/UserDTO.md)) the backend communicates with the API and the PostgreSQL database to perform the CRUD operations. It is an intermediary between the frontend and the database, ensuring that data is processed and served correctly.

For more information on the data layer, please refer to the [Data Structure](data.md) section.

## Modules

Modules are distinct components within the backend layer that encapsulate specific functionality and responsibilities. Each module focuses on a particular aspect of the project, ensuring a clear separation of concerns and improving maintainability. They interact with each other to provide a cohesive backend system that supports the overall application. In the project, the following modules have been implemented:

1. [Auth](modules/AuthModule.md) (authentication and authorization processes),
2. [Event](modules/EventModule.md) (manages all [event](data/EventEntity.md)-related operations)
3. [Profile](modules/ProfileModule.md) (responsible for managing [user](data/UserEntity.md) profiles, e.g. update personal information)
4. [User](modules/UserModule.md) ([user](data/UserEntity.md) management, e.g. user registration)

More information on the modules can be found in the [Modules](modules.md) section.

## Algorithms

Throughout the code, the Levenshtein algorithm was used to measure the similarity between strings, which is particularly useful for functionalities such as user search and event matching. For more details on the implementation and usage of the Levenshtein algorithm, please refer to the [Levenshtein Algorithm](levenshtein.md) section.
