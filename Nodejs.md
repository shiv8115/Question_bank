# Node.js + Express.js Interview Questions

## 🟢 Level 1 – Easy

### 1. What is Node.js and how is it different from the browser JavaScript?
Node.js is a server-side JavaScript runtime built on Chrome's V8 engine. Key differences:
- Runs on server instead of browser
- Has access to file system and OS
- Uses CommonJS modules
- No DOM/BOM access
- Built-in server capabilities

### 2. Explain the role of the event loop in Node.js
The event loop is Node.js's mechanism for handling asynchronous operations. It:
- Processes events in a loop
- Handles callbacks and I/O operations
- Maintains non-blocking behavior
- Manages the execution queue

### 3. What is a callback function?
A function passed as an argument to another function, executed after the main function completes. Example:
```javascript
function fetchData(callback) {
    setTimeout(() => callback('Data'), 1000);
}
```

### 4. How do you create a basic server using the http module?
```javascript
const http = require('http');
const server = http.createServer((req, res) => {
    res.end('Hello World');
});
server.listen(3000);
```

### 5. What is npm and how is it used?
Node Package Manager (npm) is:
- Default package manager for Node.js
- Manages project dependencies
- Runs project scripts
- Publishes packages
- Manages versions

### 6. Difference between require() and import?
- `require()`: CommonJS, synchronous, dynamic
- `import`: ES6, asynchronous, static
- `require()` returns module.exports
- `import` returns a promise

### 7. What is the use of package.json?
Project manifest file that:
- Lists dependencies
- Defines scripts
- Contains metadata
- Manages versions
- Configures project

### 8. How do you handle asynchronous operations in Node.js?
Using:
- Callbacks
- Promises
- Async/await
- Event emitters
- Streams

### 9. What are global objects in Node.js?
Built-in objects available everywhere:
- `global`
- `process`
- `console`
- `Buffer`
- `__dirname`
- `__filename`

### 10. Explain the use of process object
Provides information and control over the current Node.js process:
- Environment variables
- Process events
- Memory usage
- Process control
- Exit codes

## 🟡 Level 2 – Intermediate

### 1. How does Node.js handle concurrent requests?
Through:
- Event-driven architecture
- Non-blocking I/O
- Event loop
- Worker threads
- Clustering

### 2. How would you implement JWT authentication?
```javascript
const jwt = require('jsonwebtoken');
const authenticateToken = (req, res, next) => {
    const token = req.headers['authorization']?.split(' ')[1];
    if (!token) return res.sendStatus(401);
    jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
        if (err) return res.sendStatus(403);
        req.user = user;
        next();
    });
};
```

### 3. Explain middleware chaining in Express
Sequence of middleware functions that:
- Execute in order
- Share req/res objects
- Use next() for control flow
- Can modify request/response
- End request-response cycle

### 4. How do you handle error logging?
```javascript
const winston = require('winston');
const logger = winston.createLogger({
    level: 'info',
    format: winston.format.json(),
    transports: [
        new winston.transports.File({ filename: 'error.log', level: 'error' }),
        new winston.transports.File({ filename: 'combined.log' })
    ]
});
```

### 5. How do you prevent callback hell?
Using:
- Promises
- Async/await
- Promise.all()
- Try/catch
- Modular code

## 🔴 Level 3 – Advanced

### 1. What are performance bottlenecks in Node.js?
Common issues:
- CPU-intensive operations
- Memory leaks
- Inefficient DB queries
- Unoptimized file ops
- Poor error handling
- Inadequate caching
- Sync operations

### 2. How does garbage collection work?
V8's garbage collection:
- Mark-and-sweep algorithm
- Generational collection
- Scavenge and mark-sweep
- Memory spaces
- Automatic management

### 3. How do you handle memory leaks?
Strategies:
- Memory profiling
- Heap monitoring
- Event listener cleanup
- Weak references
- Regular GC
- Leak detection

### 4. How to build real-time apps with WebSockets?
```javascript
const WebSocket = require('ws');
const wss = new WebSocket.Server({ server });
wss.on('connection', (ws) => {
    ws.on('message', (message) => {
        wss.clients.forEach((client) => {
            if (client !== ws && client.readyState === WebSocket.OPEN) {
                client.send(message);
            }
        });
    });
});
```

### 5. How to implement RBAC?
```javascript
const checkRole = (roles) => {
    return (req, res, next) => {
        if (!roles.includes(req.user.role)) {
            return res.status(403).json({ message: 'Forbidden' });
        }
        next();
    };
};
```

### 6. What is event-driven architecture?
Architecture based on:
- Events and handlers
- Decoupled components
- Asynchronous processing
- Observer pattern
- Message passing

