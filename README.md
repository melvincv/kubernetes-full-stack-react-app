# Kubernetes Deployment Series - Full Stack React App
Deploy a Full Stack React App on Kubernetes using ReactJS + NGINX, Express server and Postgres DB

## Development 

### Server

Create a "server" folder and run

npm init

Create package.json, keys.js and index.js

npm run dev

Go to http://localhost:5000  to see the welcome message.

Create Dockerfile.dev

docker build -t melvincv/fs-react-server -f ./Dockerfile.dev .
docker run -it -p 4003:5000 melvincv/fs-react-server

### Client

Create the client folder structure

npm run start

Go to http://localhost:3000 

Create Dockerfile.dev

docker build -t melvincv/fs-react-client -f ./Dockerfile.dev .
docker run -it -p 4002:3000 melvincv/fs-react-client

## Docker Compose



## References

YouTube: Dockerizing a React application with Nodejs Postgres and NginX | dev and prod | step by step
https://www.youtube.com/watch?v=-pTel5FojAQ