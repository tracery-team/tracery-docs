# **Main Page (`Main.tsx`)**

### Purpose
The Main page is the dashboard after successful login, displaying the user's profile information, friends list, and events.

### Structure

- **Container (`Box`)**
  - Full-screen layout with `flex` display, containing the Appbar and main content area.

- **Appbar (`Appbar.tsx`)**
  - Navigation bar at the top with application-wide navigation options.

- **Main Content Section (`Box`)**
  - **User Information (`UserInformation.tsx`)**: Displays profile data.
  - **User Friends (`UserFriends.tsx`)**: Displays the list of friends.
  - **User Events (`UserEvents.tsx`)**: Displays the list of events the user is involved in.
