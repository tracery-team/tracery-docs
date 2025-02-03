
# Frontend
## Application Entry Point 

File `main.tsx` is the entry point of the application. It is responsible for **rendering the React application**, **setting up global providers**, and **configuring routing**.

## Overview  

The entry file initializes the React application with:
- **React Strict Mode** for best practices.
- **React Query (`@tanstack/react-query`)** for data fetching and caching.
- **React Router (`react-router`)** for client-side navigation.
- **Emotion (`@emotion/react`)** for global styling.


## Key Components and Their Purpose

### Strict mode
Ensures best practices and highlights potential issues in the application during development.
It helps identify unsafe lifecycle methods and deprecated APIs.

```tsx
import { StrictMode } from 'react'

<StrictMode>
  {/* Application Components Here */}
</StrictMode>
```

### Query Client Provider

Provides API request management using React Query `(@tanstack/react-query)`.
It enables caching, background synchronization, and efficient data fetching.

```tsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'

const queryClient = new QueryClient()

<QueryClientProvider client={queryClient}>
  {/* Application Components Here */}
</QueryClientProvider>
```
### React router (Client-Side Navigation)
Manages application routing using React Router `(react-router)`.
It defines paths for navigation between different pages.

```tsx
import { BrowserRouter, Route, Routes } from 'react-router'

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

```
### Global Styles (Emotion)

Applies global CSS styles using Emotion `(@emotion/react)`.
This ensures consistent styling across the application.

```tsx
import { css, Global } from '@emotion/react'

<Global
  styles={css`
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
  `}
/>
```
# Pages
# Login Page Documentation  

## Overview  
The **Login Page** allows users to authenticate and gain access to the application. It integrates with the authentication API using the `useLoginAPI` hook.  

## Components Involved  
- **Login Form**: Captures user credentials.  
- **API Integration (`useLoginAPI`)**: Sends login requests to the backend.  
- **Navigation**: Redirects users upon successful authentication.  
- **Error Handling**: Displays error messages if login fails.  


## Login Component (`Login.tsx`)  

### Route Protection  
Ensures that **only unauthenticated users** can access the login page. If an authenticated user tries to access it, they are redirected.  

```tsx
import { useRouteProtection } from '../../hooks/useRouteProtection'

useRouteProtection({ authorized: false })
```

### Form Handling

The login form is managed using `react-hook-form`. This allows for efficient form validation and submission.

```tsx
import { useForm } from 'react-hook-form'

const { register, handleSubmit, formState, reset } = useForm<LoginFormValues>()
```
- **register** binds input fields to state.
- **handleSubmit** processes form submission.
- **formState** tracks validation errors.
- **reset** clears the form after a successful login.

### API Call Integration

The login request is handled using the `useLoginAPI` hook, which manages authentication via React Query.

```tsx
import { useLoginAPI } from './api'

const { mutate, error, isError, isSuccess } = useLoginAPI()

```
- **mutate(data)** sends the login request.
- **isSuccess** indicates whether authentication was successful.
- **isError** determines if an error occurred, triggering an error message.

### Redirect After Login
Once the user is authenticated, they are redirected to the main page `(/)`.

```tsx
import { useEffect } from 'react'
import { useNavigate } from 'react-router'

const navigate = useNavigate()

useEffect(() => {
  if (!isSuccess) return
  reset()
  navigate(REDIRECT_ROUTE)
}, [isSuccess, navigate, reset])

```

### Handling Login Submission

When the form is submitted, `handleLogin` calls `mutate` with the user's login credentials.

```tsx
const handleLogin: SubmitHandler<LoginFormValues> = useCallback(
  data => {
    mutate(data)
  },
  [mutate],
)
```

### Error handling

If the authentication request fails, an error message is displayed on the screen.

```tsx
{isError && (
  <Typography variant="subtitle1" color="error">
    {extractMessage(error)}
  </Typography>
)}
```

# SignUp Page 

## Purpose
The `SignUp` component allows users to create a new account. It handles the form submission, validation of the input fields, and navigation to the login page upon successful registration.


## Component Breakdown

### Form Handling
The sign-up form uses react-hook-form for form management. The `useForm` hook is used to manage the form state, including registration of input fields, validation, and submission handling.

