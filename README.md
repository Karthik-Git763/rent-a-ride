# 🚗 Rent-a-Ride

A modern, full-stack car rental platform built with React, Hono, and Cloudflare Workers. This application provides a seamless experience for users to browse, rent, and manage vehicles with real-time availability and location tracking.

## ✨ Features

- **🔐 Authentication**: Secure user authentication powered by Better Auth with email verification
- **🚙 Vehicle Management**: Browse available vehicles with detailed information, pricing, and availability
- **📍 Location Tracking**: Interactive maps using Leaflet for vehicle location and GPS tracking
- **💰 Pricing System**: Dynamic pricing with price-per-day calculations
- **📊 Dashboard**: Owner dashboard for managing vehicles and tracking earnings
- **📧 Email Notifications**: Automated email notifications using React Email and Resend
- **🎨 Modern UI**: Beautiful, responsive interface built with Tailwind CSS and Radix UI components
- **⚡ Edge Computing**: Deployed on Cloudflare Workers for global low-latency access
- **💾 Database**: Powered by Cloudflare D1 (SQLite) with Drizzle ORM

## 🛠️ Tech Stack

### Frontend
- **React 19** - UI framework
- **React Router 7** - Client-side routing
- **Tailwind CSS 4** - Utility-first styling
- **Radix UI** - Accessible component primitives
- **Lucide React** - Icon library
- **Motion** - Animation library
- **Recharts** - Data visualization
- **Leaflet** - Interactive maps

### Backend
- **Hono** - Lightweight web framework
- **Cloudflare Workers** - Serverless compute platform
- **Cloudflare D1** - SQLite database at the edge
- **Better Auth** - Authentication system
- **Drizzle ORM** - TypeScript ORM

### Development
- **Vite 6** - Build tool and dev server
- **TypeScript 5.8** - Type safety
- **ESLint** - Code linting
- **Wrangler** - Cloudflare Workers CLI

## 📋 Prerequisites

Before you begin, ensure you have the following installed:
- **Node.js** (v18 or higher)
- **pnpm** (recommended) or npm
- **Wrangler CLI** - Cloudflare Workers CLI tool
- **Cloudflare Account** - For deployment

## 🚀 Installation

### 1. Clone the repository
```bash
git clone https://github.com/Karthik-Git763/rent-a-ride.git
cd rent-a-ride
```

### 2. Install dependencies
```bash
pnpm install
# or
npm install
```

### 3. Set up environment variables
Create a `.env` file in the root directory:
```env
# Database
DATABASE_URL=your_database_url

# Better Auth
BETTER_AUTH_SECRET=your_secret_key
BETTER_AUTH_URL=http://localhost:5173

# Email (Resend)
RESEND_API_KEY=your_resend_api_key

# Cloudflare
CLOUDFLARE_ACCOUNT_ID=your_account_id
CLOUDFLARE_DATABASE_ID=your_database_id
```

### 4. Set up the database

#### Create Cloudflare D1 Database
```bash
# Create a new D1 database
pnpm wrangler d1 create rent-a-ride-db

# Update wrangler.toml with the database ID
```

#### Generate Database Schema
```bash
# Generate Better Auth schema
pnpx @better-auth/cli generate --config=./src/auth/index.ts

# Push schema to database
pnpm drizzle-kit push
```

#### Run Migrations
```bash
# For local development
pnpm wrangler d1 migrations apply db --local

# For production
pnpm wrangler d1 migrations apply db --remote
```

> **Note**: If you encounter an error about existing items, remove all items from `.wrangler/state/v3/d1` and run the migration again.

### 5. Configure Wrangler
Update the `wrangler.jsonc` file with your Cloudflare account details:
```jsonc
{
  "name": "rent-a-ride",
  "compatibility_date": "2024-01-01",
  "d1_databases": [
    {
      "binding": "DB",
      "database_name": "rent-a-ride-db",
      "database_id": "your-database-id"
    }
  ]
}
```

## 💻 Development

### Start the development server
```bash
pnpm dev
```

This will start the Vite development server at `http://localhost:5173`

### Type checking
```bash
pnpm check
```

### Linting
```bash
pnpm lint
```

## 🏗️ Building

### Build for production
```bash
pnpm build
```

This command:
1. Compiles TypeScript
2. Builds the React application
3. Prepares the Cloudflare Worker

