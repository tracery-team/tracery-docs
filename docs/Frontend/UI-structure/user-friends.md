**User Friends (UserFriends.tsx)**

### Purpose
Displays the user's friends and potential friends, allowing the user to manage their connections (add or remove friends).

### Structure

- **TextField:**
  - Search bar for finding friends by name or username.
  - Debounced input optimizes search results.

- **List:**
  - A list of friends and potential friends is displayed using `ListItem` components.
  - Each friend or potential friend has:
    - **Avatar:** A clickable avatar that navigates to the user's profile.
    - **Name:** Displayed as the user's nickname and full name.
    - **Action Buttons:** Allows users to add or remove friends.

### Features

- `CircularProgress` is displayed during loading.
- `Alert` displays an error or message if no friends are found.
- Memoization for optimized performance.
- `useNavigate` is used for navigating to user profiles.

### API Hooks Used

- `usePotentialFriendSearchAPI`: Fetches users who may be potential friends based on search input.
- `useProfileInfoAPI`: Fetches the user's profile and their current friends.
- `useAddFriendAPI` & `useRemoveFriendAPI`: Allows adding or removing friends.
