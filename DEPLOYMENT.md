# Deployment Guide

This guide will help you deploy the Next.js AI Chatbot to Vercel.

## Prerequisites

Before deploying, ensure you have:

- A [Vercel account](https://vercel.com/signup)
- A [GitHub](https://github.com), [GitLab](https://gitlab.com), or [Bitbucket](https://bitbucket.org) account
- API keys from AI model providers:
  - [xAI](https://console.x.ai/) - For chat and image models
  - [Groq](https://console.groq.com/keys) - For reasoning models

## Quick Deploy (Recommended)

The fastest way to deploy is using the one-click deploy button:

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fvercel%2Fai-chatbot&env=AUTH_SECRET&envDescription=Generate%20a%20random%20secret%20to%20use%20for%20authentication&envLink=https%3A%2F%2Fgenerate-secret.vercel.app%2F32&project-name=my-awesome-chatbot&repository-name=my-awesome-chatbot&demo-title=AI%20Chatbot&demo-description=An%20Open-Source%20AI%20Chatbot%20Template%20Built%20With%20Next.js%20and%20the%20AI%20SDK%20by%20Vercel&demo-url=https%3A%2F%2Fchat.vercel.ai&products=%5B%7B%22type%22%3A%22integration%22%2C%22protocol%22%3A%22ai%22%2C%22productSlug%22%3A%22grok%22%2C%22integrationSlug%22%3A%22xai%22%7D%2C%7B%22type%22%3A%22integration%22%2C%22protocol%22%3A%22ai%22%2C%22productSlug%22%3A%22api-key%22%2C%22integrationSlug%22%3A%22groq%22%7D%2C%7B%22type%22%3A%22integration%22%2C%22protocol%22%3A%22storage%22%2C%22productSlug%22%3A%22neon%22%2C%22integrationSlug%22%3A%22neon%22%7D%2C%7B%22type%22%3A%22blob%22%7D%5D)

This will:
1. Clone the repository to your account
2. Create a new Vercel project
3. Set up integrations for Postgres (via Neon) and Blob storage
4. Deploy your application

## Manual Deployment

### Step 1: Set Up Database and Storage

1. **Postgres Database**: Create a Postgres database using [Vercel Postgres](https://vercel.com/docs/storage/vercel-postgres/quickstart) or [Neon](https://neon.tech/)
2. **Blob Storage**: Set up [Vercel Blob](https://vercel.com/docs/storage/vercel-blob) for file storage

### Step 2: Configure Environment Variables

Set up the following environment variables in your Vercel project:

```bash
# Authentication Secret
# Generate one at: https://generate-secret.vercel.app/32
AUTH_SECRET=your-secret-here

# AI Model Provider API Keys
XAI_API_KEY=your-xai-api-key
GROQ_API_KEY=your-groq-api-key

# Storage (automatically created by Vercel integrations)
BLOB_READ_WRITE_TOKEN=your-blob-token
POSTGRES_URL=your-postgres-url
```

### Step 3: Deploy Using Vercel CLI

```bash
# Install Vercel CLI
npm i -g vercel

# Link your project
vercel link

# Deploy
vercel --prod
```

### Step 4: Deploy Using GitHub Integration

1. Push your code to GitHub
2. Go to [Vercel Dashboard](https://vercel.com/dashboard)
3. Click "Add New Project"
4. Import your repository
5. Configure environment variables
6. Click "Deploy"

## Configuration Files

### vercel.json

The `vercel.json` file contains deployment configuration:

- **buildCommand**: Specifies the build command (`pnpm run build`)
- **framework**: Identifies the project as Next.js
- **headers**: Security headers applied to all routes
  - `X-Content-Type-Options`: Prevents MIME type sniffing
  - `X-Frame-Options`: Prevents clickjacking
  - `X-XSS-Protection`: Enables browser XSS protection
  - `Referrer-Policy`: Controls referrer information

### .vercelignore

The `.vercelignore` file specifies which files should not be uploaded during deployment, similar to `.gitignore`.

## Post-Deployment

After deployment:

1. Visit your deployment URL
2. Sign in or create an account
3. Start chatting with the AI

## Troubleshooting

### Database Migration Errors

If you encounter database migration errors:

1. Ensure `POSTGRES_URL` is set correctly
2. Check that your database is accessible
3. Run migrations manually: `pnpm db:migrate`

### Build Failures

If the build fails:

1. Check all environment variables are set
2. Review the build logs in Vercel dashboard
3. Ensure dependencies are installed correctly

### Runtime Errors

If the app runs but has errors:

1. Check API keys are valid
2. Verify database connection
3. Check Vercel function logs

## Local Development with Deployed Resources

To develop locally while using deployed resources:

```bash
# Install Vercel CLI
npm i -g vercel

# Link to your Vercel project
vercel link

# Pull environment variables
vercel env pull

# Install dependencies
pnpm install

# Run development server
pnpm dev
```

Your app will run at `http://localhost:3000` with production environment variables.

## Environment-Specific Configuration

### Development
```bash
cp .env.example .env.local
# Edit .env.local with your development credentials
```

### Production
Configure environment variables in the Vercel dashboard under Project Settings → Environment Variables.

## Additional Resources

- [Vercel Documentation](https://vercel.com/docs)
- [Next.js Deployment Documentation](https://nextjs.org/docs/deployment)
- [AI SDK Documentation](https://sdk.vercel.ai/docs)
- [Vercel Postgres Documentation](https://vercel.com/docs/storage/vercel-postgres)
- [Vercel Blob Documentation](https://vercel.com/docs/storage/vercel-blob)
