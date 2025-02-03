
# State Management

#### Overview

The project uses **Zustand** for state management. Zustand is a small, fast, and scalable state-management solution that provides a simple API for managing global state in React applications.

#### JWT Token Management

The `useJWT` hook is used to manage the JWT token, which is stored in local storage and used for authentication purposes.

##### **useJWT.ts**

```tsx
import { registerJWT, unregisterJWT } from '../http-client'
import { create } from 'zustand'

export const JWT_LOCALSTORAGE_KEY = 'tracery_jwt_token'

type TokenStorage = {
  token: string | null
  setToken: (newToken: string | null) => void
}

export const useJWT = create<TokenStorage>(set => ({
  token: window.localStorage.getItem(JWT_LOCALSTORAGE_KEY),
  setToken: newToken => {
    if (newToken === null) {
      window.localStorage.removeItem(JWT_LOCALSTORAGE_KEY)
      unregisterJWT()
    } else {
      window.localStorage.setItem(JWT_LOCALSTORAGE_KEY, newToken)
      registerJWT(newToken)
    }
    set(prev => ({
      ...prev,
      token: newToken,
    }))
  },
}))
```
# Global State Management

Zustand is used to manage global state across the application. The state is defined using the `create` function from Zustand, which provides a simple and scalable way to manage state.

## Benefits of Using Zustand

- **Simplicity**: Zustand provides a simple API for managing state without the need for boilerplate code.
- **Performance**: Zustand is fast and efficient, making it suitable for large-scale applications.
- **Scalability**: Zustand's API is designed to be scalable, allowing you to manage complex state logic with ease.