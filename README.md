# GoGreenMatrix - Car Rental & Booking Platform Backend API

GoGreenMatrix is the backend service behind a car rental and vehicle-sharing platform. It provides the APIs and business logic needed to manage vehicle listings, bookings, payments, real-time communication, and day-to-day platform operations for renters, vehicle owners, and administrators.

---

## Overview

Built with Node.js, Express, TypeScript, and MongoDB, the backend is designed to support the complete rental workflow—from listing a vehicle and making a booking to processing payments, managing payouts, and keeping users updated in real time.

The platform also includes role-based access control, automated background jobs, Stripe payment integration, Socket.IO for real-time messaging, and multi-channel notifications through Firebase Cloud Messaging (FCM), SMS, and email.

## Design Reference

The backend APIs in this project were developed based on the approved UI/UX design.

| Resource | Link |
|----------|------|
| Figma Design | [Click Here](https://www.figma.com/design/6j99QbPuFEghJtS83f01VL/Rimaiziza?node-id=3752-22446&p=f&t=P1ZnaQVaecaExbYm-0) |

> The UI/UX design was provided by the project team and is referenced here to help understand the API flow and business requirements.

---

### Core Features

- Manage users with dedicated roles for renters, vehicle owners, and administrators.
- Create, manage, and track vehicle listings and rental bookings throughout their lifecycle.
- Process secure payments, handle webhook events, calculate platform fees, and manage host payouts.
- Enable real-time messaging between renters and vehicle owners using Socket.IO.
- Send important updates through push notifications, SMS, and email.
- Automate scheduled tasks and recurring background processes using cron jobs.
- Provide administrators with the tools needed to monitor, manage, and maintain the platform.

---


## Features

- **Authentication & OAuth**: Email/password authentication, Google & Facebook OAuth identity verification, OTP phone verification via Twilio/AfrikSMS, and JWT access/refresh token management.
- **Role-Based Access Control (RBAC)**: Enforced authorization across `USER`, `HOST`, `ADMIN`, and `SUPER_ADMIN` roles.
- **Car & Fleet Management**: Vehicle cataloging, feature management, custom pricing, rules, media attachments, host approvals, user favorites, and reviews.
- **Booking Engine & Automation**: Full booking lifecycle handling (creation, cancellation, approval, completion) with background cron jobs for status updates.
- **Payment & Financial Processing**: Integrated Stripe Checkout, Stripe Webhooks, CinetPay payment support, platform fee management, and transaction history.
- **Host Payout Management**: Host earnings tracking and payout request processing workflows.
- **Real-Time Communication**: Socket.IO-based chat channels and direct messaging between renters and vehicle hosts.
- **Multi-Channel Notifications**: Firebase Cloud Messaging (FCM) push notifications, SMS alerts (Twilio and AfrikSMS), and SMTP email notifications via Nodemailer.
- **Content & Support Management**: Support ticket handling, promotional banner management, platform FAQs, and policy/rule management.
- **Analytics & Admin Reporting**: Platform usage analytics, booking statistics, and revenue tracking endpoints for system administrators.

---

## Tech Stack

| Category | Technology |
| :--- | :--- |
| **Runtime Environment** | Node.js |
| **Language** | TypeScript |
| **Framework** | Express.js |
| **Database** | MongoDB |
| **ODM** | Mongoose |
| **Authentication** | JWT (`jsonwebtoken`), Passport.js (Google & Facebook OAuth) |
| **Validation** | Zod |
| **Real-Time Communication** | Socket.IO |
| **Payment Gateways** | Stripe (API & Webhooks), CinetPay |
| **Storage & Image Processing**| Multer, Express Static (`/uploads`), Sharp |
| **Push Notifications** | Firebase Admin SDK (FCM) |
| **SMS Providers** | Twilio, AfrikSMS |
| **Email Provider** | Nodemailer (SMTP / Gmail) |
| **Logging System** | Winston, Winston Daily Rotate File, Morgan |
| **Background Jobs** | Node Cron (`node-cron`) |
| **Code Formatter** | Prettier |

---

## Project Structure

```
Remaiziza-STA-Backend/
├── docs/                      # Project documentation and ER diagrams
│   └── erd/                   # Database Entity Relationship Diagrams
├── src/                       # Application source code
│   ├── app/                   # Main application modules and configuration
│   │   ├── builder/           # Dynamic query builder helpers
│   │   ├── middlewares/       # Authentication guards, file uploaders, error handlers
│   │   ├── modules/           # Business domain modules
│   │   └── routes/            # Main API router definitions (v1 & v2)
│   ├── config/                # Environment variable management
│   ├── constants/             # Global application constants
│   ├── DB/                    # Database initializers and Super Admin seeder
│   ├── enums/                 # System enumerations (Roles, Statuses)
│   ├── errors/                # Custom error classes (AppError, ApiError)
│   ├── handlers/              # File and event handlers
│   ├── helpers/               # Core utility helpers (JWT, Socket, Email, Notifications)
│   ├── shared/                # Shared utilities, Winston loggers, and HTTP helpers
│   ├── types/                 # Custom TypeScript definitions
│   ├── util/                  # Helper utilities
│   ├── app.ts                 # Express application setup and global middlewares
│   └── server.ts              # HTTP server entry point and socket server setup
├── views/                     # EJS template views for email rendering
├── .env.example               # Environment variables configuration template
├── package.json               # Project manifest, dependencies, and scripts
└── tsconfig.json              # TypeScript configuration
```

### Directory Explanations

- `src/app/modules/`: Contains domain-driven business modules (Controllers, Services, Models, Routes, Interfaces, Validations).
- `src/app/middlewares/`: Houses authentication guards, rate limiters, multi-part form handlers, and error handlers.
- `src/helpers/`: Utility services for socket connections, JWT operations, email dispatches, and notification generation.
- `src/shared/`: Centralized loggers (Winston & Morgan), standard response wrappers, and database helpers.
- `docs/erd/`: Auto-generated database ER diagrams in Draw.io, PNG, SVG, and PDF formats.

---

## Installation

### Prerequisites

- **Node.js**: v18 or higher
- **MongoDB**: Local instance or MongoDB Atlas URI
- **npm**: v9 or higher

### Step-by-Step Setup

1. **Clone the Repository**:
   ```bash
   git clone <repository-url>
   cd Remaiziza-STA-Backend
   ```

2. **Install Dependencies**:
   ```bash
   npm install
   ```

3. **Configure Environment Variables**:
   ```bash
   cp .env.example .env
   ```
   *Edit `.env` and fill in your database URIs, API keys, and secret keys.*

4. **Start MongoDB**:
   Ensure MongoDB service is running locally or verify your remote MongoDB Atlas connection string in `.env`.

5. **Start the Development Server**:
   ```bash
   npm run dev
   ```

---

## Environment Variables

| Variable | Required | Description |
| :--- | :---: | :--- |
| `IP` | Yes | Host IP address binding for the Express HTTP server |
| `PORT` | Yes | Port number on which the HTTP server listens |
| `NODE_ENV` | Yes | Application execution environment (`development` or `production`) |
| `RESPONSE_MODE` | No | API response formatting behavior (`SOFT` or `STRICT`) |
| `DATABASE_URL` | Yes | MongoDB connection URI string |
| `BCRYPT_SALT_ROUNDS` | Yes | Salt rounds used for Bcrypt password hashing |
| `JWT_SECRET` | Yes | Secret key used for signing JWT access tokens |
| `jwtRefreshSecret` | Yes | Secret key used for signing JWT refresh tokens |
| `JWT_EXPIRE_IN` | Yes | Expiration duration for access tokens (e.g., `30d`) |
| `JWT_REFRESH_EXPIRES_IN` | Yes | Expiration duration for refresh tokens (e.g., `30d`) |
| `AUTH_API_KEY` | No | UNKNOWN (Base64 key used for external auth proxy evaluation) |
| `CLIENT_URL` | Yes | Base URL of the frontend web application |
| `BASE_URL` | Yes | Base URL of the backend API server |
| `CURRENCY` | Yes | Default transaction currency code (e.g., `MYR`) |
| `REDIS_HOST` | No | Redis server hostname or connection URL |
| `REDIS_PORT` | No | Redis server port number |
| `REDIS_PASSWORD` | No | Redis authentication password |
| `REDIS_DB` | No | Redis database index number |
| `START_CRON` | Yes | Toggle flag to enable background cron workers (`true`/`false`) |
| `STRIPE_SECRET_KEY` | Yes | Stripe secret API key for payment processing |
| `STRIPE_WEBHOOK_SECRET` | Yes | Stripe webhook endpoint signing secret |
| `CINATPAY_SITE_ID` | No | CinetPay merchant site identifier |
| `CINATPAY_API_KEY` | No | CinetPay API key |
| `CINATPAY_SECRET_KEY` | No | CinetPay webhook signing secret |
| `FIREBASE_CLIENT_EMAIL` | No | Firebase Admin SDK service account email |
| `FIREBASE_PROJECT_ID` | No | Firebase project ID for push notifications |
| `FIREBASE_PRIVATE_KEY` | No | Firebase Admin SDK private key string |
| `GOOGLE_CLIENT_ID` | No | Google OAuth 2.0 Client ID |
| `GOOGLE_CLIENT_SECRET` | No | Google OAuth 2.0 Client Secret |
| `TWILIO_ACCOUNT_SID` | No | Twilio account SID for SMS verification |
| `TWILIO_AUTH_TOKEN` | No | Twilio authentication token |
| `TWILIO_SERVICE_SID` | No | Twilio Verification service SID |
| `EMAIL_HOST` | Yes | SMTP server host address |
| `EMAIL_PORT` | Yes | SMTP server port number |
| `EMAIL_USER` | Yes | SMTP authentication username |
| `EMAIL_PASS` | Yes | SMTP authentication password |
| `EMAIL_FROM` | Yes | Default sender email address |
| `SUPPORT_RECEIVER_EMAIL` | Yes | Destination email address for support ticket alerts |
| `ADMIN_EMAIL` | Yes | Initial Super Admin user email address for DB seeding |
| `ADMIN_PASSWORD` | Yes | Initial Super Admin user password for DB seeding |

---

## Available Scripts

| Script | Command | Description |
| :--- | :--- | :--- |
| `npm run dev` | `ts-node-dev --respawn --transpile-only src/server.ts` | Runs the server in development mode with hot reloading. |
| `npm run start` | `node dist/server.js` | Runs the compiled JavaScript application in production. |
| `npm run build` | `tsc` | Compiles TypeScript source files into JavaScript inside `dist/`. |
| `npm run format` | `prettier . --write` | Formats source files using Prettier according to
---


## Database

- **Database**: MongoDB
- **ODM**: Mongoose
- **Migration Strategy**: Schema evolution and field updates managed directly through Mongoose models.
- **Seed Scripts**: Automatic Super Admin account creation (`seedSuperAdmin()`) executed during application startup.
- **Connection Approach**: Singleton connection established in `src/server.ts` using `mongoose.connect()`.

---

## Authentication & Authorization

### Authentication

- **JWT Tokens**: Secure authentication utilizing JWT access tokens (`JWT_SECRET`) and refresh tokens (`jwtRefreshSecret`).
- **OAuth Providers**: Social identity verification supported via Passport.js strategies (`passport-google-oauth20`).
- **Phone Verification**: OTP generation and SMS dispatches using Twilio and AfrikSMS gateways.
- **Password Reset**: OTP-backed password reset flow managed via token verification modules.

### Authorization (RBAC)

Access control is managed through user roles defined in `USER_ROLES` (`src/enums/user.ts`):
- `SUPER_ADMIN`: Full administrative authorization over platform entities, system roles, and financial transactions.
- `ADMIN`: System-wide monitoring, user/host approvals, content moderation, and support processing.
- `HOST`: Vehicle creation, rental pricing management, booking request approvals, and payout requests.
- `USER`: Vehicle browsing, rental reservations, favorites management, reviews, and host messaging.

Protected endpoints enforce authorization via the `auth(...roles)` middleware (`src/app/middlewares/auth.ts`).

---

## Modules

The application is structured into 22 distinct business modules:

- **Analytics**: Admin reporting, booking metrics, and platform revenue stats.
- **Auth**: Login, registration, token refresh, password resets, and OAuth processing.
- **Banner**: Promotional hero banner management.
- **Booking**: Vehicle reservation processing, scheduling, status management, and automated cron workers.
- **Car**: Vehicle listings, details, media uploads, pricing, availability, and filtering.
- **Charges**: Platform service fees and charge configuration.
- **Chat**: Real-time communication channels between users and hosts.
- **FCMToken**: Management of Firebase Cloud Messaging tokens for push notifications.
- **FavoriteCar**: User saved/favorite vehicle management.
- **FAQ**: Platform frequently asked questions management.
- **Media**: Image processing, media storage, and static asset handling.
- **Message**: Individual chat message dispatching and history.
- **Notification**: User in-app notifications and push alert dispatches.
- **Payout**: Host earnings management and payout request processing.
- **ResetToken**: Password reset verification tokens.
- **Review**: Vehicle and host rating and review management.
- **Rule**: Platform policy and terms documentation.
- **Stripe**: Stripe Checkout sessions, webhooks, and payment integration.
- **Support**: Customer support ticket creation and email dispatches.
- **Transaction**: Payment history and transaction logs.
- **TwilioService**: Phone OTP verification services.
- **User**: User profile management, host verification workflows, and admin user operations.

---

## Running the Project

### Development Mode

```bash
npm run dev
```

### Production Build

```bash
# Build the TypeScript project
npm run build

# Start the production server
npm start
```

### Code Formatting

```bash
npm run format
```

---

## Logging

- **Application & Error Logging**: Implemented using **Winston** with daily log file rotation (`winston-daily-rotate-file`) in `src/shared/logger.ts`. Log outputs are structured separately into `logs/winston/successes/` and `logs/winston/errors/`.
- **HTTP Request Logging**: Configured using **Morgan** middleware (`src/shared/morgan.ts`) to record incoming API requests and HTTP status responses.

---

## Error Handling

- **Global Error Middleware**: Centralized Express error handler (`src/app/middlewares/globalErrorHandler.ts`) catches asynchronous and synchronous errors, normalizing output into standardized error response structures.
- **Custom Error Classes**: Implements `AppError` and `ApiError` extending the native `Error` object for controlled status code propagation.
- **Process Protection**: Process-level listeners catch `uncaughtException` and `unhandledRejection` events in `src/server.ts` to log errors through Winston before managing clean server shutdowns.

---

## Security

- **CORS Protection**: Origin-restricted Cross-Origin Resource Sharing configured in `src/app.ts`.
- **Rate Limiting**: Request rate limiting enforced via `express-rate-limit` middleware (`src/app/middlewares/rateLimiter.ts`).
- **Password Hashing**: Secure password encryption utilizing `bcrypt` salt rounds.
- **Input Validation**: Request payload sanitization and schema validation powered by `zod` (`src/app/middlewares/validateRequest.ts`).
- **Role-Based Guards**: Strict route protection checking authenticated JWT tokens and verified user roles.
- **Webhook Security**: Raw body parsing and signature validation on Stripe webhooks (`src/app/middlewares/verifyWebhook.ts`).

---



## Entity Relationship Diagram (ERD)

The project includes automatically generated Entity Relationship Diagrams (ERDs) mapping the Mongoose schemas and relationships across modules.

#### System-Wide ER Diagram
Below is the rendered project-wide database structure:

![System-Wide ER Diagram](./docs/erd/modules/whole-er-diagram/er-diagram.png)

#### Module-Specific Diagrams
For a focused view of each module, check the following directories:
* **Banner:** [PNG](./docs/erd/modules/banner/er-diagram.png)
* **Booking:** [PNG](./docs/erd/modules/booking/er-diagram.png)
* **Car:** [PNG](./docs/erd/modules/car/er-diagram.png)
* **Charges:** [PNG](./docs/erd/modules/charges/er-diagram.png)
* **Chat:** [PNG](./docs/erd/modules/chat/er-diagram.png)
* **FAQ:** [PNG](./docs/erd/modules/faq/er-diagram.png)
* **Favorite Car:** [PNG](./docs/erd/modules/favoriteCar/er-diagram.png)
* **FCM Token:** [PNG](./docs/erd/modules/fcmToken/er-diagram.png)
* **Media:** [PNG](./docs/erd/modules/media/er-diagram.png)
* **Message:** [PNG](./docs/erd/modules/message/er-diagram.png)
* **Notification:** [PNG](./docs/erd/modules/notification/er-diagram.png)
* **Payout:** [PNG](./docs/erd/modules/payout/er-diagram.png)
* **Reset Token:** [PNG](./docs/erd/modules/resetToken/er-diagram.png)
* **Review:** [PNG](./docs/erd/modules/review/er-diagram.png)
* **Rule:** [PNG](./docs/erd/modules/rule/er-diagram.png)
* **Support:** [PNG](./docs/erd/modules/support/er-diagram.png)
* **Transaction:** [PNG](./docs/erd/modules/transaction/er-diagram.png)
* **User:** [PNG](./docs/erd/modules/user/er-diagram.png)

---


## License

ISC

---

## Maintainer

Moshfiqur Rahman - moshfiqurrahman37@gmail.com
