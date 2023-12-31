# Storing Database Credentials in AWS Secrets Manager.


Purpose of the this hands-on training is to show how to store database credentials securely via AWS Secrets Manager.
 

## Learning Outcomes

At the end of the this extra hands-on training, students will be able to;

- use AWS secret manager 

- Implement the AWS secrets manager to Python-Flask code



## Outline

- Part 1 - Create a Secret in AWS Secrets Manager

- Part 2 - Run the the Python code in EC2


## Part 1 - Examine and Run the Sample Web Application with SQLite and database implementation in your Local

- We assume that you have already created an MySQL Database with following attitude.

```
- Engine option         : MySQL
- Version               : 8.0.32
- Template              : Free tier
- DB instance class     : db.t2.micro
- Publicly accessible   : ***Yes
- Master username       : admin
- Master password       : Clarusway_1
- Initial DB name       : clarusway

```
  

- Go to the AWS Secrets Manager

- Click on "Store a new Secret"

    Choose secret type: `Credentials for Amazon RDS database`
    Credentials: 
        - User name: `admin`
        - Password: `Clarusway_1`
    Encryption key: `Keep it as is`
    Database: `Choose the database you created`
    Secret name and description: `aws-flask-demo-credential`

- Keep all settings as default and create Secret.

- Open the secret and retrieve the credentials. 


## Part 2 -  Run the the Python code in EC2

- Create a IAM role for EC2 to retrieve data from AWS Secrets Manager 


Trusted entity type  : `AWS Service`
Use case             : `EC2`
Permissions policies : `SecretsManagerReadWrite`
Role name            : `Secret_Manager_EC2` 



- Create if you don't have any EC2 instance. 


    Instance        : `t2.micro`
    Image           : `Amazon 2023 AMI`
    Security Group  : `SSH+HTTP+8080 >>>>.0.0.0.0/0`
    IAM Profile     : `Secret_Manager_EC2` 

- If you have already had a EC2 instance check the security group and attach `Secret_Manager_EC2` IAM Role 

- Connect to EC2 instance 

- Install the dependency 

```
sudo yum update -y
sudo yum install python3 -y
sudo yum install python-pip
sudo pip3 install flask
sudo pip3 install flask-mysql
sudo pip3 install boto3
sudo yum install git -y
```

- Upload the `templates` folder and `app-with-mysql.py` 

```
git clone [your public repo]
```
- open the `app-with-mysql.py` 

- Run the code and show the connection between  RDS database and Flask

- Terminate the Instance and the Secret

# Hands-on Flask-05 : Handling SQL with Flask Web Application

Purpose of the this hands-on training is to give the students introductory knowledge of how to handle forms, how to connect to database and how to use sql within Flask web application on Amazon Linux 2023 EC2 instance. 

![Databases in Flask](./database.png)

## Learning Outcomes

At the end of the this hands-on training, students will be able to;

- install Python and Flask framework on Amazon Linux 2023 EC2 instance.

- build a web application with Python Flask framework.

- handle forms using the flask-wtf module.

- configure connection to the `sqlite` database.

- configure connection to the `MySQL` database.

- work with a database using the SQL within Flask application.

- use git repo to manage the application versioning.

- run the web application on AWS EC2 instance using the GitHub repo as codebase.

## Students are expected to 

- Examine and run the `app-with-sqlite.py` 

- Create `app-with-mysql.py` at same folder and  configure the same application according to RDS MYSQL database. 

## Outline

- Part 1 - Run the  Sample Web Application with SQLite and database implementation

- Part 2 - Write same application with MySQL

- Part 3 - Install Python and Flask framework on Amazon Linux 2023 EC2 Instance using RDS


## Part 1 - Examine and Run the Sample Web Application with SQLite and database implementation in your Local

- Examine which module are imported for this project 

- Examine the anatomy of the `Database` configuration of `app-with-sqlite.py`

```
- It adds required environmental variables for SQLite  according to documentation    
   https://flask-sqlalchemy.palletsprojects.com/en/2.x/config/

- It drops users table if exists, creates new users table and adds some rows for sample

- It Executes sql commands to  commit data to tables

```

- Examine the anatomy of the `Functions`  determined in `app-with-sqlite.py`

```
- Function named `find_emails` which find emails using keyword from the user table in the db,
 and returns result as tuples `(name, email)`.

- Function named `insert_email` which adds new email to users table the db.
```

- Examine the anatomy of  the `Decorators `  determined in `app-with-sqlite.py`

```
- Function named `emails`  finds email addresses by keyword using `GET` and `POST` methods,
uses template files named `emails.html` given under `templates` folder
and it assigns to the static route of ('/')

- Function named `add_email` inserts new email to the database using `GET` and `POST` methods,
uses template files named `add-email.html` given under `templates` folder and  it assign to the static route of ('add')

- The Flask application  can be reached from any host on port 8080.
```

- Be sure that you install `flask-mysql, sqlalchemy, Flask-SQLAlchemy` via  `sudo pip3 install `

- Run the  application `app-with-sqlite.py` 

- Check the database tables via SQLite Browser ( You can download the SQLite Browser from the link : 
https://sqlitebrowser.org/)



## Part 2 - Write same application with MySQL and run on in your Local

- Create an RDS database with following configurations: 

```
- Engine option         : MySQL
- Version               : 8.0.32
- Template              : Free tier
- DB instance class     : db.t2.micro
- Publicly accessible   : ***Yes
- Master username       : admin
- Master password       : Clarusway_1
-Initial DB name        : clarusway

```
- Create `app-with-mysql.py` the  same folder near the `app-with-sqlite.py` 

- Configure the same application according to RDS MYSQL database rather than SQLite

- Check the documentation for environment variable

https://flask-mysqldb.readthedocs.io/en/latest/

- commit your code and push it to your GitHub repo

- Launch an Instance and pull your files in to it


## Part 3 - Install Python and Flask framework on Amazon Linux 2023 EC2 Instance 

- Launch an Amazon EC2 instance using the Amazon Linux 2023 AMI with security group allowing SSH (Port 22) and HTTP (Port 8080) connections.

- Connect to your instance with SSH.

- Update the installed packages and package cache on your instance.

- Install `Python 3` and `pip3` packages.

- Check the python3  and `pip3` version

- Install `Python 3 Flask` framework.

- Install `flask_mysql`.

- ıf its not working with error code "cannot import name '_request_ctx_stack' from 'flask'" 

  `pip install Flask==2.3.3`

- Run application with Python