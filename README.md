# Google Docs Clone

A collaborative document editor built with Next.js, featuring real-time collaboration, authentication, and organizational support. This application provides a Google Docs-like experience with rich text editing, live cursors, comments, and document management.

## Features

- **Real-time Collaboration**: Multiple users can edit documents simultaneously with live cursors and instant updates
- **Rich Text Editor**: Powered by TipTap with support for headings, formatting, tables, images, and more
- **Authentication**: Secure user authentication with Clerk, supporting organizations and personal accounts
- **Document Management**: Create, edit, delete, and organize documents with search functionality
- **Organizational Support**: Share documents within organizations and manage team collaboration
- **Responsive Design**: Modern UI built with Tailwind CSS and shadcn/ui components
- **Template Gallery**: Pre-built document templates for common use cases

## Tech Stack

- **Frontend**: Next.js 15, React 19, TypeScript
- **Backend**: Convex for real-time database and API
- **Authentication**: Clerk with organization support
- **Real-time Collaboration**: Liveblocks for collaborative editing
- **Editor**: TipTap with collaborative extensions
- **Styling**: Tailwind CSS, shadcn/ui components
- **State Management**: Zustand

## Prerequisites

Before setting up the project, ensure you have:

- A Convex account
- A Clerk account
- A Liveblocks account

## Setup Instructions

### 1. Clone and Install Dependencies

```bash
git clone <your-repository-url>
cd google-docs-clone
npm install
```

> **Note**: Use `npm install --legacy-peer-deps` flag if you encounter version conflicts during installation.

### 2. Environment Variables

Create a `.env.local` file in the root directory with the following variables:

```env
# Convex
NEXT_PUBLIC_CONVEX_URL=your-convex-deployment-url

# Clerk
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=your-clerk-publishable-key
CLERK_SECRET_KEY=your-clerk-secret-key

# Liveblocks
LIVEBLOCKS_SECRET_KEY=your-liveblocks-secret-key
```

### 3. Convex Setup

#### Install Convex CLI
```bash
npm install -g convex
```

#### Initialize Convex
```bash
npx convex dev
```

#### Configure Authentication
1. Go to your Convex dashboard
2. Navigate to Settings > Authentication
3. Add Clerk as an authentication provider
4. Use your Clerk domain: `https://your-clerk-domain.clerk.accounts.dev`
5. Set the application ID to: `convex`

#### Deploy Convex Functions
```bash
npx convex deploy
```

### 4. Clerk Setup

#### Create Clerk Application
1. Sign up at [clerk.com](https://clerk.com)
2. Create a new application
3. Enable organizations in your Clerk dashboard:
   - Go to Configure > Organizations
   - Enable organizations feature
   - Configure organization settings as needed

#### Configure JWT Template
1. In your Clerk dashboard, go to Configure > JWT Templates
2. Create a new template called `convex`
3. Add the following claims:
```json
{
  "aud": "convex",
  "name": "{{user.full_name}}",
  "email": "{{user.primary_email_address}}",
  "picture": "{{user.image_url}}",
  "nickname": "{{user.username}}",
  "given_name": "{{user.first_name}}",
  "updated_at": "{{user.updated_at}}",
  "family_name": "{{user.last_name}}",
  "phone_number": "{{user.primary_phone_number}}",
  "email_verified": "{{user.email_verified}}",
  "organization_id": "{{org.id}}",
  "phone_number_verified": "{{user.phone_number_verified}}"
}
```

#### Update Convex Auth Config
Update `convex/auth.config.ts` with your Clerk domain:
```typescript
export default {
  providers: [
    {
      domain: "https://your-clerk-domain.clerk.accounts.dev",
      applicationID: "convex"
    }
  ]
}
```

### 5. Liveblocks Setup

#### Create Liveblocks Project
1. Sign up at [liveblocks.io](https://liveblocks.io)
2. Create a new project
3. Get your public and secret keys from the dashboard

### 6. Run the Development Server

```bash
npm run dev
```

The application will be available at `http://localhost:3000`.

## Project Structure

```
src/
├── app/                   # Next.js app directory
│   ├── (home)/            # Home page and document listing
│   ├── api/               # API routes
│   ├── documents/         # Document editing interface
│   └── layout.tsx         # Root layout
├── components/            # Reusable components
│   ├── ui/                # shadcn/ui components
│   └── ...                # Custom components
├── hooks/                 # Custom React hooks
├── lib/                   # Utility functions
└── store/                 # Zustand stores

convex/
├── documents.ts          # Document CRUD operations
├── schema.ts             # Database schema
└── auth.config.ts        # Authentication configuration
```

## Key Configuration Files

- `convex/schema.ts`: Defines the database schema for documents
- `convex/auth.config.ts`: Configures Clerk authentication for Convex
- `liveblocks.config.ts`: TypeScript definitions for Liveblocks
- `src/middleware.ts`: Clerk middleware configuration

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request
