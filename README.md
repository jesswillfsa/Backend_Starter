# Backend Starter Repo
This README walks through the structure and setup of your backend repo. Index.js has some sample code, but I highly encourage you to read through plan.md for step by step instructions for setting up your backend from imports to your server.

## Quick Start/Testing
Clone the repo <strong>(DO NOT FORK)</strong>, install dependencies, and start the server:
 ```bash
 git clone <repo-url>
 cd backend-starter-repo
 npm install
 npm run dev
 ```

## Project Structure Options

### Files required for either Structure
- package.json
- .gitignore
- .env
- ReadME (.md file)

#### Creating package.json 
You can create a `package.json` file with:

```bash
npm init -y
```
and then add the following scripts to your `package.json` file:
```json
"scripts": {
  "start": "node index.js",
  "dev": "nodemon index.js"
}
```
#### Dependencies

After initializing your project and creating the `package.json` file, install the required packages:

```bash
npm install express pg dotenv cors
npm install --save-dev nodemon
```
(If you are unable to install pg, follow the instructions in psql-setup.md before reaching out for assistance)

### Environment Variables

Create a `.env` file in the root directory with the following content:

`DB_USER=your_username`
`DB_HOST=localhost`
`DB_NAME=your_database`
`DB_PASSWORD=your_password`
`DB_PORT=5432`

Make sure not to commit/push this file to GitHub!

#### Important: Add `.env` and `node_modules` to `.gitignore`

To avoid accidentally exposing sensitive data, create a `.gitignore` file and add the `.env` file to your `.gitignore`. This ensures the environment file won’t be tracked by Git or pushed to a public repository. Adding `node_modules` to the gitignore will keep the repo small and prevent errors when trying to `npm install`

### Option 1: Simple (Single File-ish)

```
 backend-starter-repo/
 ├── .env
 ├── .gitignore
 ├── package.json
 ├── package-lock.json
 ├── index.js
 ├── README.md
```
- `index.js`: Contains everything (server, routes, database connection).
- Great for small projects and quick prototypes.

### Option 2: Modular (Multiple Files)
```
 backend-starter-repo/
 ├── .env
 ├── .gitignore
 ├── package.json
 ├── package-lock.json
 ├── index.js
 ├── db/
 │   └── client.js            # Database connection
 ├── routes/
 │   └── userRoutes.js        # User-related routes
 ├── controllers/
 │   └── userController.js    # User logic
 └── README.md
 ```

 - Better for larger projects with complex logic.
 - Easier to maintain and scale.

 Pick the structure that you are most comfortable with and fits your project best!

## Helpful Resources
- [Express.js Documentation](https://expressjs.com/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [Node.js Documentation](https://nodejs.org/en/docs)
- [dotenv Package](https://www.npmjs.com/package/dotenv)
- [MDN Web Docs - HTTP Status Codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
- [REST API Design Guide](https://www.restapitutorial.com/)