# containerization

This documentation will help you understand the architecture, setup, and usage of the movie streaming platform built using microservices infrastructure.

## Table of Contents

1. [Project Overview](#project-overview)
2. [Getting Started](#getting-started)
   - [Prerequisites](#prerequisites)
   - [Installation](#installation)
3. [API Documentation](#api-documentation)
   - [API Gateway](#api-gateway)
   - [Inventory API](#inventory-api)
   - [Billing API](#billing-api)
4. [Docker Compose](#docker-compose)
5. [Project Organization](#project-organization)

## 1. Project Overview <a name="project-overview"></a>

The play-with-containers project implements a movie streaming platform with six microservices: `Inventory`, `Billing`, `RabbitMQ`, `API Gateway` and 2 databases. The API Gateway manages communication between these services, using HTTP for `Inventory` and RabbitMQ for `Billing`. The project is organized into docker images to start docker containers which simulate a production environment.

## 2. Getting Started <a name="getting-started"></a>

### Prerequisites <a name="prerequisites"></a>

Make sure you have the following installed on your machine:

- Node.js 18.16
- PostgreSQL
- RabbitMQ
- Postman
- Docker
- Docker Compose

### Installation <a name="installation"></a>

1. Clone the repository:

   ```bash
   git clone https://github.com/Zewasik/play-with-containers.git
   ```

2. Navigate to the project directory:

   ```bash
   cd play-with-containers
   ```

3. Create a `.env` file in the project root and configure the necessary environment variables. You can use `.example.env` for testing.

4. Install dependencies for each service:

   ```bash
   cd srcs/api-gateway
   npm install

   cd ../inventory-app
   npm install

   cd ../billing-app
   npm install
   ```

5. Run the services locally:

   ```bash
   # In srcs/api-gateway
   npm start

   # In srcs/inventory-app
   npm start

   # In srcs/billing-app
   npm start
   ```

6. Use Postman to test the APIs. You may use Postman configuration located at `postman-config` folder.

## 3. API Documentation <a name="api-documentation"></a>

### API Gateway <a name="api-gateway"></a>

The API Gateway routes requests between the `Inventory` and `Billing` services. It uses a proxy system to forward requests to the appropriate service. API documentation is available in the OpenAPI format. Refer to [http://localhost:3000/api-docs](http://localhost:3000/api-docs) for detailed API documentation.

### Inventory API <a name="inventory-api"></a>

The `Inventory` API is a CRUD RESTful API that provides information about movies. It uses a PostgreSQL database named `movies`. Endpoints include:

- `GET /api/movies`
- `GET /api/movies?title=[name]`
- `POST /api/movies`
- `DELETE /api/movies`
- `GET /api/movies/:id`
- `PUT /api/movies/:id`
- `DELETE /api/movies/:id`

Refer to [Postman Tests](/postman-config) for API testing.

### Billing API <a name="billing-api"></a>

The `Billing` API processes messages received through RabbitMQ. It parses JSON messages and creates entries in the `orders` database. Endpoints include:

- RabbitMQ Queue: `billing_queue`

## 4. Docker Compose <a name="docker-compose"></a>

The project uses Docker Compose to set up all microservices:

- `api-gateway-app`: API Gateway.
- `inventory-app`: `Inventory` API.
- `inventory-database`: Contains the `movies` database.
- `billing-app`: `Billing` API.
- `billing-database`: Contains the `orders` database.
- `rabbitmq`: Runs RabbitMQ service.

### Environment Variables

Configure your environment variables in the `.env` file for centralized credential management.

### Configuration of images

Use the `docker-compose.yml` to create and start the services. Execute the following command from the project root:

```bash
docker compose up
```

To stop and remove containers and volumes use: 

```bash
docker compose down -v
```

## 5. Project Organization <a name="project-organization"></a>

### Overall File Structure

```console
.
├── README.md
├── .env
├── srcs
│   ├── api-gateway
│   │   ├── ...
│   ├── billing-app
│   │   ├── ...
│   └── inventory-app
│       ├── ...
├── postman-config
|   ├── ...
└── docker-compose.yml
```
