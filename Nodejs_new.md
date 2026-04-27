# Node.js Interview Questions

> **Section:** Node.js
> **Total Questions:** 52
> **Difficulty:** 40% Basic · 40% Intermediate · 20% Practical
> **Focus:** Event loop, streams, modules, Express, error handling, performance

---

## Basic Questions

---

## Q1. What is Node.js?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is Node.js? What makes it different from running JavaScript in the browser?

### Answer
Node.js is a **JavaScript runtime** built on Chrome's V8 engine that allows JavaScript to run on the server. Key differences from browser JS:
- No DOM, `window`, or browser APIs
- Has `process`, `fs`, `http`, `path` modules
- Uses CommonJS (`require`) by default (also supports ESM)
- Non-blocking I/O via libuv

---

## Q2. What is the Node.js Event Loop?
**Difficulty:** Basic | **Type:** Conceptual

### Question
Describe the Node.js event loop and its phases.

### Answer
The event loop allows Node.js to perform **non-blocking I/O** despite being single-threaded, by offloading operations to the OS/libuv and processing callbacks when they complete.

**Phases (in order):**
1. **Timers** — `setTimeout`, `setInterval` callbacks
2. **Pending callbacks** — I/O errors from previous cycle
3. **Idle/Prepare** — internal use
4. **Poll** — retrieve new I/O events
5. **Check** — `setImmediate` callbacks
6. **Close callbacks** — `socket.on("close")`

**Between phases:** microtasks (`process.nextTick`, Promises) drain completely.

### Code
```javascript
console.log("1 - sync");

setTimeout(() => console.log("2 - setTimeout"), 0);

setImmediate(() => console.log("3 - setImmediate"));

Promise.resolve().then(() => console.log("4 - Promise"));

process.nextTick(() => console.log("5 - nextTick"));

console.log("6 - sync");

// Output: 1, 6, 5, 4, 2, 3
// (sync → nextTick → Promise microtask → timer → immediate)
```

---

## Q3. What is `process.nextTick`?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is `process.nextTick`? How does it differ from `setImmediate`?

### Answer
- `process.nextTick` — runs **before** the next event loop iteration, after the current operation, before I/O and timers. Highest priority among async callbacks.
- `setImmediate` — runs in the **Check phase** of the current iteration, after I/O events.

### Code
```javascript
setImmediate(() => console.log("setImmediate"));
process.nextTick(() => console.log("nextTick"));
Promise.resolve().then(() => console.log("Promise"));

// Output: nextTick → Promise → setImmediate
```

---

## Q4. What is `require` vs `import`?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is the difference between CommonJS `require` and ES Module `import` in Node.js?

### Answer
| Feature         | CommonJS (`require`)      | ESM (`import`)           |
|----------------|---------------------------|--------------------------|
| Execution       | Synchronous               | Asynchronous             |
| Loading time    | Runtime                   | Parse/compile time       |
| Tree shaking    | No                        | Yes                      |
| Default in Node | Yes                       | Opt-in (`.mjs` or `"type":"module"`) |
| `__dirname`     | Available                 | Not available (use `import.meta.url`) |

---

## Q5. What is `module.exports` vs `exports`?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is the difference between `module.exports` and `exports`?

### Answer
`exports` is a **reference** to `module.exports`. Reassigning `exports` breaks the link. Always use `module.exports` when exporting a single value or class.

### Code
```javascript
// ✅ Safe — both work for adding properties
exports.add = (a, b) => a + b;
module.exports.subtract = (a, b) => a - b;

// ❌ Broken — reassigning exports breaks the link
exports = { add: (a, b) => a + b }; // module.exports still {}

// ✅ Correct — reassign module.exports
module.exports = { add: (a, b) => a + b };
module.exports = class UserService { /* ... */ };
```

---

## Q6. What is the `fs` module?
**Difficulty:** Basic | **Type:** Conceptual

### Question
How do you read and write files in Node.js? What's the difference between sync and async versions?

### Answer
The `fs` module provides file system operations. Prefer async versions in production to avoid blocking the event loop.

### Code
```javascript
const fs = require("fs");
const fsp = require("fs").promises;

// Sync (blocks event loop — avoid in servers)
const data = fs.readFileSync("file.txt", "utf8");

// Callback-based async
fs.readFile("file.txt", "utf8", (err, data) => {
  if (err) throw err;
  console.log(data);
});

// Promise-based (preferred)
async function readFile() {
  const data = await fsp.readFile("file.txt", "utf8");
  await fsp.writeFile("out.txt", data.toUpperCase());
}
```

