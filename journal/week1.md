# Week 1 — App Containerization

## About Docker and Containerization

### Why do we need docker
To make environments easy. We don't need to worry about different versions of software and libraries on different machines.

### Add docker to back-end
0. Docker engine
We need to install docker engine on the michine if we want to create or run a docker image.
1. Create Dockerfile
To create a docker image, we need to have a Dockfile, this is like a script to tell the docker engine how to make the image.
In the Dockerfile, add the following statements:
```
# build the image on top of python:3.10-slim-buster image
FROM python:3.10-slim-buster
# define the workdir as the working folder in the docker 
WORKDIR /backend-flask
# copy the requirements.txt into the working folder 
COPY requirements.txt requirements.txt
# install the libraries 
RUN pip3 install -r requirements.txt
# copy all files into the folder
COPY . .
# define environment variables
ENV FLASK_ENV=development
# tell the docker will run on which port
EXPOSE ${PORT}
# start the application in the docker and run it on port 4567
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]

```

2. Create the image
Build the image by the following command `Build image -- docker build -t  backend-flask ./backend-flask`.
It means use the ./backend-flask folder and dockerfile in that folder to create an image and tagged it as backend-flask.

3. List the images
After the image built, if we use `docker image ls`, we will see the image on our machine.

4. Run the container
Now we can run the image by `docker run --rm -p 4567:4567 -e FRONTEND_URL='*' -e BACKEND_URL='*' -d backend-flask `.
It means we will create and run the container based on the image called backend-flask on port 4567 in deamon mode. Set both frontend_url and frontend_url as "*".The container will only run one time, when it stops, it will be detroyed. Next time we need to run it, we have to create it from the image again.

5. Check the container
If we call `docker ps`, we should be able to see the docker info while the docker is still running. 

### Add docker to front-end
1. Create docker image
Same as back-end, we need to create a Dockerfile in the front-end folder.
```
FROM node:16.18

ENV PORT=3000

COPY . /frontend-react-js
WORKDIR /frontend-react-js
RUN npm install
EXPOSE ${PORT}
CMD ["npm", "start"]
```

2. Build the image
Same as the back-end process, we will used `docker build -t frontend-react-js ./frontend-react-js` to create the iamge.

3. Run the container
Use `docker run -p 3000:3000 -d frontend-react-js` to create and run the container on port 3000.

## Docker Compose
### Why do we need docker compose
Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.

### Configure and run compose
1. Create a docker-compose.yml file in the root of the whole folder. Add the following statements, note that yml file is very strict with tabs and spaces, so make sure we have the right indents. 
```
version: "3.8"
services:
  backend-flask:
    environment:
      FRONTEND_URL: "https://3000-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
      BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./backend-flask
    ports:
      - "4567:4567"
    volumes:
      - ./backend-flask:/backend-flask
  frontend-react-js:
    environment:
      REACT_APP_BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./frontend-react-js
    ports:
      - "3000:3000"
    volumes:
      - ./frontend-react-js:/frontend-react-js

# the name flag is a hack to change the default prepend folder
# name when outputting the image names
networks: 
  internal-network:
    driver: bridge
    name: cruddur
```

2. Run the compose up
We can run the compose by right clicking the docker-compose.yml file and choose the 'Compose Up' option.

3. Check the ports
Check the ports tab, you should be able to see two containers running on two differnet ports, make sure they are open the public, then you can visit the front-end url. You shoule be able to see the app running. 


## Add Notification Functionality
### Add notifiction functionality to back-end
1. Add new route in openAPI
OpenAPI is a specification for a machine-readable interface definition language for describing, producing, consuming and visualizing RESTful web services.
We can add api information into the openapi-3.0.yml file, and it can automatically generate a page for us to see all the routes.

2. Create new route in app.py
- Create nofifications_activities.py in the services folder.
- Import the service and create new route in the app.py file.

### Add notifiction functionality to front-end
- Create NotificationsPage.js and css into the pages folder. 
- Import  NotificationsPage module and create new route for the notification.

### Rebuild the image and run the compose
Run the containers again, this time, sign up and sign in. There will be a notification button on left side area. Click it and you will be able to see the notifications.

## Add DynamodbLocal and Postgres containers
### Add Dynamodb Local
Add the following statements to the docker-compose.yml file:
```
services:
  dynamodb-local:
    # https://stackoverflow.com/questions/67533058/persist-local-dynamodb-data-in-volumes-lack-permission-unable-to-open-databa
    # We needed to add user:root to get this working.
    user: root
    command: "-jar DynamoDBLocal.jar -sharedDb -dbPath ./data"
    image: "amazon/dynamodb-local:latest"
    container_name: dynamodb-local
    ports:
      - "8000:8000"
    volumes:
      - "./docker/dynamodb:/home/dynamodblocal/data"
    working_dir: /home/dynamodblocal
```
We will be able to build and run a dynamodblocal container 

### Add PostgreSQL
Same as dynamoDB Local, add the following codes:
```
services:
  db:
    image: postgres:13-alpine
    restart: always
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    ports:
      - '5432:5432'
    volumes: 
      - db:/var/lib/postgresql/data
volumes:
  db:
    driver: local
```
