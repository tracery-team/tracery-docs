


# API Integration

## Overview

The project uses Axios for making HTTP requests to the backend API. The Axios instance is configured in the `http-client.ts` file, which sets up the base URL, timeout, and common headers for all requests. Additionally, the project includes custom hooks for interacting with various API endpoints.

## Axios Configuration

The Axios instance is configured in the `http-client.ts` file. This configuration includes the base URL for the API, a timeout setting, and default headers for all requests.

### `http-client.ts`

```tsx
import axios from 'axios'

export const client = axios.create({
  baseURL: '/api',
  timeout: 1000,
  headers: {
    'Content-Type': 'application/json',
  },
})

export const registerJWT = (token: string) => {
  client.defaults.headers.common['Authorization'] = `Bearer ${token}`
}

export const unregisterJWT = () => {
  delete client.defaults.headers.common['Authorization']
}
```

## Error handling

The project includes a utility file `lib.ts` for handling API errors. This file defines the structure of API error responses and provides utility functions for managing errors.

```tsx
export type ErrorAPI = {
  message: string | string[]
  error?: string
  statusCode: number
}
```

## Custom Hooks for API Calls

The project uses custom hooks to interact with various API endpoints. These hooks encapsulate the logic for making API requests and managing the response data.

```tsx
import { useMutation } from '@tanstack/react-query'
import { useJWT } from '../../hooks/useJWT'
import { ErrorAPI, intoErrorAPI, propagateErrorAPI } from '../../lib'
import { useCallback } from 'react'
import { client } from '../../http-client'

const API_LOGIN_ROUTE = '/auth/login'

export type LoginData = {
  login: string
  password: string
}

export type LoginResponse = {
  access_token: string
}

const isLoginResponse = (obj: any): obj is LoginResponse => {
  return (
    obj !== null &&
    obj !== undefined &&
    'access_token' in obj &&
    typeof obj.access_token === 'string'
  )
}

export const useLoginAPI = () => {
  const { setToken } = useJWT()

  const mutationFn = useCallback(
    async (data: LoginData) => {
      try {
        const { data: responseData } = await client.post(API_LOGIN_ROUTE, data)
        if (isLoginResponse(responseData)) {
          setToken(responseData.access_token)
          return responseData
        }
        throw intoErrorAPI(responseData)
      } catch (error: any) {
        throw propagateErrorAPI(error)
      }
    },
    [setToken],
  )

  return useMutation<LoginResponse, ErrorAPI, LoginData>({
    mutationFn,
  })
}
```

## Example Usage

Here's an example of how to use the `useLoginAPI` hook in a component:

```tsx
import { Box, Button, Stack, TextField, Typography } from '@mui/material'
import { memo, useCallback, useEffect } from 'react'
import { SubmitHandler, useForm } from 'react-hook-form'
import { useLoginAPI } from './api'
import { useNavigate } from 'react-router'
import { extractMessage } from '../../lib'
import { useRouteProtection } from '../../hooks/useRouteProtection'

const REDIRECT_ROUTE = '/'
const SIGNUP_ROUTE = '/auth/sign-up'

type LoginFormValues = {
  login: string
  password: string
}

const Login = () => {
  useRouteProtection({ authorized: false })

  const { register, handleSubmit, formState, reset } =
    useForm<LoginFormValues>()

  const { mutate, error, isError, isSuccess } = useLoginAPI()

  const navigate = useNavigate()
  useEffect(() => {
    if (!isSuccess) return
    reset()
    navigate(REDIRECT_ROUTE)
  }, [isSuccess, navigate, reset])

  type Handler = SubmitHandler<LoginFormValues>
  const handleLogin: Handler = useCallback(
    data => {
      mutate(data)
    },
    [mutate],
  )

  const handleSignUpLinkClick = useCallback(
    (event: React.MouseEvent) => {
      event.preventDefault()
      navigate(SIGNUP_ROUTE)
    },
    [navigate],
  )

  return (
    <Box
      sx={{
        width: '100vw',
        height: '100vh',
        display: 'grid',
        placeItems: 'center',
      }}
    >
      <Stack spacing={4}>
        <Stack spacing={0.5}>
          <Typography variant="h2" component="h1">
            Welcome to Tracery
          </Typography>
          <Typography variant="h5" color="textSecondary">
            Keep in contact with your friends offline
          </Typography>
        </Stack>

        <Stack spacing={1.5}>
          <TextField
            label="Login"
            {...register('login', { required: 'Please, provide your login' })}
            fullWidth
            error={Boolean(formState.errors.login)}
            helperText={formState.errors.login?.message}
          />
          <TextField
            label="Password"
            type="password"
            {...register('password', {
              required: 'Please, provide your password',
            })}
            fullWidth
            error={Boolean(formState.errors.password)}
            helperText={formState.errors.password?.message}
          />
          <Button onClick={handleSubmit(handleLogin)}>Login</Button>
          <Button
            component="a"
            href={SIGNUP_ROUTE}
            onClick={handleSignUpLinkClick}
          >
            Don't have an account?
          </Button>
          {isError && (
            <Typography variant="subtitle1" color="error">
              {extractMessage(error)}
            </Typography>
          )}
        </Stack>
      </Stack>
    </Box>
  )
}

export default memo(Login)

```
## List of API Hooks

- `useLoginAPI`: Hook for logging in a user.
- `useProfileInfoAPI`: Hook for fetching user profile information.
- `useAddFriendAPI`: Hook for adding a friend.
- `useAddEventAPI`: Hook for adding an event.
- `useEventInfoAPI`: Hook for fetching event information.