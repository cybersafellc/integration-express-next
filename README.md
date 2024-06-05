# Integration Express js - Next js Freamworks

## Introduction

maybe some of us, including me, hate the make api that is provided by next js, namely the route handler, I hate it because it is difficult to custom middleware which has to be shared with page middleware and route handler middleware, riweh (in Javanese). Moreover, if you have been using Express JS for a long time, it's as if you don't accept the use of a route handler, not because it's difficult, just because it's strange. Therefore, the solution we can use is to develop our fullstack application by integrating NextJS with ExpressJS so that we can create an API like how we normally create a restfull API.

When you run Next.js with Express.js as a custom server, there is only one server that needs to be running, namely the Express.js server that handles all requests, including those directed to Next.js. You don't need to run Next.js separately because the Express.js server will prepare and handle all Next.js requests.

Below are detailed steps to ensure a Next.js server with Express.js is running correctly on a single port.

## Step by step

- 1. package.json Skrip

```json
{
  "name": "my-nextjs-app",
  "version": "1.0.0",
  "scripts": {
    "dev": "node server/app/server.js",
    "build": "next build",
    "start": "NODE_ENV=production node server/app/server.js"
  },
  "dependencies": {
    "express": "^4.17.1",
    "next": "latest",
    "react": "latest",
    "react-dom": "latest"
  }
}
```

- 2. File Server: server/app/server.js

````markdown
```javascript
import express from "express";
import next from "next";

const dev = process.env.NODE_ENV !== "production";
const app = next({ dev });
const handle = app.getRequestHandler();

app.prepare().then(() => {
  const server = express();

  server.use((req, res, next) => {
    console.log("Middleware dijalankan");
    next();
  });

  server.get("/api", (req, res) => {
    console.log("Route /api diakses");
    res.status(200).json({
      message: "api aman",
    });
  });

  server.all("*", (req, res) => {
    console.log("Request diterima oleh Next.js handler");
    return handle(req, res);
  });

  const port = 3000;
  server.listen(port, (err) => {
    if (err) throw err;
    console.log(`Server berjalan di http://localhost:${port}`);
  });
});
```
````

- 3. Run server

```bash
$ npm run dev

```

- 4. example folder & file structure

```bash
my-nextjs-app/
├── public/
├── server/
│   ├── app/
│   │   ├── server.js
├── src/
│   ├── app/
│   │   ├── page.js
│   │   ├── layout.js
│   │   ├── dashboard/
│   │   │   ├── page.js
│   │   ├── profile/
│   │   │   ├── page.js
├── .env.local
├── next.config.js
├── package.json

```