---

## Q7. What is middleware in Express?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is middleware in Express.js? How does the request-response cycle work?

### Answer
Middleware is a function with signature `(req, res, next)` that sits in the request pipeline. It can: modify req/res, end the request, or pass to the next middleware via `next()`.

### Code
```javascript
const express = require("express");
const app = express();

// Application-level middleware
app.use(express.json()); // parse JSON body
app.use(express.urlencoded({ extended: true }));

// Custom middleware
function requestLogger(req, res, next) {
  console.log(`${req.method} ${req.path} - ${Date.now()}`);
  next(); // must call next() or the request hangs!
}
app.use(requestLogger);

// Route-level middleware
function authMiddleware(req, res, next) {
  if (!req.headers.authorization) {
    return res.status(401).json({ error: "Unauthorized" });
  }
  next();
}
app.get("/protected", authMiddleware, (req, res) => res.json({ data: "secret" }));
```

---

## Q8. What are streams in Node.js?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What are streams? What are the four types?

### Answer
Streams are **objects that let you read/write data piece by piece** without loading everything into memory. Four types:
1. **Readable** — source of data (file read, HTTP request)
2. **Writable** — destination (file write, HTTP response)
3. **Duplex** — both readable and writable (TCP socket)
4. **Transform** — duplex that transforms data (gzip, encryption)

### Code
```javascript
const fs = require("fs");
const zlib = require("zlib");

// Piping streams (memory efficient)
fs.createReadStream("large.txt")
  .pipe(zlib.createGzip())
  .pipe(fs.createWriteStream("large.txt.gz"))
  .on("finish", () => console.log("Compressed!"));

// vs. readFile (loads entire file into memory)
// const data = await fs.promises.readFile("large.txt"); // bad for large files
```

---

## Q9. What is the `path` module?
**Difficulty:** Basic | **Type:** Conceptual

### Question
Why should you use the `path` module instead of string concatenation for file paths?

