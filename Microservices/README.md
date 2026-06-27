# Microservices Containerization with Docker & Docker Compose

A microservices-based Node.js application containerized using Docker and orchestrated with Docker Compose. The project runs four independent services on a shared bridge network, with a gateway that routes to the others.

## Services

| Service          | Port | Key Endpoint                          |
|------------------|------|---------------------------------------|
| User Service     | 3000 | http://localhost:3000/users           |
| Product Service  | 3001 | http://localhost:3001/products        |
| Order Service    | 3002 | http://localhost:3002/orders          |
| Gateway Service  | 3003 | http://localhost:3003/api/...         |

The Gateway exposes the other services under `/api`:
- http://localhost:3003/api/users
- http://localhost:3003/api/products
- http://localhost:3003/api/orders

## Project Structure

```
.
├── user-service/
│   └── Dockerfile
├── product-service/
│   └── Dockerfile
├── order-service/
│   └── Dockerfile
├── gateway-service/
│   └── Dockerfile
├── docker-compose.yml
└── README.md
```

## Prerequisites

- Docker (20.10+)
- Docker Compose (v2+)

```bash
docker --version
docker compose version
```

![Docker version](screenshots/01-docker-version.png)
*Figure 1: Verifying Docker and Docker Compose are installed.*

## Setup Instructions

1. Fork and clone the repository:

   ```bash
   git clone <your-forked-repo-url>
   cd <repo-folder>
   ```

   ![git clone](screenshots/02-git-clone.png)
   *Figure 2: Cloning the repository.*

2. Build and start all services:

   ```bash
   docker-compose up --build
   ```

3. Confirm the four containers are running:

   ```bash
   docker ps
   ```

## Configuration

The `docker-compose.yml` defines all four services, maps their ports, and connects
them on a shared bridge network (`microservices-net`) so they can communicate by
service name.

![docker-compose.yml](screenshots/03-docker-compose-file.png)
*Figure 3: The docker-compose.yml configuration.*

## Build & Run

Running `docker-compose up --build` builds an image for each service from its
Dockerfile and starts all four containers together.

![docker-compose up](screenshots/04-docker-compose-up.png)
*Figure 4: `docker-compose up --build` building and starting all four services.*

![docker ps](screenshots/05-docker-ps.png)
*Figure 5: `docker ps` confirming all four containers are up with correct port mappings.*

## How to Test Each Service

Direct service access:

```bash
curl http://localhost:3000/users
curl http://localhost:3001/products
curl http://localhost:3002/orders
```

Through the Gateway:

```bash
curl http://localhost:3003/api/users
curl http://localhost:3003/api/products
curl http://localhost:3003/api/orders
```

Or open any of these URLs in a browser.

![User Service](screenshots/06-user-service-3000.png)
*Figure 6: User Service at http://localhost:3000/users returning the list of users in JSON.*

![Product Service](screenshots/07-product-service-3001.png)
*Figure 7: Product Service at http://localhost:3001/products returning the list of products.*

![Order Service](screenshots/08-order-service-3002.png)
*Figure 8: Order Service at http://localhost:3002/orders. The service is running and reachable.*

![Gateway Service](screenshots/09-gateway-service-3003.png)
*Figure 9: Gateway Service at http://localhost:3003/api/users routing through to the User Service over the shared network, confirming inter-service communication.*

## Stopping the Application

```bash
docker-compose down
```
