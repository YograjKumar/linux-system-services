Here’s an improved Docker Compose file with .env support. This lets you configure usernames, passwords, and ports easily without modifying the YAML each time.
Create a .env file (in the same folder as docker-compose.yml): add the same config as present in 'example.env'

How it works:
1. Docker Compose automatically loads the .env file.
2. ${VARIABLE} in the YAML will be replaced with the value from .env.
3. If you want to set different usernames, passwords, or ports on another system, just change the .env file — no need to touch the Compose file itself.

on runing the compose file all credentials and ports come from the .env file.
---------------------------------------------------------------------------------------

# To run the docker-compose file
docker compose -f system-docker-compose.yaml up -d
---------------------------------------------------------------------------------------

# To stop and remove all containers, networks, and default volumes from the stack
docker compose -f system-docker-compose.yaml down

-This will stop all running containers from the Compose file.
-Remove the containers, networks, and default volumes created by the Compose stack.

# Remove named volumes (if you want to clear MongoDB & Redis data too)
By default, docker compose down does not remove named volumes (mongo_data and redis_data).
To remove them: docker compose -f system-docker-compose.yaml down -v

-v → deletes all volumes declared in the Compose file.
Caution: This deletes all MongoDB and Redis data stored in volumes.

# Remove images
If you also want to remove the downloaded images:
docker compose -f system-docker-compose.yaml down --rmi all -v

--rmi all → removes the images used by the stack.
-v → removes volumes.
After this, it’s like the stack was never deployed.
---------------------------------------------------------------------------------------

After running the compose file following will be the url for the host-machine of the following containers:
1. MONGO_DB: mongodb://admin:admin0147@localhost:27017
2. REDIS: redis//:admin0147@localhost:6379
---------------------------------------------------------------------------------------

To connect your app with the elastic-search
1. run the elasticsearch terminal, but we are working with docker so we will run
docker exec -it elasticsearch /bin/bash

2. Then run command with below given syntax
elasticsearch-users useradd YOUR_CUSTOM_ELASTIC-SEARCH_USERNAME -p YOUR_CUSTOM_ELASTIC-SEARCH_PASSWORD -r superuser

NOTE: For full access (like elastic), use superuser. For safer access, create a custom role with only required permissions.
