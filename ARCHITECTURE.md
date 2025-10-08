# Architecture Documentation

## Overview

This document describes the architecture and design patterns used in the Meeting Room Booking application.

## Design Principles

### Clean Architecture

The application follows clean architecture principles with clear separation of concerns:

1. **Presentational Layer** (`src/components/`)
   - Pure components with no business logic
   - Accept data through props
   - Call callbacks for interactions
   - Fully reusable and testable

2. **Business Logic Layer** (`src/hooks/`)
   - Custom React hooks
   - Encapsulate all business logic
   - Handle derived state and calculations
   - Can be easily tested in isolation

3. **Data Layer** (`src/store/`)
   - Redux Toolkit for state management
   - Organized into slices by domain
   - Selectors for derived state
   - Middleware for side effects

4. **Utility Layer** (`src/utils/`)
   - Pure functions
   - No side effects
   - Easily testable
   - Reusable across the app

## Component Structure

### Component Categories

1. **Common Components** (`components/common/`)
   - Button, Modal, Toast, SearchInput, LoadingSpinner
   - Generic, highly reusable components
   - No domain-specific logic

2. **Layout Components** (`components/layout/`)
   - AppLayout, Navbar
   - App shell and navigation
   - Consistent across all pages

3. **Domain Components**
   - `components/rooms/` - Room-specific UI
   - `components/calendar/` - Calendar UI
   - `components/analytics/` - Analytics visualizations
   - `components/forms/` - Form components

4. **Modal Components** (`components/modals/`)
   - BookingModal, RoomModal
   - Manages modal state from Redux
   - Handles form submission and validation

## State Management

### Redux Store Structure

```typescript
{
  rooms: {
    rooms: Room[],
    loading: boolean,
    error: string | null
  },
  bookings: {
    bookings: Booking[],
    loading: boolean,
    error: string | null
  },
  ui: {
    toast: { open, message, type },
    bookingModal: { open, booking, roomId, mode },
    roomModal: { open, roomId }
  }
}
```

### State Flow

1. **User Interaction** → Component callback
2. **Component** → Dispatch action
3. **Action** → Updates Redux state
4. **State Change** → Component re-renders
5. **Middleware** → Side effects (localStorage)

## Custom Hooks

### useRoomStatus
- Calculates current room status
- Determines next status change
- Updates automatically with time

### useRoomFilters
- Manages filter and search state
- Provides filtered room list
- Debounced search

### useAnalytics
- Aggregates analytics data
- Memoized calculations
- Responds to filter changes

### useBookingValidation
- Validates booking data
- Checks for overlaps
- Returns validation errors

### useLocalStorage
- Loads initial data
- Generates mock data if needed
- Hydrates Redux store

## Data Flow

### Creating a Booking

```
1. User clicks calendar slot
   ↓
2. Page dispatches openBookingModal
   ↓
3. BookingModal renders with form
   ↓
4. User fills form and submits
   ↓
5. Form validates using useBookingValidation
   ↓
6. If valid: dispatch addBooking
   ↓
7. Redux updates bookings state
   ↓
8. Middleware saves to localStorage
   ↓
9. Calendar re-renders with new booking
   ↓
10. Toast notification shown
```

## Routing

### Route Structure

```
/ (redirect to /rooms)
  ├── /rooms (Rooms Overview)
  ├── /rooms/:id (Room Detail)
  ├── /rooms/manage (Room Management)
  ├── /analytics (Analytics Dashboard)
  └── * (Not Found)
```

### Route Components

All routes render within `<AppLayout>` which provides:
- Persistent navigation
- Toast notifications
- Consistent styling

## TypeScript Types

### Type Organization

1. **Domain Types** (`types/*.types.ts`)
   - Room, Booking, Analytics types
   - Organized by domain
   - Exported interfaces

2. **Component Props**
   - Defined inline with components
   - Extends base component types when needed
   - Clear prop documentation

## Styling Strategy

### MUI + Tailwind Integration

1. **MUI for Components**
   - Pre-built components (Button, Dialog, etc.)
   - Theme for consistent colors
   - sx prop for component-specific styling

2. **Tailwind for Layout**
   - Grid layouts
   - Flexbox utilities
   - Responsive design
   - Spacing utilities

3. **Integration Points**
   - Tailwind's preflight disabled
   - MUI theme uses Tailwind colors
   - `#root` selector for important styles

## Performance Considerations

### Memoization

1. **Redux Selectors**
   - `createSelector` from Reselect
   - Memoizes expensive calculations
   - Prevents unnecessary re-renders

2. **React Hooks**
   - `useMemo` for derived data
   - `useCallback` for stable callbacks
   - Dependencies properly tracked

### Data Persistence

1. **LocalStorage**
   - Automatic save on state changes
   - Middleware-based persistence
   - Lazy loading on mount

## Future Enhancements

### Backend Integration

The app is designed for easy backend integration:

1. **API Service Layer**
   - Already implemented in `services/api.ts`
   - Axios configured with interceptors
   - Ready for auth tokens

2. **Redux Thunks**
   - Add async thunks to slices
   - Replace local actions with API calls
   - Handle loading/error states

3. **Real-time Updates**
   - WebSocket connection
   - Subscribe to booking changes
   - Update Redux on remote changes

### Authentication

1. **Auth Slice**
   - User state
   - Login/logout actions
   - Token management

2. **Protected Routes**
   - Route guards
   - Role-based access
   - Redirect to login

3. **API Integration**
   - Auth token in headers
   - Refresh token flow
   - Handle 401 responses

## Testing Strategy

### Unit Tests

1. **Utils**
   - Pure functions
   - Easy to test
   - High coverage target

2. **Hooks**
   - React Testing Library
   - Mock Redux store
   - Test logic in isolation

3. **Components**
   - Render tests
   - Interaction tests
   - Snapshot tests

### Integration Tests

1. **Page Flows**
   - Complete user journeys
   - Redux integration
   - Router integration

2. **API Integration**
   - Mock API responses
   - Error handling
   - Loading states

## Code Quality

### ESLint Configuration

1. **TypeScript Rules**
   - No unused vars (with exceptions)
   - Strict type checking
   - No explicit any

2. **React Rules**
   - Hooks rules enforced
   - No unescaped entities
   - Prop types disabled (using TS)

3. **Best Practices**
   - Import order
   - Code style
   - Accessibility

## Accessibility

### WCAG AA Compliance

1. **Keyboard Navigation**
   - All interactive elements focusable
   - Logical tab order
   - Focus indicators

2. **Screen Readers**
   - ARIA labels
   - Semantic HTML
   - Alt text for images

3. **Color Contrast**
   - WCAG AA ratios
   - Not relying on color alone
   - High contrast mode support

## Mobile Responsiveness

### Breakpoints

- xs: 0px
- sm: 600px
- md: 900px
- lg: 1200px
- xl: 1536px

### Mobile Optimizations

1. **Navigation**
   - Hamburger menu on mobile
   - Drawer navigation
   - Touch-friendly targets

2. **Calendar**
   - Day view on mobile
   - Week view on desktop
   - Touch interactions

3. **Tables**
   - Horizontal scroll
   - Compact layout
   - Priority columns

## Error Handling

### Error Boundaries

1. **Component Level**
   - Catch render errors
   - Show fallback UI
   - Log to error service

2. **API Level**
   - Axios interceptors
   - Toast notifications
   - Retry logic

### Validation

1. **Client-side**
   - Form validation
   - Type checking
   - User feedback

2. **Server-side** (future)
   - Backend validation
   - Error messages
   - Conflict resolution

