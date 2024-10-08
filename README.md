# Databases-MySQL-NoSQL

# Task 1: SQL Database Creation and Configuration

Here's a step-by-step guide to installing, configuring, and setting up a MySQL database on a CentOS server.

# 1. Install MySQL Database Server

Update the system:

sudo yum update

Install the MySQL repository: You need to set up the MySQL repository if it's not already available. Download the MySQL community repository package.


sudo yum install https://dev.mysql.com/get/mysql80-community-release-el7-

<img width="458" alt="543" src="https://github.com/user-attachments/assets/1ddddf09-67c9-414f-8201-f0b2389404c7">

# 3.noarch.rpm

Install MySQL server:

sudo yum install mysql-server

<img width="449" alt="5432" src="https://github.com/user-attachments/assets/79424c52-95c5-4a76-ae28-e0a0b0be55e8">


Start the MySQL service:

sudo systemctl start mysqld

Enable MySQL to start on boot:

sudo systemctl enable mysqld

<img width="458" alt="ew" src="https://github.com/user-attachments/assets/201bdc4c-4cb7-4c23-a431-9cc9af7d66e6">


# 2. Secure the MySQL Installation

Run the MySQL secure installation script:

sudo mysql_secure_installation

Follow the prompts to set the root password, remove anonymous users, 
disable remote root login, and remove the test database.

<img width="432" alt="432" src="https://github.com/user-attachments/assets/f81dd958-eedb-433c-a3e5-a69ddde37b08">


# 3. Log in to MySQL as the Root User

Log in to the MySQL command-line client:

mysql -u root -p

Enter the root password you set during the secure installation process.

<img width="451" alt="435" src="https://github.com/user-attachments/assets/df1847f2-d090-4d27-ab07-e02bd573f407">


# 4. Create a New Database

Create a new database:

CREATE DATABASE my_database CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
Use the new database:

USE my_database;

# 5. Create Tables and Define Relationships

Create a table:

CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    position VARCHAR(50),
    hire_date DATE
);
Create another table and define a relationship:

CREATE TABLE departments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

CREATE TABLE employee_departments (
    employee_id INT,
    department_id INT,
    FOREIGN KEY (employee_id) REFERENCES employees(id),
    FOREIGN KEY (department_id) REFERENCES departments(id),
    PRIMARY KEY (employee_id, department_id)
);

<img width="456" alt="234" src="https://github.com/user-attachments/assets/7219658d-31ed-4a75-ac3b-a723b428f9c3">


# 6. Configure Additional Parameters

Configure storage engine (InnoDB is recommended): You can set the default storage engine in the MySQL configuration file (/etc/my.cnf).

[mysqld]
default-storage-engine=innodb

<img width="342" alt="654" src="https://github.com/user-attachments/assets/16be017a-fc2c-4d3a-be1d-a1b938c75048">


Indexing example:

CREATE INDEX idx_name ON employees(name);

Logging options: You can adjust logging settings in /etc/my.cnf. For example:

[mysqld]
general_log = 1
general_log_file = /var/log/mysql/mysql.log

<img width="323" alt="65" src="https://github.com/user-attachments/assets/8cb8c920-c12b-42f5-aa86-eb671c06f851">


# 7. Grant Privileges

Create a new user and grant privileges:

CREATE USER 'my_user'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON my_database.* TO 'my_user'@'localhost';
FLUSH PRIVILEGES;

<img width="425" alt="df" src="https://github.com/user-attachments/assets/323e0335-d33a-40e0-ac22-86a722bea01e">


# 8. Test the SQL Database

Insert data:

INSERT INTO employees (name, position, hire_date) VALUES ('Alice', 'Developer', '2024-09-01');
Query data:

SELECT * FROM employees;

Update data:

UPDATE employees SET position = 'Senior Developer' WHERE name = 'Alice';

<img width="457" alt="da" src="https://github.com/user-attachments/assets/f2b26db7-07e9-41bb-9570-51425ab4337d">


Delete data:

DELETE FROM employees WHERE name = 'Alice';

# Completion Criteria

