# Meeting Room Booking App

A modern, full-featured meeting room booking application built with React, TypeScript, Redux Toolkit, Material-UI, Tailwind CSS, and FullCalendar.

## Features

- **Room Overview**: Browse all meeting rooms with real-time status (Available/Busy/Unavailable)
- **Room Management**: CRUD operations for managing meeting rooms
- **Calendar Booking**: Interactive calendar for viewing and creating bookings
- **Analytics Dashboard**: Comprehensive usage statistics and insights
  - Utilization rate
  - Peak hours analysis
  - Daily trends
  - Hourly heatmaps
  - Room leaderboard
- **Responsive Design**: Mobile-friendly interface
- **LocalStorage Persistence**: Data persists across sessions

## Architecture

The app follows clean architecture principles with clear separation of concerns:

```
src/
├── components/      # Presentational components (pure, no logic)
│   ├── analytics/  # Analytics-specific components
│   ├── calendar/   # Calendar components
│   ├── common/     # Reusable UI components
│   ├── forms/      # Form components
│   ├── layout/     # Layout components
│   ├── modals/     # Modal dialogs
│   └── rooms/      # Room-specific components
├── hooks/          # Custom React hooks (business logic)
├── pages/          # Page components (route containers)
├── store/          # Redux store, slices, and selectors
├── types/          # TypeScript type definitions
├── utils/          # Pure utility functions
├── services/       # API service layer (ready for backend)
└── theme/          # MUI theme configuration
```

## Tech Stack

- **React 18** - UI framework
- **TypeScript** - Type safety
- **Redux Toolkit** - State management
- **React Router 6** - Routing
- **Material-UI (MUI)** - UI components
- **Tailwind CSS** - Utility-first styling
- **FullCalendar** - Calendar functionality
- **date-fns** - Date manipulation
- **Axios** - HTTP client (ready for API integration)
- **Vite** - Build tool
- **ESLint** - Code linting

## Getting Started

### Prerequisites

- Node.js 16+ and npm

### Installation

1. Clone the repository
2. Install dependencies:

```bash
npm install
```

3. Start the development server:

```bash
npm run dev
```

4. Open [http://localhost:5173](http://localhost:5173) in your browser

### Build for Production

```bash
npm run build
```

### Lint Code

```bash
npm run lint
```

## Usage

### Rooms Page

- View all rooms with current status
- Filter by status (All/Available/Busy/Unavailable)
- Search by room name or location
- Click any room card to view details

### Room Detail Page

- View room calendar with all bookings
- Click empty time slots to create new bookings
- Click existing bookings to edit or delete
- Inactive rooms cannot be booked

### Room Management Page

- Add new rooms
- Edit existing rooms
- Delete rooms
- Set room capacity, location, work hours, and amenities

### Analytics Page

- View utilization metrics
- Filter by date range (Today/Last 7 Days/Last 30 Days)
- Filter by specific rooms
- Adjust work hours for calculations
- View hourly heatmaps and daily trends
- See room leaderboard by usage

## Data Persistence

The app uses localStorage to persist:
- Meeting rooms
- Bookings

On first load, mock data is automatically generated with:
- 10 sample rooms
- 30+ bookings across different dates

## Utilization Calculation

Utilization rate is calculated as:

```
Utilization = (Total booked minutes clipped to work hours) / (Total available minutes)
```

Where:
- Booked minutes are clipped to work hours (default 08:00-20:00)
- Available minutes = Number of days × Active rooms × Work minutes per day

## API Integration

The app is ready for backend integration. The API service layer (`src/services/api.ts`) provides functions for:

- Fetching rooms and bookings
- Creating/updating/deleting rooms
- Creating/updating/deleting bookings

To integrate with a backend:

1. Set `VITE_API_BASE_URL` in `.env`
2. Implement async thunks in Redux slices
3. Replace direct Redux actions with API calls



## License

MIT

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

