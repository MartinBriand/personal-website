# My personal website

Hi, thank you for reading about this project.

## Reasons behind the project

* I need a website for my scientific publications
* I want to learn more about coding a website which is the reason why I use much more complex tools than a simple WordPress website.
* I would like this website to become a broader part of my online life. Therefore, I would like to be able to add modules to it, like a blog posts module, a secret Santa module, a votation module… So a simple WordPress might soon be too limited.

## Technologies of the project

* Django restful API for the backend
* React for the front end
    * Perhaps some other front-end tools to make the website look good
* Simple containerization for running the server with Docker

## Contributing

I really want to learn, so it would be better if no one contributes directly to the code but me. However, if you want to warn me about problems in my code or security issues, or things I could do better, you are more than welcome. Do not hesitate to contact me.

Also, I made the project public and open-source, so that anyone could duplicate the code and use it for his/her own purpose. So do not hesitate to do so (even if I doubt there will be a lot of people doing so).

## Setting-up the development environment

### Backend

#### Django

Install a Python environment in the `backend` folder.

```bash
python3 -m virtualenv .venv
```

And activate it (you will have to redo this each time you work on that project)

```bash
source .venv/bin/activate
```

Install the required packages

```bash
pip install -r requirements.txt
```


#### Database

Install PostgreSQL

```bash
sudo apt install postgresql
```

And start the service (you will have to redo this each time you work on the project)

```bash
sudo service postgresql start
```

In another terminal window, connect to the database and start a `psql` session.

```bash
sudo -u postgres psql
```

In the session, create a user with a pretty simple password. You will change the password at the time of production, so for the time of development, don’t worry too much about security issues.

```sql
CREATE USER personal_website_djangobackend_user WITH encrypted PASSWORD 'dummy_password';
```

And make it compliant with good web development practices:

```sql
ALTER ROLE personal_website_djangobackend_user SET client_encoding TO 'utf8';
ALTER ROLE personal_website_djangobackend_user SET default_transaction_isolation TO 'read committed';
ALTER ROLE personal_website_djangobackend_user SET timezone TO 'UTC';
```

Finally give the backend user some rights to create databases and alter the created databases (useful for testing).

```sql
ALTER USER personal_website_djangobackend_user CREATEDB;
```

Now create the main database

```sql
CREATE DATABASE personal_website_database;
```

And give privileges on that database to the backend user

```sql
GRANT ALL PRIVILEGES ON DATABASE personal_website_database TO personal_website_djangobackend_user;
```

```sql
\c personal_website_database
```

```sql
GRANT ALL PRIVILEGES ON SCHEMA public TO personal_website_djangobackend_user;
```

```sql
\q
```

Your development database is almost all set. Now we make Django able to connect to it by creating a `development_credentials.py` file in the `backend` folder like below. (Make sure the file has this exact name for the file is “gitignored” and is imported in other parts of the code.)

```python
# PostgreSQL development credentials
DBNAME="personal_website_database"
DBUSER="personal_website_djangobackend_user"
PASSWORD_DBUSER="dummy_password"
```

Finally, you can type the following command in a bash shell in the `backend` folder with the Python environment activated:

```bash
python manage.py migrate
```

This will create all the tables for the database. You should see only "OK"s.

During the time of development, you may need to access the database to see the different entries.

##### Connecting to the database with the cli
 
 Type in:

 ```bash
 psql -U personal_website_djangobackend_user -h localhost -d personal_website_database
 ```

 ##### Connecting to the database using pgadmin

 Download pgAdmin on Windows and set it up as follows:

1. Install pgAdmin on Windows and start the app.
1. Click on `Add New Server`
1. In `General`, enter a name (for example: `personal_website_developdb`)
1. In `Connection`, enter
    1. Host name/address: `localhost`
    1. Port: `5432`
    1. Maintenance database: `personal_website_database`
    1. Username: `personal_website_djangobackend_user`
    1. Password: `dummy_password`


## Project management

I don't have any deadlines, but here are the different steps I am planning to work on :

* Create a backend for my scientific publications
* Create à backend for some blog posts
* Create a backend for a Secret Santa app
* Create a backend for a polling app
* Create a front end for all this apps

More specifically, there is a GitHub Project associated with the repository.
