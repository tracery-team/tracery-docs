# **Appbar (`Appbar.tsx`)**

### Purpose
The Appbar serves as the navigation header for the application, containing the app's title.

### Structure

- **`AppBar`**
    - Positioned statically at the top of the page.
    - Contains a toolbar that houses the title of the app (`Dashboard`).
- **Back Button (`Button`)**
    - A button to navigate back to the homepage.

- **Typography**
    - Displays the title of the app (`Dashboard`).
    - Uses `variant="h4"` to display the title in a prominent, large font.

- **Features**
    - Static positioning ensures it stays at the top of the page during scroll.
    - Wrapped in memo to prevent unnecessary re-renders.