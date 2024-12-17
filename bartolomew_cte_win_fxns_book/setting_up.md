# Setting up using Docker

Rather than installing MySQL, decided to use Docker. Followed the instructions given here in a [Datacamp tutorial](https://www.datacamp.com/tutorial/set-up-and-configure-mysql-in-docker)

## Local set-up

- copy the file for chapter 1 the Docker volume directory

```sh
docker cp bartholomew-ch01.csv mysql-db:/var/lib/mysql
```

- Create the shell for logging in to the terminal

```sh
docker exec -it mysql-db bash
```

- Log in to the MySQL server (Not sure if `--local-infile=1` option is needed)

```sh
mysql --local-infile=1 -u root -p
```

- Connect to the database named _bart_

```sql
use bart
```

- Create the table

```sql
CREATE TABLE employees (
  id serial primary key,
  name VARCHAR(150) NOT NULL,
  title VARCHAR(100),
  office VARCHAR(100)
);
```

- Load the data for chapter 1

```sql
LOAD DATA LOCAL INFILE 'var/lib/mysql/bartholomew-ch01.csv' INTO TABLE employees FIELDS TERMINATED BY ',';
```