### 7. How to manage logs in distributed apps?
Using:
- Centralized logging (ELK)
- Log aggregation
- Structured logging
- Log levels
- Correlation IDs
- Log rotation

### 8. How to handle database transactions?
```javascript
async function transferMoney(from, to, amount) {
    const client = await pool.connect();
    try {
        await client.query('BEGIN');
        await client.query('UPDATE accounts SET balance = balance - $1 WHERE id = $2', [amount, from]);
        await client.query('UPDATE accounts SET balance = balance + $1 WHERE id = $2', [amount, to]);
        await client.query('COMMIT');
    } catch (e) {
        await client.query('ROLLBACK');
        throw e;
    } finally {
        client.release();
    }
}
```

### 9. How to implement multi-tenancy?
Strategies:
- Database per tenant
- Schema per tenant
- Row-level security
- Tenant middleware
- Data isolation

### 10. How to throttle API endpoints?
```javascript
const rateLimit = require('express-rate-limit');
const limiter = rateLimit({
    windowMs: 15 * 60 * 1000,
    max: 100
});
app.use('/api/', limiter);
```

### 11. How to monitor errors in real-time?
```javascript
const Sentry = require('@sentry/node');
Sentry.init({ dsn: process.env.SENTRY_DSN });
app.use((err, req, res, next) => {
    Sentry.captureException(err);
    logger.error('Unhandled error:', { error: err.message });
    res.status(500).json({ error: 'Internal server error' });
});
```

### 12. How to ensure service consistency?
Using:
- Saga pattern
- Event sourcing
- Message queues
- Circuit breakers
- Retry mechanisms

### 13. When to use worker threads?
For:
- CPU-intensive tasks
- Parallel processing
- Memory sharing
- I/O operations
- Better performance

### 14. How to version APIs?
```javascript
app.use('/api/v1', versionMiddleware('v1'));
app.use('/api/v2', versionMiddleware('v2'));
app.get('/api/v1/users', v1Controller);
app.get('/api/v2/users', v2Controller);
```

### 15. How to use TypeScript?
1. Install:
```bash
npm install typescript @types/node @types/express --save-dev
```

2. Configure tsconfig.json
3. Convert .js to .ts
4. Add type definitions
5. Use interfaces

### 16. How to implement permission control?
```javascript
const checkPermissions = (requiredPermissions) => {
    return async (req, res, next) => {
        const userPermissions = await getUserPermissions(req.user.id);
        if (!requiredPermissions.every(p => userPermissions.includes(p))) {
            return res.status(403).json({ error: 'Insufficient permissions' });
        }
        next();
    };
};
```

### 17. app.use() vs app.all()?
- `app.use()`: Middleware, path prefix, all methods
- `app.all()`: Route handler, exact path, all methods

### 18. How to use NGINX with Node.js?
```nginx
upstream nodejs_backend {
    server 127.0.0.1:3000;
    server 127.0.0.1:3001;
}
server {
    listen 80;
    location / {
        proxy_pass http://nodejs_backend;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
    }
}
```

### 19. High availability considerations?
- Load balancing
- Health checks
- Auto-scaling
- Monitoring
- DB replication
- Caching
- Session management
- Logging
- Security
- Backup

### 20. How to handle circular dependencies?
Solutions:
- Code restructuring
- Dependency injection
- Shared modules
- Dynamic imports
- Mediator pattern 

## 🔵 HTTP API Design & Best Practices

### 1. What are RESTful API principles?
Key principles:
- Resource-based URLs
- HTTP methods (GET, POST, PUT, DELETE)
- Stateless communication
- JSON/XML responses
- HATEOAS (optional)
- Versioning
- Proper status codes

### 2. How to handle API versioning?
Common approaches:
```javascript
// URL versioning
app.use('/api/v1', v1Router);
app.use('/api/v2', v2Router);

// Header versioning
app.use((req, res, next) => {
    const version = req.headers['api-version'];
    req.apiVersion = version;
    next();
});
```

### 3. What are common HTTP status codes?
- 2xx: Success (200 OK, 201 Created, 204 No Content)
- 3xx: Redirection (301 Moved, 304 Not Modified)
- 4xx: Client Error (400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found)
- 5xx: Server Error (500 Internal Server Error, 502 Bad Gateway)

### 4. How to implement rate limiting?
```javascript
const rateLimit = require('express-rate-limit');
const limiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100, // limit each IP to 100 requests per windowMs
    message: 'Too many requests from this IP'
});
app.use('/api/', limiter);
```

### 5. How to handle CORS?
```javascript
const cors = require('cors');
app.use(cors({
    origin: ['https://yourdomain.com', 'https://api.yourdomain.com'],
    methods: ['GET', 'POST', 'PUT', 'DELETE'],
    allowedHeaders: ['Content-Type', 'Authorization']
}));
```

