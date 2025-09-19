# Deployment Guide

This guide will help you deploy the Payload CMS public demo to GitHub, Vercel, and Supabase.

## 1. GitHub Setup

Your code is already connected to GitHub. To push any changes:

```bash
git add .
git commit -m "Configure for Vercel and Supabase deployment"
git push origin master
```

## 2. Supabase Setup

1. Go to [supabase.com](https://supabase.com) and create a new project
2. Once created, go to Settings > Database
3. Copy your connection string (it will look like: `postgresql://postgres:[YOUR-PASSWORD]@db.[YOUR-PROJECT-REF].supabase.co:5432/postgres`)
4. Save this for the Vercel environment variables

## 3. Vercel Deployment

1. Go to [vercel.com](https://vercel.com) and sign in with your GitHub account
2. Click "New Project" and import your GitHub repository
3. Configure the following environment variables in Vercel:

### Required Environment Variables:

```bash
# Database
DATABASE_URL=postgresql://postgres:[YOUR-PASSWORD]@db.[YOUR-PROJECT-REF].supabase.co:5432/postgres

# Payload Configuration
PAYLOAD_SECRET=your-secure-random-string-here
PAYLOAD_PUBLIC_SERVER_URL=https://your-app-name.vercel.app
PAYLOAD_PUBLIC_DRAFT_SECRET=your-draft-secret-here
REVALIDATION_KEY=your-revalidation-key-here
PAYLOAD_DEMO_RESET_KEY=your-demo-reset-key-here

# Next.js Configuration
NEXT_PUBLIC_SERVER_URL=https://your-app-name.vercel.app
NEXT_PUBLIC_IS_LIVE=true
NEXT_PRIVATE_DRAFT_SECRET=your-draft-secret-here
NEXT_PRIVATE_REVALIDATION_KEY=your-revalidation-key-here

# Optional
PORT=3000
```

### Generate Secure Secrets:

Use these commands to generate secure secrets:

```bash
# For PAYLOAD_SECRET (32 characters)
openssl rand -base64 32

# For other secrets (24 characters)
openssl rand -base64 24
```

## 4. Build Configuration

The project includes a `vercel.json` file with the following configuration:

- Build command: `yarn build`
- Output directory: `dist`
- Node.js runtime: 18.x

## 5. Deployment Steps

1. **Push to GitHub** (if you have changes):
   ```bash
   git add .
   git commit -m "Configure for deployment"
   git push origin master
   ```

2. **Import to Vercel**:
   - Go to Vercel dashboard
   - Click "New Project"
   - Import from GitHub
   - Select this repository

3. **Configure Environment Variables**:
   - Add all the environment variables listed above
   - Replace placeholder values with your actual values

4. **Deploy**:
   - Click "Deploy"
   - Wait for the build to complete

## 6. Post-Deployment

After successful deployment:

1. Visit your Vercel URL
2. The database tables will be automatically created on first run
3. You can access the admin panel at `https://your-app.vercel.app/admin`
4. Default login credentials (as configured):
   - Email: `demo@payloadcms.com`
   - Password: `demo`

## 7. Troubleshooting

- **Build failures**: Check the Vercel build logs for specific errors
- **Database connection issues**: Verify your DATABASE_URL is correct
- **Environment variables**: Ensure all required variables are set in Vercel
- **CORS issues**: Make sure PAYLOAD_PUBLIC_SERVER_URL matches your actual domain

## 8. Security Notes

- Change default admin credentials immediately after first login
- Use strong, unique secrets for production
- Enable row-level security in Supabase if needed
- Consider setting up proper authentication for production use