### Answer
The `path` module handles OS-specific path separators (`/` vs `\`) and edge cases like trailing slashes.

### Code
```javascript
const path = require("path");

path.join("/users", "alice", "docs", "file.txt"); // "/users/alice/docs/file.txt"
path.resolve("./config", "../data");              // absolute path
path.basename("/users/alice/file.txt");           // "file.txt"
path.extname("index.html");                       // ".html"
path.dirname("/users/alice/file.txt");            // "/users/alice"

// __dirname is the directory of the current file
const configPath = path.join(__dirname, "config", "app.json");
```

---

## Q10. What is `package.json` and what are the key fields?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What are the important fields in `package.json`?

### Answer
Key fields:
- `name`, `version` — package identity
- `main` — entry point for CommonJS
- `module` — entry point for ESM
- `scripts` — npm run commands
- `dependencies` — production packages
- `devDependencies` — dev-only packages
- `engines` — required Node/npm versions
- `type: "module"` — enables ESM

---

## Q11. What is `npm` vs `npx`?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is the difference between `npm` and `npx`?

### Answer
- `npm` — installs packages and manages dependencies
- `npx` — executes a package **without installing it globally**, using the local `node_modules/.bin` or downloading temporarily

### Code
```bash
# npm — installs
npm install express
npm run dev

# npx — executes without global install
npx create-react-app my-app
npx ts-node script.ts
npx nodemon server.js  # uses local nodemon if installed
```

---

## Q12. How does error handling work in Express?
**Difficulty:** Basic | **Type:** Conceptual

### Question
How do you write an error-handling middleware in Express?

### Answer
Error-handling middleware has **4 parameters**: `(err, req, res, next)`. Express calls it automatically when you call `next(err)` or when a sync error is thrown in a route.

### Code
```javascript
// Trigger error
app.get("/user/:id", async (req, res, next) => {
  try {
    const user = await getUser(req.params.id);
    if (!user) throw Object.assign(new Error("Not found"), { status: 404 });
    res.json(user);
  } catch (err) {
    next(err); // pass to error handler
  }
});

// Error-handling middleware (must be LAST, 4 params)
app.use((err, req, res, next) => {
  const status = err.status || 500;
  const message = err.message || "Internal Server Error";
  console.error(err.stack);
  res.status(status).json({ error: message });
});
```

---

## Q13. What is `dotenv` and how do you use it?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is `dotenv`? How do you manage environment variables in Node.js?

### Answer
`dotenv` loads variables from a `.env` file into `process.env`. Never commit `.env` to version control.

### Code
```javascript
// .env file
// DB_HOST=localhost
// DB_PORT=5432
// JWT_SECRET=supersecret

require("dotenv").config();

// Access variables
const dbHost = process.env.DB_HOST;
const port = parseInt(process.env.PORT) || 3000;

// Validate required env vars at startup
const required = ["DB_HOST", "JWT_SECRET"];
required.forEach(key => {
  if (!process.env[key]) throw new Error(`Missing env var: ${key}`);
});
```

---

## Q14. What is `nodemon`?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is `nodemon` and when do you use it?

### Answer
`nodemon` is a development tool that **automatically restarts the Node.js server** when file changes are detected. Only used in development.

### Code
```json
// package.json
{
  "scripts": {
    "dev": "nodemon server.js",
    "start": "node server.js"
  }
}
```

---

## Q15. What is the `http` module?
**Difficulty:** Basic | **Type:** Conceptual

### Question
How do you create a basic HTTP server using Node's built-in `http` module?

### Answer

### Code
```javascript
const http = require("http");

const server = http.createServer((req, res) => {
  if (req.method === "GET" && req.url === "/") {
    res.writeHead(200, { "Content-Type": "application/json" });
    res.end(JSON.stringify({ message: "Hello World" }));
  } else {
    res.writeHead(404);
    res.end("Not Found");
  }
});

server.listen(3000, () => console.log("Server running on port 3000"));
```

---

## Intermediate Questions

---

## Q16. What is the cluster module? How do you scale Node.js across CPU cores?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
Since Node.js is single-threaded, how do you use multiple CPU cores?

### Answer
The `cluster` module lets you spawn multiple worker processes that share the same port. Each worker is a separate Node.js process with its own event loop.

### Code
```javascript
const cluster = require("cluster");
const os = require("os");

if (cluster.isPrimary) {
  const numCPUs = os.cpus().length;
  console.log(`Primary ${process.pid} starting ${numCPUs} workers`);

  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  cluster.on("exit", (worker) => {
    console.log(`Worker ${worker.process.pid} died. Restarting...`);
    cluster.fork(); // auto-restart
  });
} else {
  // Each worker runs its own Express server
  const app = require("./app");
  app.listen(3000, () => console.log(`Worker ${process.pid} listening`));
}
```

---

## Q17. What is the `worker_threads` module?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What is the difference between `cluster` and `worker_threads`?

### Answer
- **`cluster`**: Multiple Node.js processes (separate memory, event loops). Best for HTTP servers.
- **`worker_threads`**: Multiple threads **within one process** (shared memory via `SharedArrayBuffer`). Best for CPU-intensive tasks.

### Code
```javascript
const { Worker, isMainThread, parentPort } = require("worker_threads");

if (isMainThread) {
  const worker = new Worker(__filename);
  worker.on("message", result => console.log("Result:", result));
  worker.postMessage({ task: "compute", data: 1000000 });
} else {
  parentPort.on("message", ({ data }) => {
    // CPU-intensive work runs in separate thread
    const result = heavyComputation(data);
    parentPort.postMessage(result);
  });
}
```

---

## Q18. What is event-driven architecture? How does EventEmitter work?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
How does Node.js `EventEmitter` work? Give a practical example.

### Answer

### Code
```javascript
const EventEmitter = require("events");

class OrderService extends EventEmitter {
  async placeOrder(order) {
    // Process order
    await saveOrder(order);

    // Emit events instead of directly calling other services
    this.emit("order:created", order);
    this.emit("order:payment-required", { orderId: order.id, amount: order.total });
  }
}

const orderService = new OrderService();

// Listeners (could be in separate modules)
orderService.on("order:created", async (order) => {
  await sendConfirmationEmail(order.userId);
});

orderService.on("order:created", async (order) => {
  await updateInventory(order.items);
});

// Once listener — fires only once
orderService.once("order:payment-required", async ({ orderId, amount }) => {
  await initiatePayment(orderId, amount);
});
```

---

## Q19. How do you handle unhandled promise rejections in Node.js?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What happens with unhandled promise rejections in Node.js? How should you handle them?

### Answer
In modern Node.js (v15+), unhandled rejections **crash the process** by default. You should handle them explicitly.

### Code
```javascript
// Global handlers for unexpected errors
process.on("unhandledRejection", (reason, promise) => {
  console.error("Unhandled Rejection at:", promise, "reason:", reason);
  // Optionally: log to monitoring service, then exit
  process.exit(1);
});

process.on("uncaughtException", (error) => {
  console.error("Uncaught Exception:", error);
  // Cleanup, then exit (process is in unknown state)
  process.exit(1);
});

// Express async route wrapper to avoid try/catch everywhere
const asyncHandler = (fn) => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);