```tsx
const { register, handleSubmit, formState, reset } = useForm<SignUpData>()
```
- register: Binds the input fields to form state.
- handleSubmit: Handles form submission.
- formState: Contains validation errors.
- reset: Resets the form after successful submission.

### SignUp API Call

The form submission triggers an API call to register the user. The `useSignUpAPI` hook handles the sign-up API call.
```tsx
const { mutate, error, isError, isSuccess } = useSignUpAPI()

```
- mutate(data): Executes the sign-up request.
- isSuccess: Indicates if the request was successful.
- isError: Indicates if an error occurred during submission.
- error: The error object containing the error message.

### Navigation
Once the user has successfully signed up, they are redirected to the login page.
```tsx
const navigate = useNavigate()
const navigateToLogin = useCallback(() => {
  navigate(LOGIN_ROUTE)
}, [navigate])

useEffect(() => {
  if (!isSuccess) return
  reset()
  navigateToLogin()
}, [isSuccess, navigateToLogin, reset])

```
### Error handling
If there is an error during the sign-up process, an error message is displayed.

```tsx
 there is an error during the sign-up process, an error message is displayed.
 ```

## Main Component

### File: `Main.tsx`

### Description:
The `Main` component serves as the layout for the authenticated user's dashboard. It uses `useRouteProtection` for authentication and displays three sections: user information, friends, and events.

### Key Responsibilities:
- **Route Protection**: Ensures the user is authorized to access the page.
- **Layout**: Uses Material UI's `Box` for the overall layout, with a grid for organizing content into two columns and two rows.
- **Component Integration**: Renders `Appbar`, `UserInformation`, `UserFriends`, and `UserEvents` components.

### Behavior:
- **Route Protection**: Redirects unauthorized users.
- **Layout**: A flexbox container with a grid for responsive display of sections.
- **Rendering**: Conditionally displays components based on authentication status.

### Usage:
- Used as the main page for logged-in users, displaying user-specific content.

### Future Enhancements:
- Improve mobile responsiveness.
- Refine error handling for authorization issues.

## UserEvents Component

### File: `UserEvents.tsx`

### Description:
The `UserEvents` component allows users to search for events and view them based on whether they are participating or not. It integrates with APIs for fetching events, adding/removing events, and displaying profile information. It also features a responsive layout with a search bar and grid of event cards.

### Key Responsibilities:
- **Event Search**: Provides a search bar to filter events.
- **API Integration**: Fetches event data, adds/removes events using API hooks.
- **Event Display**: Displays events in a grid layout with options to add or remove participation.


#### **State and Search Logic**
```js
const [search, setSearch] = useState('')
const handleSearchChange = (event: React.ChangeEvent<HTMLInputElement>) => {
  setSearch(event.target.value)
}
const debouncedSearch = useDebounce(search, 500)
```
- State Setup: The component uses useState to track the search query (search).
- Debounce: The search input is debounced using the useDebounce hook to avoid excessive API calls.

### API data fetching
Fetches event data and profile info, categorizes events, and handles adding/removing participation.
```tsx
const { data: searchEvents, error, isLoading } = useSearchEventsAPI(1, debouncedSearch)
```

Event Fetching: The useSearchEventsAPI hook fetches the events based on the debounced search value.
### Profile Data and Event Categorization

```tsx
const { data: profileInfo } = useProfileInfoAPI()
const myEventIDs = useMemo(() => {
  if (!profileInfo) return []
  return profileInfo.events.map(({ id }) => id)
}, [profileInfo])
```
Profile Information: Fetches profile data using useProfileInfoAPI.
Event Categorization: Events are categorized into myEventIDs (events the user is participating in) and myFriendIDs (events friends are involved in).

### Event Categorization Based on Participation
```tsx const [eventsParticipated, eventsNotParticipated] = useMemo(() => {
  if (!searchEvents) return [[], []]
  const participated = []
  const notParticipated = []
  for (const event of searchEvents) {
    if (myEventIDs.includes(event.id)) {
      participated.push(event)
    } else {
      notParticipated.push(event)
    }
  }
  return [participated, notParticipated]
}, [searchEvents, myEventIDs])
```
Categorization: Events are divided into participated and notParticipated based on whether the user is participating.

## UserFriends Component

### File: `UserFriends.tsx`

