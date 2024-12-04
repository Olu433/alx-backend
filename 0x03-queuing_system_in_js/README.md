# Queuing System in Node.js with Redis and Kue

This project demonstrates how to set up a Redis-based queuing system using Node.js and Kue. It includes basic Redis operations, job creation, and job processing. Additionally, it features an inventory management system using Express and Redis.

## Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
  - [Redis Operations](#redis-operations)
  - [Job Creation and Processing](#job-creation-and-processing)
  - [Inventory Management](#inventory-management)
- [Testing](#testing)
- [Contributing](#contributing)
- [License](#license)

## Introduction

This project is designed to help you understand how to use Redis for basic key-value operations, job queuing, and inventory management. It includes several tasks that guide you through setting up Redis, creating and processing jobs with Kue, and building a simple inventory management system using Express and Redis.

## Prerequisites

Before you begin, ensure you have the following installed on your machine:

- Node.js (v14 or higher)
- npm (v6 or higher)
- Redis (v6.0.10 or higher)

## Installation

1. **Clone the repository:**
   ```sh
   git clone https://github.com/your-username/queuing-system-in-js.git
   cd queuing-system-in-js
   ```

2. **Install dependencies:**
   ```sh
   npm install
   ```

3. **Download and compile Redis:**
   ```sh
   wget http://download.redis.io/releases/redis-6.0.10.tar.gz
   tar xzf redis-6.0.10.tar.gz
   cd redis-6.0.10
   make
   ```

4. **Start Redis in the background:**
   ```sh
   src/redis-server &
   ```

5. **Verify Redis is running:**
   ```sh
   src/redis-cli ping
   ```

## Usage

### Redis Operations

1. **Basic Redis Client:**
   - Run the basic Redis client to connect to the Redis server and perform simple operations.
   ```sh
   npm run dev 0-redis_client.js
   ```

2. **Basic Redis Operations:**
   - Run the script to set and get keys in Redis.
   ```sh
   npm run dev 1-redis_op.js
   ```

3. **Async Redis Operations:**
   - Run the script to perform Redis operations using async/await.
   ```sh
   npm run dev 2-redis_op_async.js
   ```

4. **Advanced Redis Operations:**
   - Run the script to create and retrieve hash values in Redis.
   ```sh
   npm run dev 4-redis_advanced_op.js
   ```

### Job Creation and Processing

1. **Redis Publisher and Subscriber:**
   - Run the subscriber script in one terminal.
   ```sh
   npm run dev 5-subscriber.js
   ```
   - Run the publisher script in another terminal.
   ```sh
   npm run dev 5-publisher.js
   ```

2. **Job Creator:**
   - Run the job creator script to create jobs in the queue.
   ```sh
   npm run dev 6-job_creator.js
   ```

3. **Job Processor:**
   - Run the job processor script to process jobs in the queue.
   ```sh
   npm run dev 6-job_processor.js
   ```

4. **Track Progress and Errors:**
   - Run the job creator script to create multiple jobs with progress tracking.
   ```sh
   npm run dev 7-job_creator.js
   ```
   - Run the job processor script to process these jobs.
   ```sh
   npm run dev 7-job_processor.js
   ```

### Inventory Management

1. **Start the Express Server:**
   - Run the inventory management server.
   ```sh
   npm run dev 9-stock.js
   ```

2. **List Products:**
   - Use `curl` to list all available products.
   ```sh
   curl localhost:1245/list_products
   ```

3. **Get Product Details:**
   - Use `curl` to get details of a specific product.
   ```sh
   curl localhost:1245/list_products/1
   ```

4. **Reserve a Product:**
   - Use `curl` to reserve a product.
   ```sh
   curl localhost:1245/reserve_product/1
   ```

## Testing

1. **Run the test suite:**
   ```sh
   npm test 8-job.test.js
   ```

## Contributing

Contributions are welcome! Please follow these steps to contribute:

1. Fork the repository.
2. Create a new branch: `git checkout -b feature/your-feature`.
3. Make your changes and commit them: `git commit -m 'Add some feature'`.
4. Push to the branch: `git push origin feature/your-feature`.
5. Submit a pull request.
