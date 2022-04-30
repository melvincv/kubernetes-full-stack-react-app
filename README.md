# Kubernetes Deployment Series - Full Stack React App
Deploy a Full Stack React App on Kubernetes using ReactJS + NGINX, Express server and Postgres DB

## Development 

### Server

Create a "server" folder and run

    npm init

Create package.json, keys.js and index.js

    npm run dev

Go to http://localhost:5000  to see the welcome message.

#### Create Dockerfile.dev

```
docker build -t melvincv/fs-react-server -f ./Dockerfile.dev .
docker run -it -p 4003:5000 melvincv/fs-react-server
```

### Client

Create the client folder structure

    npm run start

Go to http://localhost:3000 

#### Create Dockerfile.dev

```
docker build -t melvincv/fs-react-client -f ./Dockerfile.dev .
docker run -it -p 4002:3000 melvincv/fs-react-client
```

## Docker Compose

Create docker-compose.yml and the nginx folder inthe repo root for local dev deployment

    docker compose up --build

## Production Build

```
cd client
docker build -t melvincv/fs-react-client .

cd server
docker build -t melvincv/fs-react-server .

docker login
docker push melvincv/fs-react-client
docker push melvincv/fs-react-server
```

## Kubernetes

### Create namespace

    k create ns fs-react-app

### Create client.yml

```
Kind: Deployment
Name: client-d
Image: melvincv/fs-react-client
Port: 3000
---
Kind: Service
Name: client-svc
Type: ClusterIP
Port: 3000
TPort: 3000
```

### Create postgres.yml

```
Kind: Deployment
Name: postgres-d
Image: postgres
Port: 5432
Replicas: 1
Add Volume: 
    - Name: db-data
    - Add PVC
Add Volume Mount:
    - Name: db-data
    - Path: /var/lib/postgresql/data
    - subPath: postgres
Add env:
    - POSTGRES_PASSWORD
---
Kind: Service
Name: postgres-svc
Type: ClusterIP
Port: 5432
TPort: 5432
```

### Create db-pvc.yml

```
Kind: PersistentVolumeClaim
Name: 
AccessModes: ReadWriteOnce
Storage: 1Gi
```

### Create secrets.yml

    Name: pgpassword

### Create server.yml

```
Kind: Deployment
Name: server-d
Image: melvincv/fs-react-server
Port: 5000
Add env:
    - PGUSER
    - PGHOST
    - PGDATABASE
    - PGPORT
    - PGPASSWORD: from Secret
---
Kind: Service
Name: server-svc
Type: ClusterIP
Port: 5000
TPort: 5000
```

### Install the NGINX Ingress Controller

    kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.2.0/deploy/static/provider/cloud/deploy.yaml

### Create ingress.yml

```
Kind: Ingress
Annotations:
    - Ingress Class: nginx
    - Regex: true
    - Rewrite: /$1
```

## References

YouTube: Dockerizing a React application with Nodejs Postgres and NginX | dev and prod | step by step \
https://www.youtube.com/watch?v=-pTel5FojAQ

YouTube: Kubernetes Multi Container Deployment | React | Node.js | Postgres | Ingress Nginx | step by step \
https://www.youtube.com/watch?v=OVVGwc90guo