app.get("/users", asyncHandler(async (req, res) => {
  const users = await User.findAll();
  res.json(users);
}));
```

---

## Q20. What is rate limiting and how do you implement it in Express?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
How do you protect an Express API from abuse with rate limiting?

### Answer

### Code
```javascript
const rateLimit = require("express-rate-limit");

// Global rate limit
const globalLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100,
  message: { error: "Too many requests, please try again later" },
  standardHeaders: true,
  legacyHeaders: false,
});

// Stricter limit for auth routes
const authLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 5, // only 5 login attempts per 15 min
  skipSuccessfulRequests: true,
});

app.use(globalLimiter);
app.post("/auth/login", authLimiter, loginController);
```

---

## Q21. How do you secure an Express application?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
List security best practices for an Express API.

### Answer
Key security measures:

### Code
```javascript
const helmet = require("helmet");
const cors = require("cors");

// 1. Helmet — sets security headers
app.use(helmet());

// 2. CORS — control origins
app.use(cors({
  origin: ["https://yourapp.com"],
  methods: ["GET", "POST", "PUT", "DELETE"],
  credentials: true,
}));

// 3. Input validation (express-validator)
const { body, validationResult } = require("express-validator");
app.post("/user",
  body("email").isEmail(),
  body("password").isLength({ min: 8 }),
  (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) return res.status(400).json({ errors: errors.array() });
    // proceed
  }
);

// 4. Avoid sending stack traces in production
app.use((err, req, res, next) => {
  res.status(500).json({
    error: process.env.NODE_ENV === "production" ? "Server error" : err.message
  });
});
```

---

## Q22. What is JWT authentication and how do you implement it?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
Explain how JWT-based authentication works in a Node.js API.

### Answer
JWT (JSON Web Token) is a signed token containing claims. The server signs it at login; clients send it with requests; the server verifies the signature — no session storage needed.

### Code
```javascript
const jwt = require("jsonwebtoken");

// Sign token at login
app.post("/auth/login", async (req, res) => {
  const { email, password } = req.body;
  const user = await User.findOne({ email });
  if (!user || !(await bcrypt.compare(password, user.passwordHash))) {
    return res.status(401).json({ error: "Invalid credentials" });
  }

  const token = jwt.sign(
    { userId: user.id, email: user.email },
    process.env.JWT_SECRET,
    { expiresIn: "7d" }
  );
  res.json({ token });
});

// Verify token middleware
function authenticate(req, res, next) {
  const auth = req.headers.authorization;
  if (!auth?.startsWith("Bearer ")) return res.status(401).json({ error: "No token" });

  try {
    const payload = jwt.verify(auth.slice(7), process.env.JWT_SECRET);
    req.user = payload;
    next();
  } catch {
    res.status(401).json({ error: "Invalid token" });
  }
}
```

---

## Q23. What is connection pooling in Node.js?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What is a database connection pool? Why is it important in Node.js?

### Answer
A connection pool maintains a **set of reusable database connections** instead of creating/destroying one per request. Reduces connection overhead and prevents exhausting DB connections.

### Code
```javascript
const { Pool } = require("pg");

const pool = new Pool({
  host: process.env.DB_HOST,
  database: process.env.DB_NAME,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  port: 5432,
  max: 20,           // max pool size
  idleTimeoutMillis: 30000,
  connectionTimeoutMillis: 2000,
});

// Pool reuses connections
async function getUser(id) {
  const { rows } = await pool.query(
    "SELECT * FROM users WHERE id = $1",
    [id]
  );
  return rows[0];
}

// Graceful shutdown
process.on("SIGTERM", async () => {
  await pool.end();
  process.exit(0);
});
```

---

## Q24. How do you implement caching in Node.js?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
How would you cache API responses in a Node.js service?

### Answer

### Code
```javascript
const redis = require("ioredis");
const client = new redis(process.env.REDIS_URL);

