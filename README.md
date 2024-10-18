# Django and Postgre Local Installation

## Prerequisites

- Tested on Linux Mint 21.3 Cinnamon
- Python & Pip

```shell
py --version
# Python 3.10.12
```

Also ensure that "python" points to python 3

```shell
python --version
# Python 3.10.12
```

```shell
pip --version
# pip 22.0.2
```

- PostgreSQL

```shell
sudo -u postgres psql postgres -c 'SELECT version()' | grep PostgreSQL
# PostgreSQL 16.4
```

The following apt package may be required :

- python3-venv
- python3-dev
- libpq-dev
- postgresql
- postgresql-contrib

```shell
sudo apt install python3-venv python3-dev libpq-dev postgresql postgresql-contrib
```

## Database setup


```shell
sudo -u postgres psql
```

```sql
CREATE DATABASE myproject;

CREATE USER myprojectuser WITH PASSWORD 'password';

ALTER ROLE myprojectuser SET client_encoding TO 'utf8';
ALTER ROLE myprojectuser SET default_transaction_isolation TO 'read committed';
ALTER ROLE myprojectuser SET timezone TO 'UTC';

GRANT ALL PRIVILEGES ON DATABASE myproject TO myprojectuser;
\q
```

## Create environment

Create and activate .venv file

```shell
mkdir myprojectdir
cd myprojectdir
py -m venv myprojectenv
source myprojectenv/bin/activate
```

Install django and pg adapter

```shell
cd myprojectenv
pip install django gunicorn psycopg2-binary
```

## Generate Django skeleton

```shell
django-admin startproject myproject ../myprojectdir
```

## Change settings.py

Inside settings.py, add localhost to the list of allowed hosts, like this :

```py
ALLOWED_HOSTS = ['localhost']
```

And database settings like this  :

```py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'myproject',
        'USER': 'myprojectuser',
        'PASSWORD': 'password',
        'HOST': 'localhost',
        'PORT': '',
    }
}
```
