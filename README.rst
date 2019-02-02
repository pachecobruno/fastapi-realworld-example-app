.. image:: logo.png

Quickstart
----------

First, run ``PostgreSQL``, set enviroment variables and create database. For example using ``docker``.::

    export POSTGRES_DB=rwdb POSTGRES_PORT=5432 POSTGRES_USER=postgres POSTGRES_PASSWORD=postgres
    docker run --name pgdb --rm -e POSTGRES_USER="$POSTGRES_USER" -e POSTGRES_PASSWORD="$POSTGRES_PASSWORD" -e POSTGRES_DB="$POSTGRES_DB" postgres
    export POSTGRES_HOST=$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' pgdb)
    createdb --host=$POSTGRES_HOST --port=$POSTGRES_PORT --username=$POSTGRES_USER $POSTGRES_DB

Then run the following commands to bootstrap your environment with ``poetry``.::

    git clone https://github.com/nikelwolf/fastapi-realworld-example-app
    cd fastapi-realworld-example-app
    poetry install
    poetry shell

Then create ``.env`` file in project root and set environment variables for application.::

    touch .env
    echo PROJECT_NAME="FastAPI RealWorld Application Example" >> .env
    echo DATABASE_URL=postgresql://$POSTGRES_USER:$POSTGRES_PASSWORD@$POSTGRES_HOST:$POSTGRES_PORT/$POSTGRES_DB >> .env
    echo SECRET_KEY=$(openssl rand -hex 32) >> .env
    echo ALLOWED_HOSTS='"127.0.0.1", "localhost"' >> .env

To run the web application in debug use::

    alembic upgrade head
    uvicorn app.main:app --debug


Structure
---------

::

    models  - pydantic models that used in crud or handlers
    crud    - CRUD for types from models (create new user/article/comment, check if user is followed by another, etc)
    db      - db specific utils
    core    - some general components (jwt, security, configuration)
    api     - handlers for routes
    main.py - FastAPI application instance, CORS configuration and api router including


TODO
----

1) Implement project
2) Remove unnecessary dependencies from FastAPI
3) Deployment with ``Dockerfile`` and ``docker-compose``
4) Add tests