// Cache middleware
function cacheMiddleware(ttlSeconds) {
  return async (req, res, next) => {
    const key = `cache:${req.url}`;
    try {
      const cached = await client.get(key);
      if (cached) {
        return res.json(JSON.parse(cached));
      }
    } catch (err) {
      console.error("Cache read error:", err);
    }

    // Override res.json to cache the response
    const originalJson = res.json.bind(res);
    res.json = async (data) => {
      try {
        await client.setex(key, ttlSeconds, JSON.stringify(data));
      } catch (err) {
        console.error("Cache write error:", err);
      }
      return originalJson(data);
    };

    next();
  };
}

app.get("/products", cacheMiddleware(300), getProducts);
```

---

## Q25. What is graceful shutdown in Node.js?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What is a graceful shutdown? How do you implement it?

### Answer
Graceful shutdown means **completing in-flight requests** and cleaning up resources (DB connections, queues) before exiting, instead of killing the process abruptly.

### Code
```javascript
const server = app.listen(3000);

async function shutdown(signal) {
  console.log(`Received ${signal}. Starting graceful shutdown...`);

  // Stop accepting new connections
  server.close(async () => {
    console.log("HTTP server closed");

    // Cleanup
    await pool.end();
    await redisClient.quit();

    console.log("Cleanup complete. Exiting.");
    process.exit(0);
  });

  // Force exit after 30s (for stuck requests)
  setTimeout(() => {
    console.error("Forced shutdown after timeout");
    process.exit(1);
  }, 30000);
}

process.on("SIGTERM", () => shutdown("SIGTERM")); // Docker/K8s stop
process.on("SIGINT", () => shutdown("SIGINT"));   // Ctrl+C
```

---

## Q26. What is the difference between `readFile` and `createReadStream`?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
When should you use streams over `readFile`?

### Answer
- `readFile` — loads the **entire file into memory** before processing. Simple, but bad for large files.
- `createReadStream` — processes file **in chunks**. Memory-efficient for large files (logs, CSV, video).

### Code
```javascript
// ❌ readFile — loads 2GB file into memory
app.get("/download", (req, res) => {
  const data = fs.readFileSync("large-video.mp4"); // 2GB in RAM!
  res.send(data);
});

// ✅ createReadStream — streams chunks to client
app.get("/download", (req, res) => {
  res.setHeader("Content-Type", "video/mp4");
  fs.createReadStream("large-video.mp4").pipe(res);
});

// With range support for video seeking
app.get("/video", (req, res) => {
  const { range } = req.headers;
  const start = Number(range.replace(/\D/g, ""));
  const end = Math.min(start + 10 ** 6, fileSize - 1);
  const stream = fs.createReadStream("video.mp4", { start, end });
  res.writeHead(206, { "Content-Range": `bytes ${start}-${end}/${fileSize}` });
  stream.pipe(res);
});
```

---

## Q27. How does async/await error handling work in Express routes?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What happens if an async route throws and you don't catch it?

### Answer
In Express 4, unhandled async errors in routes **do NOT reach the error handler** — they become unhandled promise rejections. You must either wrap in try/catch or use an async wrapper.

### Code
```javascript
// ❌ Unhandled — Express 4 doesn't catch this
app.get("/user", async (req, res) => {
  const user = await User.findById(req.params.id); // if this throws...
  res.json(user); // ...this line never runs, error goes unhandled
});

// ✅ Option 1: try/catch
app.get("/user", async (req, res, next) => {
  try {
    const user = await User.findById(req.params.id);
    res.json(user);
  } catch (err) {
    next(err);
  }
});

// ✅ Option 2: wrapper utility
const asyncHandler = fn => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);

app.get("/user", asyncHandler(async (req, res) => {
  const user = await User.findById(req.params.id);
  res.json(user);
}));

// ✅ Option 3: Express 5 (handles async automatically)
```

---

## Q28. What is CORS and how do you configure it in Node.js?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What is CORS? How do you configure it properly in Express?

### Answer
CORS (Cross-Origin Resource Sharing) is a browser security mechanism that **blocks requests from different origins** unless the server explicitly allows them.

### Code
```javascript
const cors = require("cors");

// Development — allow all (never in production)
app.use(cors());

