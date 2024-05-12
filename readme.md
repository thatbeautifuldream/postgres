```markdown
# PostgreSQL with Adminer Docker Compose Setup

This repository contains a Docker Compose configuration for setting up a PostgreSQL database container along with an Adminer container for a web-based database management interface.

## Prerequisites

- Docker Engine installed on your machine
- Docker Compose installed on your machine

## Getting Started

1. Clone this repository or copy the `docker-compose.yml` file to your local machine.

2. Navigate to the directory containing the `docker-compose.yml` file.

3. Run the following command to start the containers:
```

docker-compose up -d

````

This command will start the PostgreSQL and Adminer containers in detached mode.

## Configuration

The `docker-compose.yml` file is configured as follows:

```yaml
version: "3"
services:
  db:
    image: postgres
    restart: always
    volumes:
      - ./data/db:/var/lib/postgresql/data
    ports:
      - 5432:5432
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres

  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080
````

### PostgreSQL Container

- `image: postgres`: Uses the official PostgreSQL Docker image.
- `restart: always`: Ensures that the container restarts automatically if it stops for any reason.
- `volumes`: Mounts a local directory (`./data/db`) to the container's `/var/lib/postgresql/data` directory, which is where PostgreSQL stores its data. This allows data persistence across container restarts or removals.
- `ports`: Maps the host's port 5432 to the container's port 5432, allowing access to the PostgreSQL server from the host machine.
- `environment`: Sets the following environment variables:
  - `POSTGRES_DB`: The name of the default database to be created.
  - `POSTGRES_USER`: The username for the default user.
  - `POSTGRES_PASSWORD`: The password for the default user.

### Adminer Container

- `image: adminer`: Uses the official Adminer Docker image.
- `restart: always`: Ensures that the container restarts automatically if it stops for any reason.
- `ports`: Maps the host's port 8080 to the container's port 8080, allowing access to the Adminer web interface from the host machine.

## Accessing the Database

### Using psql

To access the PostgreSQL database running in the Docker container, you can use a PostgreSQL client like `psql`.

1. Find the container ID or name using the `docker ps` command.

2. Connect to the database using the following command:

```
docker exec -it <container-id-or-name> psql -U postgres
```

Replace `<container-id-or-name>` with the actual container ID or name of the PostgreSQL container.

3. When prompted, enter the password specified in the `docker-compose.yml` file (`postgres` in this case).

You can now interact with the PostgreSQL database using the `psql` prompt.

### Using Adminer

To access the Adminer web interface, open your web browser and navigate to `http://localhost:8080`.

You should see the Adminer login page. Enter the following credentials:

- System: `PostgreSQL`
- Server: `db` (or the container name/ID of the PostgreSQL container)
- Username: `postgres`
- Password: `postgres`
- Database: `postgres`

Click "Login" to access the Adminer interface and manage your PostgreSQL database.

## Data Persistence

The PostgreSQL data is stored in the `./data/db` directory on your local machine, which is mounted to the container's `/var/lib/postgresql/data` directory. This ensures that your data persists even if you stop or remove the container.

## Cleanup

To stop and remove the PostgreSQL and Adminer containers, run the following command:

```
docker-compose down
```

This command will stop and remove the containers, as well as the associated network.

If you want to remove the persistent data stored in the `./data/db` directory, you can manually delete it after stopping the containers.

```bash
rm -rf ./data/db
```

Change permissions of the `./data/db` directory if needed.