### Preview production build
```bash
pnpm preview
```

## 🚀 Deployment

### Deploy to Cloudflare Workers
```bash
pnpm deploy
```

### Dry run (test before deploying)
```bash
pnpm check
```

### Important: Before Deployment
- Update `BaseURL.ts` with your production URL
- Ensure all environment variables are set in Cloudflare Workers settings
- Verify database migrations are applied to production

## 📁 Project Structure

```
rent-a-ride/
├── src/
│   ├── react-app/          # React frontend application
│   │   ├── components/     # Reusable UI components
│   │   ├── pages/          # Route pages
│   │   ├── hooks/          # Custom React hooks
│   │   └── lib/            # Utility functions
│   ├── worker/             # Cloudflare Worker (Hono backend)
│   │   ├── routes/         # API routes
│   │   └── index.ts        # Worker entry point
│   ├── db/                 # Database schema and migrations
│   │   ├── schema.ts       # Drizzle schema definitions
│   │   └── index.ts        # Database client
│   └── lib/                # Shared utilities
├── drizzle/                # Database migrations
├── public/                 # Static assets
├── wrangler.jsonc          # Cloudflare Workers configuration
├── vite.config.ts          # Vite configuration
├── drizzle.config.ts       # Drizzle ORM configuration
└── package.json            # Project dependencies
```

## 🔧 Configuration Files

- **`vite.config.ts`** - Vite build configuration
- **`wrangler.jsonc`** - Cloudflare Workers configuration
- **`drizzle.config.ts`** - Database ORM configuration
- **`tsconfig.json`** - TypeScript configuration
- **`components.json`** - Shadcn UI components configuration

## 📝 Available Scripts

| Script | Description |
|--------|-------------|
| `pnpm dev` | Start development server |
| `pnpm build` | Build for production |
| `pnpm preview` | Preview production build locally |
| `pnpm deploy` | Deploy to Cloudflare Workers |
| `pnpm lint` | Run ESLint |
| `pnpm check` | Type check and dry-run deployment |
| `pnpm cf-typegen` | Generate Cloudflare Worker types |

## 🗃️ Database Management

### View local database
```bash
pnpm wrangler d1 execute db --local --command="SELECT * FROM table_name"
```

### View production database
```bash
pnpm wrangler d1 execute db --remote --command="SELECT * FROM table_name"
```

### Create new migration
```bash
pnpm drizzle-kit generate
```

## 🔐 Authentication

The application uses Better Auth with the following features:
- Email/Password authentication
- Email verification (optional - currently disabled for development)
- Session management
- Cloudflare Workers compatibility

**Note**: Email verification is currently disabled in `auth.ts`. Enable it in production by setting `emailVerification: true`.

## 🎨 UI Components

This project uses:
- **Shadcn** - UI Components
- **Radix UI** - Accessible, unstyled components
- **Tailwind CSS** - Utility-first CSS framework
- **Lucide React** - Beautiful icon library
- **Custom components** - Built with class-variance-authority for variants

## 🌍 Maps & Location

- **Leaflet** - Interactive maps
- **React Leaflet** - React wrapper for Leaflet
- GPS tracking for vehicle locations
- Real-time location updates

## 📧 Email System

Email functionality powered by:
- **React Email** - Build emails with React components
- **Resend** - Email delivery service
- Automated notifications for bookings and verifications

## 🐛 Troubleshooting

### Database migration errors
If you encounter migration errors:
```bash
# Remove local database state
rm -rf .wrangler/state/v3/d1
# Re-run migrations
pnpm wrangler d1 migrations apply db --local
```

### Port already in use
```bash
# Kill process using port 5173
lsof -ti:5173 | xargs kill -9
```

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is private and not licensed for public use.

## 👨‍💻 Author

**Karthik**
- GitHub: [@Karthik-Git763](https://github.com/Karthik-Git763)

## 🙏 Acknowledgments

- [Cloudflare Workers](https://workers.cloudflare.com/)
- [Hono](https://hono.dev/)
- [Drizzle ORM](https://orm.drizzle.team/)
- [Better Auth](https://better-auth.com/)
- [Vite](https://vitejs.dev/)
- [Tailwind CSS](https://tailwindcss.com/)

---

Built with ❤️ using modern web technologies
