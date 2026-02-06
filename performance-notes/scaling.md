# Performance & Scaling Strategy

This document outlines the strategies used to ensure the application remains performant under high load and scales effectively as the user base grows.

## 1. Database Optimization (Prisma & SQL)
The database is often the first bottleneck. We utilize the following strategies to mitigate this:

### A. Indexing
Indexes are applied to columns that are frequently queried, filtered, or used for joining tables.
* **Primary Keys:** Automatically indexed (`id`).
* **Unique Constraints:** Automatically indexed (`email`).
* **Foreign Keys:** Must be manually indexed to speed up relation lookups (e.g., `authorId` on Posts).
* **Search Fields:** specific indexes on fields like `status` or `role` if frequently filtered.

### B. Connection Pooling
Since Prisma opens a connection for every client instance, we use **PgBouncer** (or a cloud equivalent like AWS RDS Proxy) to manage connection pooling.
* **Goal:** Prevent exhausting the database `max_connections` limit during traffic spikes (especially in Serverless environments).
* **Config:** Set pool size to match the database's comfortable handling capacity (e.g., 20-50 active connections).

### C. Query Optimization
* **Avoid N+1 Problems:** Use Prisma's `include` or `select` to fetch related data in a single query rather than iterating in a loop.
* **Pagination:** strict pagination (Cursor-based preferred over Offset-based) is enforced on all list endpoints (e.g., `GET /posts`) to prevent fetching thousands of rows at once.
* **Field Selection:** Only select necessary fields (`select: { id: true, name: true }`) rather than fetching the entire object.

---

## 2. Caching Strategy
We implement a multi-layer caching strategy to reduce load on the database.

### A. Application Cache (Redis)
* **Session/Auth:** Store active User sessions or JWT blacklists in Redis for instant access ( < 5ms).
* **Permission Checks:** Cache the "User Role" lookups. Since roles rarely change, we don't need to query the DB for permissions on every request.
* **Expensive Queries:** Cache results of heavy aggregation queries (e.g., "Top 10 posts") with a TTL (Time To Live) of 5-10 minutes.

### B. Content Delivery Network (CDN)
* Static assets (images, CSS, JS) are offloaded to a CDN (e.g., Cloudflare, AWS CloudFront).
* This reduces latency for global users and removes load from the main application server.

---

## 3. Horizontal Scaling (Application Layer)
The backend is designed to be **Stateless**.

* **Statelessness:** No session data is stored in the server's local memory or file system. All state is strictly in the Database or Redis.
* **Load Balancing:** We can spin up multiple instances (replicas) of the API server behind a Load Balancer (e.g., Nginx, AWS ALB). The Load Balancer distributes incoming traffic across these instances.

---

## 4. Read/Write Splitting (Future Growth)
As the application grows significantly:
* **Write Operations (INSERT, UPDATE, DELETE):** Directed to the Primary Database node.
* **Read Operations (SELECT):** Directed to Read Replicas.
* **Implementation:** Prisma allows configuring separate URLs for read and write operations to distribute the load automatically.

---

## 5. Monitoring & Alerts
To ensure performance standards are met, we track:
* **p95 / p99 Latency:** The response time for the slowest 5% and 1% of requests (Target: < 200ms).
* **Database CPU/Memory:** Alerts trigger if DB usage exceeds 70%.
* **Error Rates:** Spikes in 5xx errors trigger immediate developer alerts.