// Production — explicit configuration
app.use(cors({
  origin: (origin, callback) => {
    const allowed = ["https://app.example.com", "https://admin.example.com"];
    if (!origin || allowed.includes(origin)) {
      callback(null, true);
    } else {
      callback(new Error("Not allowed by CORS"));
    }
  },
  methods: ["GET", "POST", "PUT", "DELETE", "OPTIONS"],
  allowedHeaders: ["Content-Type", "Authorization"],
  credentials: true,
  maxAge: 86400, // preflight cache: 24 hours
}));
```

---

## Q29. How do you structure a Node.js/Express project?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What is a good project structure for a mid-size Express API?

### Answer
```
src/
├── config/          # env, db config
├── controllers/     # request/response logic
├── services/        # business logic
├── repositories/    # database queries
├── models/          # data schemas/types
├── middleware/      # auth, validation, logging
├── routes/          # route definitions
├── utils/           # helpers, constants
└── app.js           # Express app setup
server.js            # starts the HTTP server
```

**Principle:** Controllers are thin (handle HTTP). Services contain business logic. Repositories handle DB queries. This makes testing easier.

---

## Q30. What is logging best practice in Node.js?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
How should you implement logging in a production Node.js app?

### Answer
Use a structured logging library like `winston` or `pino` (never `console.log` in production). Log as JSON for easy parsing in monitoring tools.

### Code
```javascript
const pino = require("pino");

const logger = pino({
  level: process.env.LOG_LEVEL || "info",
  transport: process.env.NODE_ENV !== "production"
    ? { target: "pino-pretty" }
    : undefined,
});

// Request logging middleware
app.use((req, res, next) => {
  const start = Date.now();
  res.on("finish", () => {
    logger.info({
      method: req.method,
      url: req.url,
      status: res.statusCode,
      duration: Date.now() - start,
      requestId: req.id,
    });
  });
  next();
});

// Log with context
logger.error({ err: error, userId: req.user?.id }, "Failed to process order");
```

---

## Practical / Scenario Questions

---

## Q31. How would you build a file upload endpoint in Express?
**Difficulty:** Practical | **Type:** Coding

### Answer

### Code
```javascript
const multer = require("multer");
const path = require("path");

const storage = multer.diskStorage({
  destination: "./uploads/",
  filename: (req, file, cb) => {
    const unique = `${Date.now()}-${Math.round(Math.random() * 1E9)}`;
    cb(null, `${unique}${path.extname(file.originalname)}`);
  }
});

const upload = multer({
  storage,
  limits: { fileSize: 5 * 1024 * 1024 }, // 5MB
  fileFilter: (req, file, cb) => {
    const allowed = /jpeg|jpg|png|gif/;
    const isValid = allowed.test(path.extname(file.originalname).toLowerCase())
      && allowed.test(file.mimetype);
    cb(isValid ? null : new Error("Only images allowed"), isValid);
  }
});

app.post("/upload", upload.single("avatar"), (req, res) => {
  if (!req.file) return res.status(400).json({ error: "No file uploaded" });
  res.json({ url: `/uploads/${req.file.filename}` });
});

app.use((err, req, res, next) => {
  if (err instanceof multer.MulterError || err.message === "Only images allowed") {
    return res.status(400).json({ error: err.message });
  }
  next(err);
});
```

---

## Q32. How would you implement request validation middleware?
**Difficulty:** Practical | **Type:** Coding

### Answer

### Code
```javascript
const Joi = require("joi");

function validate(schema) {
  return (req, res, next) => {
    const { error, value } = schema.validate(req.body, { abortEarly: false });
    if (error) {
      const messages = error.details.map(d => d.message);
      return res.status(400).json({ errors: messages });
    }
    req.body = value; // use sanitized/coerced values
    next();
  };
}

const createUserSchema = Joi.object({
  name: Joi.string().min(2).max(50).required(),
  email: Joi.string().email().required(),
  age: Joi.number().integer().min(18).max(120).required(),
  role: Joi.string().valid("admin", "user", "moderator").default("user"),
});

app.post("/users", validate(createUserSchema), async (req, res) => {
  const user = await User.create(req.body);
  res.status(201).json(user);
});
```

---

## Q33. Build a simple in-memory queue processor.
**Difficulty:** Practical | **Type:** Coding

### Answer

### Code
```javascript
class JobQueue {
  constructor(concurrency = 3) {
    this.queue = [];
    this.running = 0;
    this.concurrency = concurrency;
  }