MySQL database is installed, configured, and secured on the CentOS server.

A new database with schema, tables, and relationships is created.

Configuration settings for performance and reliability are adjusted.

Appropriate privileges are granted to users.

Testing confirms that data insertion, retrieval, and modification operations function as expected.

# Task 2: NoSQL Database Creation and Configuration

# 1. Install MongoDB

Import the MongoDB GPG Key:

sudo rpm --import https://www.mongodb.org/static/pgp/server-6.0.asc

Create the MongoDB repository file:

sudo tee /etc/yum.repos.d/mongodb-org-6.0.repo <<EOF
[mongodb-org-6.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/centos/7/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-6.0.asc
EOF

<img width="459" alt="gt" src="https://github.com/user-attachments/assets/54cfd86a-2bc5-47de-858b-3ddb29191807">


Install MongoDB:


sudo yum install -y mongodb-org

# 2. Start MongoDB Service

Start the MongoDB service:

sudo systemctl start mongod

Enable MongoDB to start on boot:

sudo systemctl enable mongod

Verify MongoDB is running:

sudo systemctl status mongod

<img width="456" alt="wd" src="https://github.com/user-attachments/assets/6ffdea6a-79d0-445d-ac8b-1b90b2adbf0b">


3. Access MongoDB Shell

Access the MongoDB shell:

mongo

4. Create a New Database

Create or switch to a new database:

use my_database

<img width="149" alt="xs" src="https://github.com/user-attachments/assets/e119c931-99f0-4147-bd80-4fd6162693d0">


5. Define Collections

Create a new collection:

db.createCollection("employees")

Alternatively, you can insert documents directly into a collection which will create the collection if it does not exist:

db.employees.insertOne({ name: "Alice", position: "Developer", hire_date: new Date() })

<img width="455" alt="89" src="https://github.com/user-attachments/assets/e4d3f62e-b7a7-438b-8432-ee8a89650fc5">


# 6. Configure Indexes

Create an index on the name field of the employees collection:

db.employees.createIndex({ name: 1 })

# 7. Configure Additional Settings

Storage Engine Options: MongoDB defaults to WiredTiger. You can specify 
storage engine settings in the MongoDB configuration file /etc/mongod.conf:
storage:
  dbPath: /var/lib/mongo
  journal:
    enabled: true
Journaling: Journaling is enabled by default in MongoDB.

Authentication: Add the following to /etc/mongod.conf to enable authentication:

security:
  authorization: "enabled"

<img width="152" alt="ca" src="https://github.com/user-attachments/assets/bdbdacac-01fb-481a-89d1-dc24fb249ce6">

  
Restart MongoDB for changes to take effect:

sudo systemctl restart mongod

Create an admin user:

use admin
db.createUser({
  user: "admin",
  pwd: "password",
  roles: [{ role: "userAdminAnyDatabase", db: "admin" }]
})

<img width="322" alt="re" src="https://github.com/user-attachments/assets/d8086ae6-3e19-4438-97d3-9e2237faed2a">

# 9. Test the NoSQL Database

Insert data:

db.employees.insertOne({ name: "Bob", position: "Manager", hire_date: new Date() })

Query data:

db.employees.find()

<img width="459" alt="fw" src="https://github.com/user-attachments/assets/4de30356-6454-45b7-8495-d8bb1f249fb0">

Update data:

db.employees.updateOne({ name: "Bob" }, { $set: { position: "Senior Manager" } })

<img width="456" alt="c3" src="https://github.com/user-attachments/assets/6ad0951c-2b75-49bd-abf0-fe474e60d255">


Delete data:

db.employees.deleteOne({ name: "Bob" })

<img width="228" alt="xz" src="https://github.com/user-attachments/assets/c78c2a3e-b9d2-46b7-a066-b7d4d977092f">


# Completion Criteria

MongoDB is installed, configured, and running on the CentOS server.

A new database is created with collections and optional sharding.

Indexes and additional settings are configured for performance and scalability.

Database operations such as data insertion, retrieval, and modification are tested and confirmed to work correctly.
Sharded clusters and replication, if implemented, are operational.