### Description:
The `UserFriends` component allows users to view their friends, search for potential friends, and manage their friend list. It integrates with APIs for adding/removing friends and searching for potential friends.

### Key Responsibilities:
- **Friend Management**: Allows users to add or remove friends.
- **Friend Search**: Enables searching for potential friends with a debounced search input.
- **Profile Navigation**: Users can view profiles of friends or potential friends.

### Code Explanation:

#### 1. **State and Search Logic**
```tsx
const [search, setSearch] = useState('')
const handleChangeSearch = useCallback((event: SearchChange) => {
  const value = event.target.value
  setSearch(value)
}, [])
```
State Setup: Uses useState to manage the search query.
Search Handling: The handleChangeSearch function updates the search query when the user types in the search field.

### Debounced Search Input
Debounced Input: The useDebounce hook is used to avoid making API calls with every keystroke, instead, it waits for 500ms after the user stops typing.
```tsx
const debouncedSearch = useDebounce(search, 500)

```
### API Data Fetching
Potential Friends Search: Fetches potential friends from an API, passing the debounced search query.
```tsx
const { data, error, isLoading } = usePotentialFriendSearchAPI(1, debouncedSearch)
```
###  Profile Information and Friend Categorization
Profile Data: Fetches profile information, and extracts the list of friends (friendIDs).
Friend Categorization: Friends are categorized into two arrays: friends (users already friends) and potential (users not yet added).
```tsx
const { data: profileInfo } = useProfileInfoAPI()
const friendIDs = useMemo(() => {
  if (!profileInfo) return []
  return profileInfo.friends.map(({ id }) => id)
}, [profileInfo])

```

###  Friend List Rendering
```tsx <List sx={{ flex: 1, height: '100%' }}>
  {friends.map((user, index) => (
    <ListItem key={index}>
      <ListItemAvatar>
        <Tooltip title="See user's profile">
          <Avatar sx={{ cursor: 'pointer' }} onClick={() => gotoUserDetails(user.id)} alt="User Icon" src={noUser} />
        </Tooltip>
      </ListItemAvatar>
      <ListItemText primary={user.nickname} secondary={`${user.firstName} ${user.lastName}`} />
      <ListItemSecondaryAction>
        <Button color="error" onClick={() => removeFriend(user.id)}>Remove</Button>
      </ListItemSecondaryAction>
    </ListItem>
  ))}
  {potential.map((user, index) => (
    <ListItem key={index}>
      <ListItemAvatar>
        <Tooltip title="See user's profile">
          <Avatar sx={{ cursor: 'pointer' }} onClick={() => gotoUserDetails(user.id)} alt="User Icon" src={noUser} />
        </Tooltip>
      </ListItemAvatar>
      <ListItemText primary={user.nickname} secondary={`${user.firstName} ${user.lastName}`} />
      <ListItemSecondaryAction>
        <Button onClick={() => addFriend(user.id)}>Add</Button>
      </ListItemSecondaryAction>
    </ListItem>
  ))}
</List>
```
Friends List: Displays two lists — one for friends and another for potential friends. Each list item includes an avatar, user info (nickname and name), and an action button to add or remove friends.

### Event Handlers for Friend Actions
``tsx
<Button color="error" onClick={() => removeFriend(user.id)}>Remove</Button>
<Button onClick={() => addFriend(user.id)}>Add</Button>
```
Add/Remove Friend: Buttons are displayed to either add a potential friend or remove an existing friend. The addFriend and removeFriend API functions handle these actions.

### Profile Navigation
```tsx
const navigate = useNavigate()
const gotoUserDetails = useCallback((userId: number) => {
  navigate(`/info/user/${userId}`)
}, [])
```
Navigate to User Profile: Clicking on a user’s avatar or name navigates to their profile page, where more information about the user can be viewed.
## UserInformation Component

### File: `UserInformation.tsx`

### Description:
The `UserInformation` component displays the user's profile information, including their avatar, name, and email. It also includes a logout button that allows the user to log out by clearing their JWT token.

### Key Responsibilities:
- **Profile Display**: Shows the user's nickname, full name, and email address.
- **Logout**: Provides a button for logging out, which clears the authentication token.
- **Loading/Error Handling**: Displays a loading spinner or an error message based on the API response status.

### Code Explanation:

#### **State and API Data Fetching**
API Call: Fetches the profile information of the logged-in user.
Loading/Error Handling: isLoading is used to display a loading spinner while the data is being fetched, and error is used to show an error message if the fetch fails.
```tsx
const { data, error, isLoading } = useProfileInfoAPI()
```

### JWT Management and Logout
JWT Token: setToken is used to clear the JWT token when the user logs out, effectively logging them out of the application.

```tsx
const { setToken } = useJWT()

