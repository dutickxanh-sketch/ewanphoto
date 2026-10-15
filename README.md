# Photoshop Community

A full-stack community MVP for sharing images and posts, discovering trending work, liking, commenting, following creators, and moderating content. The interface is English-first and responsive.

## Stack
- React + TypeScript + Vite frontend
- Express + TypeScript REST API
- PostgreSQL + Prisma ORM
- JWT in an HttpOnly cookie, bcrypt password hashing, Zod validation, Helmet, CORS allow-list, and rate limiting
- Local uploads for development (use private/object storage such as S3 or Cloudinary for production)

## Requirements
Node.js 20+, npm 10+, PostgreSQL 15+.

## Run locally
1. Create a PostgreSQL database named `photoshop_community`.
2. Copy `.env.example` to `server/.env`; configure `DATABASE_URL`, and set a random `JWT_SECRET` of 32+ characters.
3. From the repository root run `npm install`.
4. Run `npm run db:generate`, then `npm run db:migrate -w server -- --name init`, and optionally `npm run db:seed`.
5. Start both apps with `npm run dev`. Frontend: http://localhost:5173, API: http://localhost:4000/api/health.

## Environment variables
- `DATABASE_URL`: PostgreSQL connection string.
- `JWT_SECRET`: private random signing secret; never put it in client code.
- `CLIENT_ORIGIN`: exact frontend origin allowed by CORS.
- `PORT`: API port (defaults to 4000).
- `NODE_ENV`: `development` or `production`.
- `UPLOAD_DIR`: upload directory relative to server (defaults to `uploads`).

## Features
- Sign up, sign in, sign out, current session endpoint
- Create posts with optional image uploads; image MIME/type/size checks
- Feed search, sort by recent or trending, hashtag topics
- Like/unlike posts, comment, follow/unfollow creators
- Profile stats and a protected admin overview/moderation endpoint
- Admin can remove posts and suspend users; only an existing ADMIN can do so
- Responsive layout, validation, empty/error/loading states, and accessible form labels

## Admin setup
The seed script creates a demo admin only when `SEED_ADMIN_EMAIL` and `SEED_ADMIN_PASSWORD` are set in the server environment. Use a strong unique password and remove or rotate the account before production. Do not deploy with demo credentials.

## Production checklist
- Deploy the API and frontend to trusted providers; configure HTTPS and exact `CLIENT_ORIGIN`.
- Use managed PostgreSQL, run Prisma migrations during release, and configure backups.
- Replace local uploads with object storage + malware scanning; local disk is ephemeral on many hosts.
- Set `NODE_ENV=production`, a high-entropy `JWT_SECRET`, secure cookie settings, request/body limits, monitoring, and log redaction.
- Add email verification/password reset, CSRF strategy appropriate to your deployment, moderation/report workflows, and automated tests before public launch.
- Build frontend with `npm run build`; serve `client/dist` from a static host. Run API with `npm start -w server`.

## Known scope limits
This is a runnable MVP, not a production-audited social network. Email verification, password recovery, cloud image storage, direct messaging, notifications, formal content reporting, and automated integration tests are not included. API tests/build must be run in an environment with dependencies and PostgreSQL available.
