## Routing

### Overview
The routing in this project is handled using `react-router`. The main entry point for the routing setup is in the `main.tsx` file. The application uses a nested routing structure to manage different pages and their respective routes.

### Main Routing Configuration
The main routing configuration is defined in the `main.tsx` file. It sets up the routes for the main pages of the application. Below is the routing setup:

```tsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import { BrowserRouter, Route, Routes } from 'react-router'
import Main from './pages/Main'
import Login from './pages/Login'
import SignUp from './pages/SignUp'
import EventInfo from './pages/EventInfo'
import UserInfo from './pages/UserInfo'
import { css, Global } from '@emotion/react'
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'

const queryClient = new QueryClient()

createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <QueryClientProvider client={queryClient}>
      <BrowserRouter>
        <Routes>
          <Route path="/" element={<Main />} />
          <Route path="auth">
            <Route path="login" element={<Login />} />
            <Route path="sign-up" element={<SignUp />} />
          </Route>
          <Route path="info">
            <Route path="event/:id" element={<EventInfo />} />
            <Route path="user/:id" element={<UserInfo />} />
          </Route>
        </Routes>
      </BrowserRouter>
    </QueryClientProvider>
    <Global
      styles={css`
        * {
          margin: 0;
          padding: 0;
          box-sizing: border-box;
        }
      `}
    />
  </StrictMode>
)  
```
### Route Protection

The project uses a custom hook `useRouteProtection` to protect routes based on the user's authentication status. This hook redirects users to the appropriate routes depending on whether they are authorized or not. Below is the `useRouteProtection` implementation:

```tsx
import { useNavigate } from 'react-router'
import { useJWT } from './useJWT'
import { useEffect } from 'react'

const AUTHORIZED_REDIRECT_ROUTE = '/'
const UNAUTHORIZED_REDIRECT_ROUTE = '/auth/login'

export const useRouteProtection = ({ authorized }: { authorized: boolean }) => {
  const { token } = useJWT()
  const navigate = useNavigate()

  useEffect(() => {
    // if authorization is required
    if (authorized) {
      if (token !== null) return
      navigate(UNAUTHORIZED_REDIRECT_ROUTE)
      return
    }
    // if there should be no authorization
    if (token === null) return
    navigate(AUTHORIZED_REDIRECT_ROUTE)
  }, [token, authorized])
}
```

### Page Components

The following are the main page components and their respective routes:

- **Main Page:**
  - **Route:** `/`
  - **Component:** `Main`

- **Login Page:**
  - **Route:** `/auth/login`
  - **Component:** `Login`

- **Sign Up Page:**
  - **Route:** `/auth/sign-up`
  - **Component:** `SignUp`

- **Event Info Page:**
  - **Route:** `/info/event/:id`
  - **Component:** `EventInfo`

- **User Info Page:**
  - **Route:** `/info/user/:id`
  - **Component:** `UserInfo`
