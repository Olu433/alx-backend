# Queuing System in JavaScript with Redis

This project demonstrates a queuing system using Node.js, Redis, and Kue. It includes various tasks such as setting up Redis, creating a Redis client, performing basic and advanced Redis operations, implementing a publisher-subscriber model, and managing job queues with Kue.

## Table of Contents

- [Project Overview](#project-overview)
- [Prerequisites](#prerequisites)
- [Setup Instructions](#setup-instructions)
- [File Structure](#file-structure)
- [Tasks](#tasks)
  - [Task 0: Install and Start Redis](#task-0-install-and-start-redis)
  - [Task 1: Node Redis Client](#task-1-node-redis-client)
  - [Task 2: Node Redis Client and Basic Operations](#task-2-node-redis-client-and-basic-operations)
  - [Task 3: Node Redis Client and Async Operations](#task-3-node-redis-client-and-async-operations)
  - [Task 4: Node Redis Client and Advanced Operations](#task-4-node-redis-client-and-advanced-operations)
  - [Task 5: Node Redis Client Publisher and Subscriber](#task-5-node-redis-client-publisher-and-subscriber)
  - [Task 6: Create the Job Creator](#task-6-create-the-job-creator)
  - [Task 7: Create the Job Processor](#task-7-create-the-job-processor)
  - [Task 8: Track Progress and Errors with Kue: Create the Job Creator](#task-8-track-progress-and-errors-with-kue-create-the-job-creator)
  - [Task 9: Track Progress and Errors with Kue: Create the Job Processor](#task-9-track-progress-and-errors-with-kue-create-the-job-processor)
  - [Task 10: Writing the Job Creation Function](#task-10-writing-the-job-creation-function)
  - [Task 11: Writing the Test for Job Creation](#task-11-writing-the-test-for-job-creation)
  - [Task 12: In Stock?](#task-12-in-stock)
- [Running the Application](#running-the-application)
- [Testing](#testing)
- [Contributing](#contributing)
- [License](#license)

## Project Overview

This project is designed to demonstrate the use of Redis and Kue for building a robust queuing system in a Node.js environment. It includes tasks for setting up Redis, performing basic and advanced Redis operations, implementing a publisher-subscriber model, and managing job queues with Kue.

## Prerequisites

- Node.js (v14 or higher)
- Redis (v6.0.10 or higher)
- npm (Node Package Manager)

## Setup Instructions

### 1. Install Node.js and npm

Ensure you have Node.js and npm installed. You can download them from the official Node.js website: https://nodejs.org/

### 2. Install Redis

1. Download, extract, and compile the latest stable Redis version:
   ```sh
   wget http://download.redis.io/releases/redis-6.0.10.tar.gz
   tar xzf redis-6.0.10.tar.gz
   cd redis-6.0.10
   make
   ```

2. Start Redis in the background:
   ```sh
   src/redis-server &
   ```

3. Verify Redis is running:
   ```sh
   src/redis-cli ping
   PONG
   ```

### 3. Clone the Repository

Clone this repository to your local machine:
```sh
git clone https://github.com/olu433/0x03-queuing_system_in_js.git
cd queuing_system_in_js
```

### 4. Install Dependencies

Install the necessary Node.js dependencies:
```sh
npm install
```

## File Structure

```
queuing_system_in_js/
├── 0-redis_client.js
├── 1-redis_op.js
├── 2-redis_op_async.js
├── 4-redis_advanced_op.js
├── 5-publisher.js
├── 5-subscriber.js
├── 6-job_creator.js
├── 6-job_processor.js
├── 7-job_creator.js
├── 7-job_processor.js
├── 8-job.js
├── 8-job-main.js
├── 8-job.test.js
├── 9-stock.js
├── package.json
├── package-lock.json
└── README.md
```

## Tasks

### Task 0: Install and Start Redis

1. **Download, Extract, and Compile Redis:**
   ```sh
   wget http://download.redis.io/releases/redis-6.0.10.tar.gz
   tar xzf redis-6.0.10.tar.gz
   cd redis-6.0.10
   make
   ```

2. **Start Redis in the Background:**
   ```sh
   src/redis-server &
   ```

3. **Verify Redis is Running:**
   ```sh
   src/redis-cli ping
   PONG
   ```

4. **Set and Get a Key:**
   ```sh
   127.0.0.1:[Port]> set Holberton School
   OK
   127.0.0.1:[Port]> get Holberton
   "School"
   ```

5. **Kill the Redis Server:**
   ```sh
   kill [PID_OF_Redis_Server]
   ```

6. **Copy the `dump.rdb` File:**
   ```sh
   cp redis-6.0.10/dump.rdb /path/to/queuing/project/
   ```

### Task 1: Node Redis Client

1. **Install `node_redis` and Babel:**
   ```sh
   npm install redis @babel/core @babel/preset-env nodemon
   ```

2. **Create `0-redis_client.js`:**
   ```javascript
   import redis from 'redis';
   import { promisify } from 'util';

   const client = redis.createClient();

   client.on('connect', () => {
     console.log('Redis client connected to the server');
   });

   client.on('error', (error) => {
     console.log(`Redis client not connected to the server: ${error}`);
   });

   client.quit();
   ```

### Task 2: Node Redis Client and Basic Operations

1. **Create `1-redis_op.js`:**
   ```javascript
   import redis from 'redis';
   import { promisify } from 'util';

   const client = redis.createClient();

   client.on('connect', () => {
     console.log('Redis client connected to the server');
   });

   client.on('error', (error) => {
     console.log(`Redis client not connected to the server: ${error}`);
   });

   const setNewSchool = (schoolName, value) => {
     client.set(schoolName, value, (err, reply) => {
       if (err) {
         console.log(`Error setting key: ${err}`);
       } else {
         console.log(`Reply: ${reply}`);
       }
     });
   };

   const displaySchoolValue = (schoolName) => {
     client.get(schoolName, (err, reply) => {
       if (err) {
         console.log(`Error getting key: ${err}`);
       } else {
         console.log(reply);
       }
     });
   };

   displaySchoolValue('Holberton');
   setNewSchool('HolbertonSanFrancisco', '100');
   displaySchoolValue('HolbertonSanFrancisco');
   ```

### Task 3: Node Redis Client and Async Operations

1. **Create `2-redis_op_async.js`:**
   ```javascript
   import redis from 'redis';
   import { promisify } from 'util';

   const client = redis.createClient();
   const getAsync = promisify(client.get).bind(client);
   const setAsync = promisify(client.set).bind(client);

   client.on('connect', () => {
     console.log('Redis client connected to the server');
   });

   client.on('error', (error) => {
     console.log(`Redis client not connected to the server: ${error}`);
   });

   const displaySchoolValue = async (schoolName) => {
     try {
       const value = await getAsync(schoolName);
       console.log(value);
     } catch (err) {
       console.log(`Error getting key: ${err}`);
     }
   };

   const setNewSchool = async (schoolName, value) => {
     try {
       const reply = await setAsync(schoolName, value);
       console.log(`Reply: ${reply}`);
     } catch (err) {
       console.log(`Error setting key: ${err}`);
     }
   };

   displaySchoolValue('Holberton');
   setNewSchool('HolbertonSanFrancisco', '100');
   displaySchoolValue('HolbertonSanFrancisco');
   ```

### Task 4: Node Redis Client and Advanced Operations

1. **Create `4-redis_advanced_op.js`:**
   ```javascript
   import redis from 'redis';
   import { promisify } from 'util';

   const client = redis.createClient();
   const hsetAsync = promisify(client.hset).bind(client);
   const hgetallAsync = promisify(client.hgetall).bind(client);

   client.on('connect', () => {
     console.log('Redis client connected to the server');
   });

   client.on('error', (error) => {
     console.log(`Redis client not connected to the server: ${error}`);
   });

   const createHash = async () => {
     const schools = {
       Portland: '50',
       Seattle: '80',
       'New York': '20',
       Bogota: '20',
       Cali: '40',
       Paris: '2'
     };

     for (const [key, value] of Object.entries(schools)) {
       try {
         const reply = await hsetAsync('HolbertonSchools', key, value);
         console.log(`Reply: ${reply}`);
       } catch (err) {
         console.log(`Error setting hash: ${err}`);
       }
     }
   };

   const displayHash = async () => {
     try {
       const hash = await hgetallAsync('HolbertonSchools');
       console.log(hash);
     } catch (err) {
       console.log(`Error getting hash: ${err}`);
     }
   };

   createHash();
   displayHash();
   ```

### Task 5: Node Redis Client Publisher and Subscriber

1. **Create `5-subscriber.js`:**
   ```javascript
   import redis from 'redis';

   const client = redis.createClient();

   client.on('connect', () => {
     console.log('Redis client connected to the server');
   });

   client.on('error', (error) => {
     console.log(`Redis client not connected to the server: ${error}`);
   });

   client.subscribe('holberton school channel', (err, count) => {
     if (err) {
       console.log(`Error subscribing: ${err}`);
     }
   });

   client.on('message', (channel, message) => {
     console.log(message);
     if (message === 'KILL_SERVER') {
       client.unsubscribe();
       client.quit();
     }
   });
   ```

2. **Create `5-publisher.js`:**
   ```javascript
   import redis from 'redis';

   const client = redis.createClient();

   client.on('connect', () => {
     console.log('Redis client connected to the server');
   });

   client.on('error', (error) => {
     console.log(`Redis client not connected to the server: ${error}`);
   });

   const publishMessage = (message, time) => {
     setTimeout(() => {
       console.log(`About to send ${message}`);
       client.publish('holberton school channel', message);
     }, time);
   };

   publishMessage("Holberton Student #1 starts course", 100);
   publishMessage("Holberton Student #2 starts course", 200);
   publishMessage("KILL_SERVER", 300);
   publishMessage("Holberton Student #3 starts course", 400);
   ```

### Task 6: Create the Job Creator

1. **Create `6-job_creator.js`:**
   ```javascript
   import kue from 'kue';

   const queue = kue.createQueue();

   const createJob = (phoneNumber, message) => {
     const job = queue.create('push_notification_code', {
       phoneNumber,
       message
     }).save((err) => {
       if (err) {
         console.log(`Error creating job: ${err}`);
       } else {
         console.log(`Notification job created: ${job.id}`);
       }
     });

     job.on('complete', () => {
       console.log(`Notification job ${job.id} completed`);
     });

     job.on('failed', (err) => {
       console.log(`Notification job ${job.id} failed: ${err}`);
     });

     job.on('progress', (progress) => {
       console.log(`Notification job ${job.id} ${progress}% complete`);
     });
   };

   createJob('4153518780', 'This is the code to verify your account');
   ```

### Task 7: Create the Job Processor

1. **Create `6-job_processor.js`:**
   ```javascript
   import kue from 'kue';

   const queue = kue.createQueue();

   const sendNotification = (phoneNumber, message, job, done) => {
     console.log(`Sending notification to ${phoneNumber}, with message: ${message}`);
     done();
   };

   queue.process('push_notification_code', (job, done) => {
     const { phoneNumber, message } = job.data;
     sendNotification(phoneNumber, message, job, done);
   });
   ```

### Task 8: Track Progress and Errors with Kue: Create the Job Creator

1. **Create `7-job_creator.js`:**
   ```javascript
   import kue from 'kue';

   const jobs = [
     { phoneNumber: '4153518780', message: 'This is the code 1234 to verify your account' },
     { phoneNumber: '4153518781', message: 'This is the code 4562 to verify your account' },
     // Add more jobs as needed
   ];

   const queue = kue.createQueue();

   const createJobs = (jobs) => {
     jobs.forEach((jobData) => {
       const job = queue.create('push_notification_code_2', jobData).save((err) => {
         if (err) {
           console.log(`Error creating job: ${err}`);
         } else {
           console.log(`Notification job created: ${job.id}`);
         }
       });

       job.on('complete', () => {
         console.log(`Notification job ${job.id} completed`);
       });

       job.on('failed', (err) => {
         console.log(`Notification job ${job.id} failed: ${err}`);
       });

       job.on('progress', (progress) => {
         console.log(`Notification job ${job.id} ${progress}% complete`);
       });
     });
   };

   createJobs(jobs);
   ```

### Task 9: Track Progress and Errors with Kue: Create the Job Processor

1. **Create `7-job_processor.js`:**
   ```javascript
   import kue from 'kue';

   const queue = kue.createQueue();
   const blacklistedPhoneNumbers = ['4153518780', '4153518781'];

   const sendNotification = (phoneNumber, message, job, done) => {
     job.progress(0, 100);
     if (blacklistedPhoneNumbers.includes(phoneNumber)) {
       job.progress(100, 100);
       done(new Error(`Phone number ${phoneNumber} is blacklisted`));
     } else {
       job.progress(50, 100);
       console.log(`Sending notification to ${phoneNumber}, with message: ${message}`);
       job.progress(100, 100);
       done();
     }
   };

   queue.process('push_notification_code_2', 2, (job, done) => {
     const { phoneNumber, message } = job.data;
     sendNotification(phoneNumber, message, job, done);
   });
   ```

### Task 10: Writing the Job Creation Function

1. **Create `8-job.js`:**
   ```javascript
   import kue from 'kue';

   const createPushNotificationsJobs = (jobs, queue) => {
     if (!Array.isArray(jobs)) {
       throw new Error('Jobs is not an array');
     }

     jobs.forEach((jobData) => {
       const job = queue.create('push_notification_code_3', jobData).save((err) => {
         if (err) {
           console.log(`Error creating job: ${err}`);
         } else {
           console.log(`Notification job created: ${job.id}`);
         }
       });

       job.on('complete', () => {
         console.log(`Notification job ${job.id} completed`);
       });

       job.on('failed', (err) => {
         console.log(`Notification job ${job.id} failed: ${err}`);
       });

       job.on('progress', (progress) => {
         console.log(`Notification job ${job.id} ${progress}% complete`);
       });
     });
   };

   export default createPushNotificationsJobs;
   ```

### Task 11: Writing the Test for Job Creation

1. **Create `8-job.test.js`:**
   ```javascript
   import kue from 'kue';
   import createPushNotificationsJobs from './8-job.js';

   describe('createPushNotificationsJobs', () => {
     let queue;

     beforeEach(() => {
       queue = kue.createQueue();
       queue.testMode.enter();
     });

     afterEach(() => {
       queue.testMode.clear();
       queue.testMode.exit();
     });

     it('display a error message if jobs is not an array', () => {
       expect(() => {
         createPushNotificationsJobs('not an array', queue);
       }).toThrow('Jobs is not an array');
     });

     it('create two new jobs to the queue', async () => {
       const jobs = [
         { phoneNumber: '4153518780', message: 'This is the code 1234 to verify your account' },
         { phoneNumber: '4153518781', message: 'This is the code 4562 to verify your account' }
       ];

       createPushNotificationsJobs(jobs, queue);

       const job1 = await queue.testMode.getJob(1);
       const job2 = await queue.testMode.getJob(2);

       expect(job1.data).toEqual(jobs[0]);
       expect(job2.data).toEqual(jobs[1]);
     });
   });
   ```

### Task 12: In Stock?

1. **Create `9-stock.js`:**
   ```javascript
   import express from 'express';
   import redis from 'redis';
   import { promisify } from 'util';

   const app = express();
   const client = redis.createClient();
   const getAsync = promisify(client.get).bind(client);
   const setAsync = promisify(client.set).bind(client);

   const listProducts = [
     { itemId: 1, itemName: 'Suitcase 250', price: 50, initialAvailableQuantity: 4 },
     { itemId: 2, itemName: 'Suitcase 450', price: 100, initialAvailableQuantity: 10 },
     { itemId: 3, itemName: 'Suitcase 650', price: 350, initialAvailableQuantity: 2 },
     { itemId: 4, itemName: 'Suitcase 1050', price: 550, initialAvailableQuantity: 5 }
   ];

   const getItemById = (id) => {
     return listProducts.find(item => item.itemId === id);
   };

   const reserveStockById = async (itemId, stock) => {
     await setAsync(`item.${itemId}`, stock);
   };

   const getCurrentReservedStockById = async (itemId) => {
     const stock = await getAsync(`item.${itemId}`);
     return stock ? parseInt(stock, 10) : 0;
   };

   app.get('/list_products', async (req, res) => {
     res.json(listProducts.map(item => ({
       itemId: item.itemId,
       itemName: item.itemName,
       price: item.price,
       initialAvailableQuantity: item.initialAvailableQuantity
     })));
   });

   app.get('/list_products/:itemId', async (req, res) => {
     const itemId = parseInt(req.params.itemId, 10);
     const item = getItemById(itemId);

     if (!item) {
       return res.json({ status: 'Product not found' });
     }

     const currentStock = await getCurrentReservedStockById(itemId);
     res.json({ ...item, currentQuantity: item.initialAvailableQuantity - currentStock });
   });

   app.get('/reserve_product/:itemId', async (req, res) => {
     const itemId = parseInt(req.params.itemId, 10);
     const item = getItemById(itemId);

     if (!item) {
       return res.json({ status: 'Product not found' });
     }

     const currentStock = await getCurrentReservedStockById(itemId);

     if (currentStock >= item.initialAvailableQuantity) {
       return res.json({ status: 'Not enough stock available', itemId });
     }

     await reserveStockById(itemId, currentStock + 1);
     res.json({ status: 'Reservation confirmed', itemId });
   });

   const PORT = 1245;
   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

## Running the Application

1. **Start Redis:**
   ```sh
   src/redis-server &
   ```

2. **Start the Express Server:**
   ```sh
   npm run dev 9-stock.js
   ```

3. **Test the API Endpoints:**
   - List all products:
     ```sh
     curl localhost:1245/list_products
     ```

   - Get product details by ID:
     ```sh
     curl localhost:1245/list_products/1
     ```

   - Reserve a product:
     ```sh
     curl localhost:1245/reserve_product/1
     ```

## Testing

1. **Run the Tests:**
   ```sh
   npm test 8-job.test.js
   ```

## Contributing

Contributions are welcome! Please follow these steps to contribute:

1. Fork the repository.
2. Create a new branch: `git checkout -b feature/your-feature`.
3. Make your changes and commit them: `git commit -m 'Add your feature'`.
4. Push to the branch: `git push origin feature/your-feature`.
5. Open a pull request.
