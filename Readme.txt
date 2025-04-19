In this project i will dockerise an application (a simple app) with three main building blocks:
A database  (mongo db for application data), 
backend (NodeJS REST API) 
Frontend (Built with React SPA)

* All containerised 

Project Goals:
1. Data in the database need to be persistent: handled using named volume -v tag at runtime
    ( $docker run -d --name mongo-database -v goalsdata:/data/db --network goals-network --rm mongo)

2. Data (the log file data) in the backend server also need to be persistent: handled using named volume
    docker run -d --rm --network goals-network --name backend-app -v logs:/app/logs  -p 80:80 goals-backend-img

3. Access to the database is limited: handled using environment variable while running the mongo-database
        ( $docker run -d --name mongo-database -v goalsdata:/data/db -e MONGOdb_USERNAME=********** -e MONGO_PASSWORD=************ --network goals-network --rm mongo)

4. Live source code changes should reflect without rebuilding image:  handled using bind-mounts.
    note: Longer container internal paths have precedence and overwrite shorter paths.

    In this project logs written by the container will not be overittwn by the bind-mount attached to it because the container path /app/logs take 
    precedence over bindmount of both backend and frontend container.

    frontend: $docker run -d -v local-path-to-sourcecode-file:/app/src --network goals-network --name frontend -p 3000:3000 goals-frontend-img
    backend: $docker run -d -v local-path-to-sourcecode-file:/app -v logs:/app/logs -v /app/node_modules --network goals-network --name frontend -p 80:80 goals-backend-img

    Also an anonymous volume was created for the path app/node_modules (-v /app/node_modules) on both container to avoid overwriting the app dependencies.

    Finally: To make changes in the code to be implemented I added an extra dependency (nodemon) to help restart the node server everytime changes are made.  


NOTE: ofcourse the live-source-code changes is for testing in dev environment. 