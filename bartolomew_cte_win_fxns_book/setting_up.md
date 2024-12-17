# Setting up using Docker

Rather than installing MySQL, decided to use Docker. Followed the instructions given here in a [Datacamp tutorial](https://www.datacamp.com/tutorial/set-up-and-configure-mysql-in-docker)

- [A useful Stackoverflow entry on loading data locally in MySQL](https://stackoverflow.com/questions/59993844/error-loading-local-data-is-disabled-this-must-be-enabled-on-both-the-client)

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

## GitHub data for the book

- [Link to GitHub repo where the zipped CSV files are stored](https://github.com/Apress/mariadb-and-mysql-common-table-expressions-and-window-functions-revealed/blob/master/bartholomew-ch01_code.zip)

## Local install of the _mysql-client_ client

"It is possible to connect to the MySQL server outside the container, as well. For example, to connect from your host machine, you can install the MySQL client manually in your system."


- Update apt and install the mysql-client

```sh
sudo apt update
sudo apt install mysql-client
```

- Check the port mappings

```sh
docker port mysql-db
```

- Connect to the MySQL server from the local client

```sh
mysql --host=127.0.0.1 --port=3306 -u root -p
```

## Removing double quotes from strings in all columns of the new table

For some reason, each text value in the VARCHAR columns is encloses in double quotes so to run a query to for example get all rows where the title is 'hr', requires the following:

```sql
select * from employees where title = '"hr"';
```

To fix this, the following update was run for each of the three VARCHAR columns:

```sql
UPDATE employees SET title = REPLACE(title, '"', ''); -- repeated for columns name and office
```