# How to Setup a Basic Server for Web

Very opinionated guide how to setup a basic server to serve web apps.

Stack: NGINX, ASP.NET Core, Postgres, tbc.

### 1. Nginx

### 2. ASP.NET  Core

### 3. Postgres

```
sudo apt install -y postgresql-common
```

```
sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh
```
This script will enable the PostgreSQL APT repository on apt.postgresql.org on
your system. The distribution codename used will be noble-pgdg.

Install `postgresql` database server package (version 17 at a time):
```
sudo apt install -y postgresql
```

#### 3.a. Postgres credentials

Log in to the PostgreSQL database server using the postgres user account.
```
sudo -u postgres psql
```

Modify the default postgres user with a new strong password:
```
ALTER USER postgres WITH ENCRYPTED PASSWORD 'your_own_strong_password';
```

To exit psql console:
```
\q
```

To enable password authentication on the server:
```
sudo sed -i '/^local/s/peer/scram-sha-256/' /etc/postgresql/17/main/pg_hba.conf
```

Restart `postgresql` to apply changes:
```
sudo systemctl restart postgresql
```
