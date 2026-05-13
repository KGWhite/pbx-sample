# pbx-sample

## Directory Structure
```
/pbx-sample
├── hal                         # Hardware Abstraction Layer
│   └── os                      # OS-level scripts and drivers (e.g., general and rocky)
├── services                    # Software container layer (hardware-independent)
│   ├── docker-compose.yml      # Centralized configuration for microservices
│   ├── api                     # Golang + Gin API service
│   ├── db                      # PostgreSQL database service
│   ├── pbx                     # Asterisk PBX service and build configuration
│   └── web                     # React frontend and Nginx reverse proxy
└── tests                       # Test files directory
```

## Service Management

Currently, there are four main Docker containers configured under the `services` directory:
1. **api**: Backend API built with Golang and the Gin framework.
2. **db**: PostgreSQL database.
3. **pbx**: Asterisk PBX service compiled from `source/asterisk`.
4. **web**: Frontend service built with React and deployed using Nginx.

You can manage these four containers uniformly using `services/docker-compose.yml`:

```bash
cd services
docker-compose up -d --build
```