  add(job) {
    return new Promise((resolve, reject) => {
      this.queue.push({ job, resolve, reject });
      this.process();
    });
  }

  async process() {
    while (this.running < this.concurrency && this.queue.length > 0) {
      const { job, resolve, reject } = this.queue.shift();
      this.running++;
      try {
        const result = await job();
        resolve(result);
      } catch (err) {
        reject(err);
      } finally {
        this.running--;
        this.process();
      }
    }
  }
}

// Usage
const queue = new JobQueue(3); // max 3 concurrent
const results = await Promise.all(
  urls.map(url => queue.add(() => fetch(url).then(r => r.json())))
);
```

---

## Q34. What would you check if a Node.js service is slow?
**Difficulty:** Practical | **Type:** Scenario

### Question
Production Node.js service is responding slowly. Walk through your debugging approach.

### Answer
**Step-by-step:**
1. **Check event loop lag** — is something blocking the loop? (`clinic.js`, `--inspect`)
2. **Profile CPU** — use `clinic flame` or `0x` to find hot functions
3. **Check DB queries** — slow queries, missing indexes, N+1 problems
4. **Memory** — `process.memoryUsage()`, heap snapshots for leaks
5. **External calls** — are third-party APIs slow? Add timeouts
6. **Concurrency** — are you accidentally doing sequential awaits?

### Code
```javascript
// Detect event loop blocking
const { monitorEventLoopDelay } = require("perf_hooks");
const histogram = monitorEventLoopDelay({ resolution: 20 });
histogram.enable();

setInterval(() => {
  const lagMs = histogram.mean / 1e6;
  if (lagMs > 50) {
    console.warn(`High event loop lag: ${lagMs.toFixed(2)}ms`);
  }
}, 5000);

// Add timeouts to all external calls
const fetchWithTimeout = (url, ms = 5000) => {
  const controller = new AbortController();
  const timeout = setTimeout(() => controller.abort(), ms);
  return fetch(url, { signal: controller.signal })
    .finally(() => clearTimeout(timeout));
};
```

---

## Q35–Q38. Quick Practical Scenarios

### Q35. How do you serve static files in Express?
```javascript
app.use("/static", express.static(path.join(__dirname, "public")));
app.use(express.static("public", {
  maxAge: "1d",       // Cache-Control header
  etag: true,
  index: "index.html"
}));
```

### Q36. How do you implement request ID tracking for logs?
```javascript
const { v4: uuidv4 } = require("uuid");
app.use((req, res, next) => {
  req.id = req.headers["x-request-id"] || uuidv4();
  res.setHeader("x-request-id", req.id);
  next();
});
```

### Q37. How do you handle 404 in Express?
```javascript
// After all routes — catch unmatched routes
app.use((req, res) => {
  res.status(404).json({ error: `Route ${req.method} ${req.url} not found` });
});
```

### Q38. How do you parse query strings in Express?
```javascript
// Built-in — req.query is auto-parsed
app.get("/search", (req, res) => {
  const { q, page = 1, limit = 10, sort = "desc" } = req.query;
  // GET /search?q=node&page=2&sort=asc
  // req.query = { q: "node", page: "2", sort: "asc" }
  const pageNum = parseInt(page);
  const limitNum = parseInt(limit);
  // query DB with sanitized values
});
```

---

## Q39. How do you implement pagination in a REST API?
**Difficulty:** Practical | **Type:** Coding

### Answer

### Code
```javascript
app.get("/api/products", async (req, res) => {
  const page = Math.max(1, parseInt(req.query.page) || 1);
  const limit = Math.min(100, parseInt(req.query.limit) || 20);
  const offset = (page - 1) * limit;
  const sortBy = ["name", "price", "created_at"].includes(req.query.sort)
    ? req.query.sort : "created_at";
  const order = req.query.order === "asc" ? "ASC" : "DESC";

  const [items, total] = await Promise.all([
    db.query(`SELECT * FROM products ORDER BY ${sortBy} ${order} LIMIT $1 OFFSET $2`,
      [limit, offset]),
    db.query("SELECT COUNT(*) FROM products")
  ]);

  res.json({
    data: items.rows,
    pagination: {
      page,
      limit,
      total: parseInt(total.rows[0].count),
      totalPages: Math.ceil(total.rows[0].count / limit),
      hasNext: page * limit < total.rows[0].count,
      hasPrev: page > 1
    }
  });
});
```

---

## Q40–Q52. Additional Questions

### Q40. What is `express-validator`? Why use it over manual validation?
**Answer:** `express-validator` provides chainable validators (`isEmail()`, `isLength()`, `isInt()`) with built-in sanitization, locale support, and custom validators. More maintainable than manual `if` checks.

### Q41. How do you prevent SQL injection in Node.js?
**Answer:** Always use parameterized queries (`$1`, `?`). Never concatenate user input into SQL strings.
```javascript
// ❌ SQL injection vulnerable
db.query(`SELECT * FROM users WHERE name = '${req.body.name}'`);
// ✅ Parameterized
db.query("SELECT * FROM users WHERE name = $1", [req.body.name]);
```

### Q42. What is the difference between `res.send()`, `res.json()`, and `res.end()`?
**Answer:**
- `res.json(data)` — sets `Content-Type: application/json`, stringifies data
- `res.send(data)` — auto-detects type (string → text/html, object → JSON)
- `res.end()` — raw Node.js method, no body processing

### Q43. How do you implement versioned API routes?
```javascript
const v1 = require("./routes/v1");
const v2 = require("./routes/v2");
app.use("/api/v1", v1);
app.use("/api/v2", v2);
```

### Q44. What is `express-async-errors`?
**Answer:** A package that monkey-patches Express to handle async errors automatically — no need for `asyncHandler` wrappers. Just `require("express-async-errors")` at the top.

### Q45. How do you test an Express API?
```javascript
// Using supertest + jest
const request = require("supertest");
const app = require("./app");