## 🟣 Tricky Questions & Edge Cases

### 1. What happens when you have multiple error handlers?
```javascript
app.use((err, req, res, next) => {
    console.log('First error handler');
    next(err); // Passes to next error handler
});

app.use((err, req, res, next) => {
    console.log('Second error handler');
    res.status(500).send('Error!');
});
```
Only the first error handler that sends a response will be executed.

### 2. How to handle uncaught exceptions?
```javascript
process.on('uncaughtException', (err) => {
    console.error('Uncaught Exception:', err);
    // Perform cleanup
    process.exit(1);
});

process.on('unhandledRejection', (reason, promise) => {
    console.error('Unhandled Rejection at:', promise, 'reason:', reason);
});
```

### 3. What's the difference between process.nextTick() and setImmediate()?
- `process.nextTick()`: Executes before the next event loop iteration
- `setImmediate()`: Executes at the end of the current event loop iteration
- `nextTick` has higher priority than `setImmediate`

### 4. How to handle memory leaks in event listeners?
```javascript
class EventEmitter {
    constructor() {
        this.events = new Map();
    }

    on(event, listener) {
        if (!this.events.has(event)) {
            this.events.set(event, new Set());
        }
        this.events.get(event).add(listener);
    }

    off(event, listener) {
        if (this.events.has(event)) {
            this.events.get(event).delete(listener);
        }
    }
}
```

### 5. How to implement request timeout?
```javascript
const timeout = require('connect-timeout');
app.use(timeout('5s'));
app.use((req, res, next) => {
    if (!req.timedout) next();
});
```

### 6. What's the difference between res.send() and res.json()?
- `res.send()`: Automatically sets Content-Type based on data type
- `res.json()`: Always sets Content-Type to application/json
- `res.json()` is essentially `res.send()` with JSON.stringify()

### 7. How to handle file uploads with size limits?
```javascript
const multer = require('multer');
const upload = multer({
    limits: {
        fileSize: 5 * 1024 * 1024 // 5MB
    },
    fileFilter: (req, file, cb) => {
        if (file.mimetype.startsWith('image/')) {
            cb(null, true);
        } else {
            cb(new Error('Not an image!'));
        }
    }
});
```

### 8. How to implement request validation?
```javascript
const { body, validationResult } = require('express-validator');
app.post('/user', [
    body('email').isEmail(),
    body('password').isLength({ min: 6 }),
    (req, res) => {
        const errors = validationResult(req);
        if (!errors.isEmpty()) {
            return res.status(400).json({ errors: errors.array() });
        }
        // Process valid request
    }
]);
```

### 9. How to handle database connection pooling?
```javascript
const pool = new Pool({
    max: 20,
    idleTimeoutMillis: 30000,
    connectionTimeoutMillis: 2000,
});
```

### 10. How to implement request queuing?
```javascript
const queue = require('express-queue');
app.use(queue({
    activeLimit: 2,
    queuedLimit: 100,
    rejectHandler: (req, res) => {
        res.status(429).send('Too many requests');
    }
}));
```

### 11. How to handle graceful shutdown?
```javascript
const server = app.listen(3000);
process.on('SIGTERM', () => {
    console.log('SIGTERM received. Shutting down gracefully');
    server.close(() => {
        console.log('Process terminated');
    });
});
```

### 12. How to implement request caching?
```javascript
const mcache = require('memory-cache');
const cache = (duration) => {
    return (req, res, next) => {
        const key = '__express__' + req.originalUrl;
        const cachedBody = mcache.get(key);
        if (cachedBody) {
            res.send(cachedBody);
            return;
        }
        res.sendResponse = res.send;
        res.send = (body) => {
            mcache.put(key, body, duration * 1000);
            res.sendResponse(body);
        };
        next();
    };
};
```

### 13. How to handle circular JSON?
```javascript
const circularJSON = require('circular-json');
const obj = { a: 1 };
obj.self = obj;
const safeJSON = circularJSON.stringify(obj);
```

### 14. How to implement request retry logic?
```javascript
const axios = require('axios');
const axiosRetry = require('axios-retry');

axiosRetry(axios, { 
    retries: 3,
    retryDelay: (retryCount) => {
        return retryCount * 1000;
    },
    retryCondition: (error) => {
        return axiosRetry.isNetworkOrIdempotentRequestError(error);
    }
});
```

### 15. How to handle API deprecation?
```javascript
app.use('/api/v1', (req, res, next) => {
    res.set('Deprecation', 'true');
    res.set('Sunset', 'Wed, 21 Oct 2023 07:28:00 GMT');
    next();
});
``` 
