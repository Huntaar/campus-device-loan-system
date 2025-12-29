# Frontend

Next.js web application for the Campus Device Loan System.

## Overview

The frontend provides a user interface for:
- **Students**: Browse devices, make reservations, join waitlists, and manage their loans
- **Staff**: Manage devices, inventory, reservations, loans, and user accounts

## Tech Stack

- **Framework**: Next.js 14 (App Router)
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **HTTP Client**: Axios
- **Authentication**: JWT tokens (stored in cookies)
- **Testing**: Jest + React Testing Library

## Prerequisites

- Node.js 18+
- npm or yarn
- Backend services running:
  - Device Service on port 7778
  - Loan Service on port 7779

## Setup

1. Install dependencies:
```bash
npm install
```

2. Set up environment variables:
   Create a `.env.local` file:
   ```env
   NEXT_PUBLIC_DEVICE_SERVICE_URL=http://localhost:7778
   NEXT_PUBLIC_LOAN_SERVICE_URL=http://localhost:7779
   ```

3. Run the development server:
```bash
npm run dev
```

4. Open [http://localhost:3000](http://localhost:3000) in your browser.

## Project Structure

```
frontend/
├── app/                    # Next.js app directory
│   ├── available-devices/ # Available devices page
│   ├── dashboard/         # User dashboard
│   ├── devices/           # Device catalog and details
│   ├── login/             # Login page
│   ├── notifications/     # Email notifications
│   ├── staff/             # Staff dashboard
│   │   ├── inventory/     # Inventory management
│   │   ├── reservations/  # Reservation management
│   │   ├── users/         # User management
│   │   └── waitlist/      # Waitlist management
│   ├── layout.tsx         # Root layout
│   └── page.tsx           # Home page
├── components/            # Reusable React components
│   ├── Navbar.tsx         # Navigation bar
│   └── ProtectedRoute.tsx  # Route protection wrapper
├── contexts/              # React contexts
│   └── AuthContext.tsx    # Authentication state management
├── lib/                   # Utilities and API clients
│   ├── api/              # API client services
│   │   ├── device-service.ts
│   │   └── loan-service.ts
│   ├── types/            # TypeScript type definitions
│   └── utils.ts          # Utility functions
└── __tests__/            # Test files
```

## Features

### Authentication
- Login with email and password
- JWT token management via HTTP-only cookies
- Protected routes with automatic redirects
- Role-based access control (student/staff)

### Student Features
- Browse device catalog with search
- View device details
- Reserve available devices
- Join waitlist for unavailable devices
- View and manage reservations
- View and manage waitlist entries
- View email notifications

### Staff Features
- View all users
- Manage device inventory
- View all reservations
- Mark reservations as collected (create loans)
- Mark loans as returned
- Manage waitlists
- View all loans

## Scripts

- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm start` - Start production server
- `npm run lint` - Run ESLint
- `npm test` - Run all tests
- `npm run test:watch` - Run tests in watch mode
- `npm run test:coverage` - Generate coverage report

## API Integration

The frontend integrates with two backend services:

- **Device Service** (port 7778): Handles devices, reservations, waitlists, users, and authentication
- **Loan Service** (port 7779): Handles loan lifecycle management

All API calls are made through client utilities in `lib/api/` that automatically handle:
- JWT token injection via Authorization headers
- Error handling and response formatting
- Cookie management for authentication

## Testing

The frontend includes comprehensive test coverage:

```bash
# Run all tests
npm test

# Run tests in watch mode
npm run test:watch

# Generate coverage report
npm run test:coverage
```

**Test Coverage**:
- Components (Navbar, ProtectedRoute)
- Contexts (AuthContext)
- API services (device-service, loan-service)
- Utility functions

## Production Build

The application is configured for DigitalOcean App Platform deployments:

**Build Configuration**:
- Build-time dependencies (Tailwind CSS, PostCSS, Autoprefixer) are in `dependencies`
- TypeScript path aliases configured with `baseUrl: "."`
- Tailwind config includes all component directories

## Related Documentation

- [Main Project README](../../README.md) - Project overview
- [API Reference](../backend/API.md) - Backend API documentation
- [Frontend README](../../frontend/README.md) - Detailed frontend documentation

