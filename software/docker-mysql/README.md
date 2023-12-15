# MySQL Docker with Host Data
> [Sheik Sadi•December 15, 2023](https://chat.openai.com/share/85e21ab0-e99b-463d-98f4-0449a726bddf)

## Run a mysql server with docker but store the data on the host.

Certainly! You can run a MySQL server with Docker and store the data on the host machine using Docker volumes. Docker volumes allow you to persist data outside the container, making it easy to manage and preserve data even if the container is stopped or removed.

Here's a basic example of running a MySQL server with Docker and storing the data on the host:

# Create a directory on the host to store MySQL data

```bash
mkdir -p /path/to/mysql_data
```
# Run the MySQL container with a Docker volume
```bash
docker run -d \
  --name mysql-container \
  -e MYSQL_ROOT_PASSWORD=my-secret-pw \
  -e MYSQL_DATABASE=mydatabase \
  -v /path/to/mysql_data:/var/lib/mysql \
  -p 3306:3306 \
  mysql:latest
```

Explanation:
- `mkdir -p /path/to/mysql_data`: Create a directory on the host machine where MySQL data will be stored.

- `docker run -d ...`: Run the MySQL Docker container in detached mode.

- `--name mysql-container`: Assign a name to the container (you can choose any name you prefer).

- `-e MYSQL_ROOT_PASSWORD=my-secret-pw`: Set the root password for MySQL.

- `-e MYSQL_DATABASE=mydatabase`: Create an initial database named "mydatabase".

- `-v /path/to/mysql_data:/var/lib/mysql`: Mount the host directory `/path/to/mysql_data` to the container's `/var/lib/mysql` directory. This is where MySQL stores its data.

- `-p 3306:3306`: Map port `3306` on the host to port `3306` on the container (MySQL default port).

- `mysql:latest`: Use the latest version of the MySQL Docker image. You can specify a specific version if needed.

**Note:** Make sure to replace `/path/to/mysql_data` with the actual path where you want to store MySQL data on your host machine.

After running this command, you should have a MySQL server running in a Docker container, and the data will be stored on the host in the specified directory. If you stop or remove the container, the data will persist in the host directory.

## Okay, I have created the container. Now I want to use python to test the DB in mysql

Great! To interact with a MySQL database using Python, you can use the mysql-connector-python library. If you haven't installed it yet, you can do so using the following:

```sh
pip install mysql-connector-python
```
Now, you can create a Python script to connect to your MySQL database and perform some basic operations. Here's a simple example:

```python
import mysql.connector

# Connect to the MySQL server
conn = mysql.connector.connect(
    host='localhost',
    user='root',
    password='my-secret-pw',
    database='mydatabase'
)

# Create a cursor object to execute SQL queries
cursor = conn.cursor()

# Create a table
create_table_query = """
CREATE TABLE IF NOT EXISTS users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL
)
"""
cursor.execute(create_table_query)

# Insert some data
insert_data_query = """
INSERT INTO users (username, email) VALUES (%s, %s)
"""
user_data = [
    ('user1', 'user1@example.com'),
    ('user2', 'user2@example.com'),
    ('user3', 'user3@example.com')
]
cursor.executemany(insert_data_query, user_data)

# Commit the changes
conn.commit()

# Fetch and print the data
select_data_query = "SELECT * FROM users"
cursor.execute(select_data_query)
result = cursor.fetchall()

for row in result:
    print(row)

# Close the cursor and connection
cursor.close()
conn.close()
```
Make sure to replace the connection details (host, user, password, database) with your actual MySQL configuration.

This script connects to your MySQL database, creates a table if it doesn't exist, inserts some data into the table, fetches and prints the data. You can customize this script based on your specific requirements and database schema.

I want to run a docker exec on container "mysql-server" to echo $PATH
ChatGPT
To run a docker exec command on a container named "mysql-server" and echo the $PATH variable, you can use the following command:

```bash
docker exec mysql-server sh -c 'echo $PATH'
```
This command executes the sh -c 'echo $PATH' command inside the "mysql-server" container. It starts a shell (sh) and runs the specified command within that shell. The command is then used to print the value of the $PATH variable.

Make sure to replace "mysql-server" with the actual name or ID of your MySQL container.
