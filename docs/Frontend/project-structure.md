# Project Structure

## src/

The `src/` folder contains the main source code of the frontend application.

- **assets/**: Contains static assets like images, icons, fonts, etc. These are files that are directly referenced in the app (images, background images).
  
- **hooks/**: This folder contains custom React hooks used throughout the application. It includes:
  - `useJWT`: Custom hook for handling JWT token management.
  - `useDebounce`: Custom hook for debouncing input values.
  - `useRouteProtection`: Custom hook that uses React Router's useNavigate and the useJWT hook to protect routes based on the user's authentication status.
  
- **pages/**: Contains all page components that represent different routes in the application. Page components include:
  - `Login`: Login page component.
  - `Main`: Main page with user-related information and events.
  - `EventInfo`: Page displaying detailed information about an event.
  - `SignUp`: Page for users to sign up for an account.

### Other Files in the `src/` folder:
  
- **http-client.ts**: 
  - Contains the setup and configuration for making HTTP requests using Axios.
  - Sets base URL and defines common request/response interceptors for managing tokens and error handling.

- **lib.ts**: 
  - Utility functions and common helper functions used across the project.
  - Contains functions like `intoErrorAPI` and `propagateErrorAPI` for error handling.

- **main.tsx**: 
  - The entry point of the application where React is rendered.
  - Contains routing logic, global styles, and the main app layout.
  
- **vite-env.d.ts**: 
  - TypeScript definition file used for Vite environment variables.


## Configuration Files

### **package.json**:
  - Contains the project's metadata (e.g., name, version, dependencies, scripts) and configuration for the NPM package manager.
  - Defines build and development scripts.

### **tsconfig.json**:
  - TypeScript configuration file that sets up TypeScript compiler options (e.g., strict mode, module resolution, etc.).

### **tsconfig.app.json**:
  - TypeScript configuration specific to the application code, extending the base `tsconfig.json`.

### **tsconfig.node.json**:
  - TypeScript configuration specific for Node.js, ensuring that Node.js modules and types are correctly recognized.

### **vite.config.ts**:
  - Vite build configuration for setting up the development environment and building the project for production.
  - Defines configurations like entry points, output paths, and plugins.

### **eslint.config.js**:
  - ESLint configuration for linting JavaScript and TypeScript files.
  - Ensures consistent code style and best practices.

### **.prettierrc.mjs**:
  - Configuration file for Prettier, which automatically formats the code to ensure a consistent code style.
  
### **.gitignore**:
  - Specifies which files and directories should be ignored by Git. Commonly used to ignore build directories, node modules, and environment files.

### **index.html**:
  - The HTML template file for the application.
  - Defines the root element and other meta information for the application.