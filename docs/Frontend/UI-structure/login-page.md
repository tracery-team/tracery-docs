# **Login Page (`Login.tsx`)**

### Purpose
The Login page is used to authenticate users with their login credentials (username and password).

### Structure

- **Container (`Box`)**
  - A full-screen container (`100vw` by `100vh`), centered with a grid layout.

- **Welcome Section (`Stack`)**
  - Title (`Typography`): `"Welcome to Tracery"`
  - Subtitle (`Typography`): `"Keep in contact with your friends offline"`

- **Form Section (`Stack`)**
  - `TextField` for `Login`
    - Required input field for username.
    - Validation error handling.
  - `TextField` for `Password`
    - Required input field for password.
    - Validation error handling.
  - `Button`: `"Login"`
    - Submits the form.
  - `Button`: `"Don't have an account?"`
    - Navigates to the SignUp page.

- **Error Message (`Typography`)**
  - Display error message if the login fails.