const handleLogout = useCallback(() => {
  setToken(null)
}, [setToken])

```

## EventInfoPage Component

### File: `EventInfoPage.tsx`

### Description:
The `EventInfoPage` component displays detailed information about a specific event, including its title, description, location, and date. It also lists friends who are participating in the event, with an option for the logged-in user to join or leave the event.

### Key Responsibilities:
- **Event Information Display**: Shows the event details such as title, description, location, and date.
- **Friends Participating**: Displays a list of friends who are participating in the event.
- **Join/Leave Event**: Allows the logged-in user to join or leave the event.
- **Dynamic Background**: Generates a unique grid-based background with randomized colors.

### Code Explanation:

#### 1. **State and Memoization**

```tsx
const myEventIDs = useMemo(() => {
  if (!profileInfo) return []
  return profileInfo.events.map(({ id }) => id)
}, [profileInfo])

const myFriendIDs = useMemo(() => {
  if (!profileInfo) return []
  return profileInfo.friends.map(({ id }) => id)
}, [profileInfo])
```

myEventIDs: Memoizes the list of event IDs the user is participating in.
myFriendIDs: Memoizes the list of friend IDs of the logged-in user.

### Event Data Fetching
```tsx
const { id } = useParams()
const { data: eventData } = useEventInfoAPI(id)
```
useEventInfoAPI: Fetches the event data based on the event ID obtained from the URL parameters (id).

### Determining Participation
```tsx
const friendsParticipating = useMemo(() => {
  if (!eventData) return []
  return eventData.users.filter(({ id }) => myFriendIDs.includes(id))
}, [eventData, myFriendIDs])

const meParticipating = useMemo(() => {
  if (!eventData) return false
  return myEventIDs.includes(eventData?.id)
}, [eventData, myEventIDs])
```
friendsParticipating: Filters the event participants to include only the user's friends.
meParticipating: Determines if the logged-in user is participating in the event by checking the user's event list.

### EventInfoPage Component
File: EventInfoPage.tsx
Description:
The EventInfoPage component displays detailed information about a specific event, including the event's description, location, date, and time. It also shows a list of friends who are participating in the event. Users can add or remove themselves from the event.

key Responsibilities:
Fetch Event Data: Uses the useEventInfoAPI hook to fetch event details.
Fetch Profile Data: Uses the useProfileInfoAPI hook to fetch the user's profile information.
Add/Remove Event Participation: Uses the useAddEventAPI and useRemoveEventAPI hooks to handle event participation.
Display Event Details: Shows event title, description, location, and date.
Display Friends Participating: Lists friends who are participating in the event.

### State and Memoization
myEventIDs: Memoizes the list of event IDs the user is participating in.
myFriendIDs: Memoizes the list of friend IDs of the logged-in user.
```tsx
const myEventIDs = useMemo(() => {
  if (!profileInfo) return []
  return profileInfo.events.map(({ id }) => id)
}, [profileInfo])

const myFriendIDs = useMemo(() => {
  if (!profileInfo) return []
  return profileInfo.friends.map(({ id }) => id)
}, [profileInfo])
```
### Event Data Fetching
useEventInfoAPI: Fetches the event data based on the event ID obtained from the URL parameters (id).

```tsx
const { id } = useParams()
const { data: eventData } = useEventInfoAPI(id)
```
### Determining Participation
friendsParticipating: Filters the list of users participating in the event to include only friends.
meParticipating: Checks if the logged-in user is participating in the event.
```tsx
const friendsParticipating = useMemo(() => {
  if (!eventData) return []
  return eventData.users.filter(({ id }) => myFriendIDs.includes(id))
}, [eventData, myFriendIDs])

const meParticipating = useMemo(() => {
  if (!eventData) return false
  return myEventIDs.includes(eventData?.id)
}, [eventData, myEventIDs])
```