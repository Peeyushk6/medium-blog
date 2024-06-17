# Medium Blog Project

We’ll be building a medium-like blog platform using the following stack:

1. **Backend**: Cloudflare Workers
2. **Validation**: Zod - By publishing your package to npm
3. **Language**: TypeScript
4. **ORM**: Prisma (with connection pooling)
5. **Database**: PostgreSQL
6. **Authentication**: JWT

## Project Structure

Our project will be divided into three parts:

1. **Backend**
2. **Frontend**
3. **Common**: This will contain all the shared components between the frontend and backend, and will be an independent npm module.

## Steps to Initialize the Project

### Step 1: Initialize the Backend

1. Create a new folder called `medium-blog` and navigate into it:

   ```sh
   mkdir medium-blog
   cd medium-blog
   ```

2. Initialize a Hono-based Cloudflare Worker app:

   ```sh
   npm create hono@latest
   ```

### Step 2: Handlers

Define the following API endpoints:

- `POST /api/v1/user/signup`
- `POST /api/v1/user/signin`
- `POST /api/v1/blog`
- `PUT /api/v1/blog`
- `GET /api/v1/blog/:id`
- `GET /api/v1/blog/bulk`

### Step 3: Initialize the Database (Prisma)

1. Obtain your connection URL from a database provider like Neon or Aiven.

   Example:

   ```sh
   postgres://avnadmin:password@host/db
   ```

2. Obtain the connection pool URL from Prisma Accelerate.

   Example:

   ```sh
   prisma://accelerate.prisma-data.net/?api_key=your_api_key --dummy
   ```

3. Initialize Prisma in your project:

   ```sh
   npm install prisma
   npx prisma init
   ```

4. Replace `DATABASE_URL` in your `.env` file:

   ```env
   DATABASE_URL="postgres://avnadmin:password@host/db"
   ```

5. Add `DATABASE_URL` as the connection pool URL in `wrangler.toml`:

   ```toml
   name = "backend"
   compatibility_date = "2023-12-01"

   [vars]
   DATABASE_URL = "prisma://accelerate.prisma-data.net/?api_key=your_api_key --dummy"
   ```

6. Initialize the Prisma schema:

   Refer to `/medium-blog/backend/prisma`

7. Generate the Prisma client:

   ```sh
   npx prisma generate --no-engine
   ```

8. Add the Prisma Accelerate extension:

   ```sh
   npm install @prisma/extension-accelerate
   ```

9. Initialize the Prisma client in your code.

### Step 4: Coding

Perform all the necessary coding tasks for your application.

### Step 5: Deploy Your App

1. Deploy your app on the Cloudflare dashboard.
2. Update your environment variables from the Cloudflare app.
3. Ensure you are logged into the Cloudflare CLI:

   ```sh
   npx wrangler login
   ```

4. Deploy your app:

   ```sh
   npm run deploy
   ```

5. Test your production URL in Postman to ensure it works.

### Step 6: Initialize the Common Module

1. Create a new folder called `common` and initialize an empty TypeScript project in it:

   ```sh
   mkdir common
   cd common
   npm init -y
   npx tsc --init
   ```

2. Update `tsconfig.json`:

   ```json
   {
     "rootDir": "./src",
     "outDir": "./dist",
     "declaration": true
   }
   ```

3. Run `npm login`.

4. Update the `package.json` to reflect your npm namespace and main entry point:

   ```json
   {
     "name": "@peeyushk6/medium-common",
     "version": "1.0.0",
     "description": "",
     "main": "dist/index.js",
     "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1"
     },
     "keywords": [],
     "author": "",
     "license": "ISC"
   }
   ```

5. Add `src` to `.npmignore`.

6. Install `zod`:

   ```sh
   npm install zod
   ```

7. Build and publish the package:

   ```sh
   tsc -b
   npm publish --access public
   ```

8. Explore your package on npmjs.com.

### Step 7: Import Zod in the Backend

1. Install the package you published:

   ```sh
   npm install @peeyushk6/medium-common
   ```

2. Explore the package:

   ```sh
   cd node_modules/@peeyushk6/medium-common
   ```

## Note

This repository does not contain the frontend code.
