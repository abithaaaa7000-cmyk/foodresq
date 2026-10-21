# FoodResq - Food Waste Management Platform

## Overview
FoodResq is a comprehensive food waste management platform that connects food donors with receivers to reduce food waste and help communities. The platform features AI-powered food quality verification to ensure safety and quality.

## Authentication
The application uses **Replit Auth** with Google login (supports Google, GitHub, X, Apple, and email/password).

### Authentication Flow
1. Users click "Login with Google" on the home page
2. They're redirected to `/api/login` which initiates the OAuth flow
3. After successful authentication, users are redirected back to the app
4. First-time users are prompted to select their role (Donor/Receiver) and complete their profile
5. Returning users are redirected to their dashboard based on their role

### Key Files
- `server/replitAuth.ts` - Authentication setup and middleware
- `client/src/hooks/useAuth.ts` - React hook for authentication state
- `client/src/lib/authUtils.ts` - Utility functions for auth error handling
- `server/routes.ts` - Protected API routes

## Database Schema

### Users Table
- `id` (varchar, primary key) - Unique user ID from OAuth
- `email` (varchar) - User's email
- `firstName`, `lastName` (varchar) - User's name
- `profileImageUrl` (varchar) - Profile picture URL
- `userRole` (varchar) - 'donor' or 'receiver'
- `location` (text) - User's location
- `phone` (varchar) - Contact number
- `createdAt`, `updatedAt` (timestamp)

### Donations Table
- `id` (varchar, primary key)
- `donorId` (varchar) - References users.id
- `imageUrl` (text) - Food image URL
- `title` (text) - Donation title
- `foodType` (varchar) - 'veg' or 'non-veg'
- `category` (varchar) - Food category
- `description` (text)
- `expiryDate` (timestamp)
- `quantity` (text)
- `location` (text)
- `aiVerificationStatus` (varchar) - 'pending', 'approved', 'rejected'
- `status` (varchar) - 'available', 'requested', 'completed', 'expired'
- `createdAt` (timestamp)

### Reviews Table
- `id` (varchar, primary key)
- `donorId`, `receiverId` (varchar) - References users.id
- `donationId` (varchar) - References donations.id
- `rating` (varchar)
- `review` (text)
- `createdAt` (timestamp)

### Sessions Table
- Required for Replit Auth session management

## User Roles

### Donors
Can access:
- `/donor/donate` - Upload food images, fill details, get AI verification
- `/donor/my-donations` - View all donations (active/expired)
- `/donor/milestones` - Track achievements and impact

### Receivers
Can access:
- `/receiver/browse` - Browse available food by location
- `/receiver/requests` - View food requests and pickup details
- `/receiver/ratings` - Rate donors and write reviews

## Key Features

### AI Food Verification
- Donors upload food images before posting
- AI analyzes image quality and freshness
- Only approved food can be posted
- Status: pending → approved/rejected

### Expiry Management
- Expired donations are visually marked (red border)
- Expired items hidden from receiver view
- Automatic status updates based on expiry date/time

### Milestone System
- Bronze (5 donations) → Silver (20) → Gold (50) → Platinum (100+)
- Track total donations, families helped, food saved

### Rating & Reviews
- Receivers can rate donors after receiving food
- 5-star rating system with written reviews
- Builds trust in the community

## Tech Stack
- **Frontend**: React, Tailwind CSS, Shadcn UI, Wouter (routing)
- **Backend**: Express.js, Node.js
- **Database**: PostgreSQL with Drizzle ORM
- **Authentication**: Replit Auth (OAuth with Google)
- **AI Integration**: Ready for OpenAI/Anthropic vision API integration

## Environment Variables
- `DATABASE_URL` - PostgreSQL connection string
- `SESSION_SECRET` - Session encryption key
- `REPLIT_DOMAINS` - Allowed domains for OAuth
- `REPL_ID` - Replit project ID

## Next Steps for Full Implementation
1. Integrate AI vision API (OpenAI/Anthropic) for food verification
2. Implement image upload to cloud storage
3. Add real-time notifications for receivers
4. Implement geolocation-based food discovery
5. Add donation request/acceptance flow
6. Create admin dashboard for moderation
7. Add analytics and impact tracking

## Development
- Run `npm run dev` to start the development server
- Run `npm run db:push` to sync database schema
- Access the app at the Replit webview URL

## Design Guidelines
See `design_guidelines.md` for comprehensive UI/UX design principles, color palette, typography, and component usage guidelines.
