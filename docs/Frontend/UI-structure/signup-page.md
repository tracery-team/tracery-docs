# **SignUp Page (`SignUp.tsx`)**

### Purpose
The SignUp page allows users to create a new account by providing their details.

### Structure

- **Container (`Box`)**
  - Full-screen layout, centered using grid.

- **Heading Section (`Stack`)**
  - Title (`Typography`): `"Create an Account"`
  - Subtitle (`Typography`): `"See what Tracery can offer for you"`

- **Form Section (`Stack`)**
  - `TextField` for `Nickname`, `First Name`, `Last Name`, `Email`, and `Password`
    - Validation for each field (e.g., required, pattern matching).
  - `Button`: `"Sign Up"`
    - Submits the form.
  - `Button`: `"Already have an account?"`
    - Navigates to the Login page.

- **Error Message (`Typography`)**
  - Display error message if signup fails.