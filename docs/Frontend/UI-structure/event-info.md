# **Event Information Page (`EventInfoPage.tsx`)**

### Purpose
Displays detailed information about a specific event, including the description, participants, and the ability to add/remove the event from the user's list.

### Structure

- **Button:**
  - A button to navigate back to the homepage or previous page.

- **Event Title Section:**
  - The title of the event and an action button to either add or remove the event from the user's list.

- **Main Content:**
  - Contains sections for event details (e.g., location, date, description) and a list of participants (friends attending the event).

### Features

- A grid of colored squares is used for a dynamic background.
- Conditional rendering displays messages when no friends are participating or when there’s an issue loading data.

### API Hooks Used

- Hooks to handle adding/removing events from the user's event list.
- Custom hooks for fetching event data.
