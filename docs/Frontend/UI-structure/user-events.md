# **User Events(`UserEvents.tsx`)**

### Purpose
Displays a list of events available in the application, allowing users to participate or remove themselves from events.

### Structure

- **`TextField`**
    - Search bar for finding events.
    - Debounced input allows for optimized API calls to fetch events as the user types.
- **`Grid`**
    - The list of events is displayed using a responsive grid layout.
    - Each event card contains:
    - **Event Title**: Clickable link to navigate to the event's detailed page.
    - **Event Description**: Displays a brief description of the event.
    - **Event Icon**: A small icon indicating whether the user is participating in the event.
    - **Add/Remove Button**: A button that allows users to add or remove themselves from events.

- **Features**
  - **CircularProgress** is shown while events are loading.
  - **Alert messages** are displayed for error handling or when no events are found.
  - **Memoization** is used to optimize performance by preventing unnecessary re-renders.

- **API Hooks Used**
  - **useSearchEventsAPI**: Fetches events based on a debounced search term.
  - **useAddEventAPI & useRemoveEventAPI**: Allows users to add or remove events from their list.
  - **useProfileInfoAPI**: Fetches the user's profile and events they are participating in.