describe("GET /users/:id", () => {
  it("returns user when found", async () => {
    const res = await request(app).get("/users/1").set("Authorization", `Bearer ${token}`);
    expect(res.status).toBe(200);
    expect(res.body).toHaveProperty("id", 1);
  });
  it("returns 404 when not found", async () => {
    const res = await request(app).get("/users/99999");
    expect(res.status).toBe(404);
  });
});
```

### Q46. What is `compression` middleware?
**Answer:** `compression` gzip-compresses response bodies, reducing payload size. Significant performance win for JSON APIs and static assets.
```javascript
const compression = require("compression");
app.use(compression({ threshold: 1024 })); // compress if > 1KB
```

### Q47. How do you reload config without restarting the server?
**Answer:** Watch the config file with `fs.watchFile` and update an in-memory config object. For env vars, this isn't possible — they're fixed at process start.

### Q48. What is `morgan`?
**Answer:** `morgan` is an HTTP request logger middleware for Express. Common in development: `app.use(morgan("dev"))`. In production, use structured loggers like `pino-http`.

### Q49. How do you send email from Node.js?
```javascript
const nodemailer = require("nodemailer");
const transporter = nodemailer.createTransport({
  host: process.env.SMTP_HOST,
  port: 587,
  auth: { user: process.env.SMTP_USER, pass: process.env.SMTP_PASS }
});
await transporter.sendMail({
  from: "noreply@app.com",
  to: user.email,
  subject: "Welcome!",
  html: "<h1>Welcome to our app!</h1>"
});
```

### Q50. What is the difference between `app.use()` and `app.get()`?
**Answer:** `app.use()` matches **any HTTP method** and path prefix. `app.get()` matches only GET requests at the exact path.
```javascript
app.use("/api", router); // all methods, all paths starting with /api
app.get("/api/users", handler); // only GET /api/users
```

### Q51. How would you implement health check endpoints?
```javascript
app.get("/health", (req, res) => res.json({ status: "ok", uptime: process.uptime() }));
app.get("/ready", async (req, res) => {
  try {
    await pool.query("SELECT 1");
    res.json({ status: "ready", db: "connected" });
  } catch {
    res.status(503).json({ status: "not ready", db: "disconnected" });
  }
});
```

### Q52. How do you implement a timeout for incoming requests?
```javascript
const timeout = require("connect-timeout");
app.use(timeout("10s"));
app.use((req, res, next) => {
  if (!req.timedout) next();
});
app.use((err, req, res, next) => {
  if (req.timedout) return res.status(503).json({ error: "Request timeout" });
  next(err);
});
```

---

*End of Node.js Questions — 52 questions total*
*Next: → [API Questions](../apis/questions.md)*
