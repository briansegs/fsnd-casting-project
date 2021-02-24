# Full Stack Casting Agency API

## Introduction

This Casting Agency API is an artist and movie management tool that gives members of a casting agency different levels of access to information based on their role in the company.

For this project, I used Auth0 for role based access control, built out the data models to power the API endpoints, tested those endpoints by building a Python Unittest file, and then deployed the project to Heroku.

You can see the site live [Here](https://fsnd-casting-project.herokuapp.com/)

## Overview

This API is fully functional and is capable of doing the following:

* Creating new movies and artists.
* Searching for movies and artists.
* Deleting movies and artists.
* Editing movies and artists.
* Managing access to endpoints based on role in JWT.

## Tech Stack (Dependencies)

### Pre-requisites and Local Development

Developers using this project should already have Python3 and pip installed on their local machines.

#### Python 3.7

Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)

#### Virtual Enviornment

We recommend working within a virtual environment whenever using Python for projects. This keeps your dependencies for each project separate and organaized. Instructions for setting up a virual enviornment for your platform can be found in the [python docs](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)

#### PIP Dependencies

Once you have your virtual environment setup and running, install dependencies by running:

```bash
pip install -r requirements.txt
```

This will install all of the required packages we selected within the `requirements.txt` file.

#### Key Dependencies

[Flask](http://flask.pocoo.org/) is a lightweight backend microservices framework. Flask is required to handle requests and responses.

[SQLAlchemy](https://www.sqlalchemy.org/) is the Python SQL toolkit and ORM we'll use handle the lightweight sqlite database. You'll primarily work in app.py and can reference models.py.

[Flask-CORS](https://flask-cors.readthedocs.io/en/latest/#) is the extension we'll use to handle cross origin requests from our frontend server.

## Main Files: Project Structure

  ```sh
  ├── README.md
  ├── requirements.txt *** The dependencies you need to install with "pip3 install -r requirements.txt"
  ├── Procfile *** For Heroku to launch the gunicorn server
  ├── manage.py *** For Heroku to manage migrations
  ├── .gitignore
  ├── app.py *** The main driver of the app.
  ├── auth.py *** Handels authentication and authorization with Flask
  ├── test_app.py *** Tests all API endpoints
  └── models.py *** Sets up the Database and Models
  ```

Implimented
-----

  1. Relational Database Architecture in `models.py`.
  2. Using SQLAlchemy, I set up normalized models for the objects supported in my app in `models.py`.
  3. Implemented the controllers for my Flask API endpoints in `app.py`.
  4. Implemented authentication and authorization in Flask in `auth.py`
  5. Implemented role-based control design patterns in `app.py`
  6. Testing Flask Application in `test_app.py`
  7. Deployed Application to Heroku


## Testing

To run the local server

```bash
export FLASK_APP=app
export FLASK_DEBUG=true
export FLASK_ENV=development
flask run

```

To run the tests, run

```bash
python3 test_app.py
```

## API Reference

### Getting Started

* The backend app is hosted on Heroku at https://fsnd-casting-project.herokuapp.com/
>**Note** - All Endpoints require an authentication token except the home endpoint (`/`)

## Error Handling

Errors are returned as JSON objects in the following format:
```bash
{
    "success": False,
    "error": 405,
    "message": "Method Not Allowed"
}
```

## API Role Based Access Control

Our API has 3 roles :

1. Casting Assistant
    * Can view actors and movies

2. Casting Director
    * All permissions a Casting Assistant has
    * Add or delete an actor from the database
    * Modify actors or movies

3. Executive Producer
    * All permissions a Casting Director has
    * Add or delete a movie from the database


## Endpoints

### GET '/movies'

- request movies in the database
- Returns:Json opject contains {'succes': True, 'movies': []}
- **sample :** `curl http://127.0.0.1:5000/movies -H "Authorization: Bearer <ACCESS_TOKEN>"`

```bash
{
    "movies": [
        {
            "id":1,
            "release_date":"1986",
            "title":"The Man"
        },
        {
            "id":2,
            "release_date":"2010",
            "title":"Steel"
        },
        {
            "id":3,
            "release_date":"1996",
            "title":"Heart"
        },
        {
            "id":4,
            "release_date":"2020",
            "title":"In the end"
        }
    ],
    "success":true
}
```
### GET '/actors'

- request actors in the database
- Returns:Json opject contains {'succes': True, 'actors': []}
- **sample :** `curl http://127.0.0.1:5000/actors -H "Authorization: Bearer <ACCESS_TOKEN>"`

```bash
{
    "actors": [
        {
            "age":34,
            "gender":"Male",
            "id":1,
            "name":"Brian Segers"
        },
        {
            "age":35,
            "gender":"Female",
            "id":2,
            "name":"Kat K"
        },
        {
            "age":62,
            "gender":"Male",
            "id":3,
            "name":"Ron Segs"
        },
        {
            "age":36,
            "gender":"Male",
            "id":4,
            "name":"James Parker"
        }
    ],
    "success":true
}

```

### DELETE '/movies/<int:id>'

- delete a single movie by id
- Returns: Json opject contains {'succes': True, 'delete': movie_id}
- **sample :** `curl -X DELETE http://127.0.0.1:5000/movies/1 -H "Authorization: Bearer <ACCESS_TOKEN>"`

```bash
{
    "delete":"1",
    "success":true
}
```

### DELETE '/actors/<int:id>'

- delete a single actor by id
- Returns: Json opject contains {'succes': True, 'delete': actor_id}
- **sample :** `curl -X DELETE http://127.0.0.1:5000/actors/1 -H "Authorization: Bearer <ACCESS_TOKEN>"`

```bash
{
    "delete":"1",
    "success":true
}
```

### POST '/movies'

- create new movie
- Request Arguments: json object contains (title, year)
- Returns: Json opject contains {'succes': True, 'movie': {}}
- **sample :** `curl -X POST http://127.0.0.1:5000/movies -H "Authorization: Bearer <ACCESS_TOKEN>" -d {'title':'Head Strong','release_date':'2020'}`

```bash
{
  "movie":
  {
    "id": 1,
    "release_date": "2020",
    "title": "Head Strong"
  },
  "success": true
}
```

### POST '/actors'

- create new actor
- Request Arguments: json object contains (name, age, gender)
- Returns: Json opject contains {'succes': True, 'actor': {}}
- **sample :** `curl -X POST http://127.0.0.1:5000/actors -H "Authorization: Bearer <ACCESS_TOKEN>" -d {'name': 'Sam England','age': '25','gender': 'male'}`

```bash
{
  "actor":
  {
    "id": 1,
    "name": "Sam England",
    "age": "25",
    "gender": "male"
  },
  "success": true
}
```

### PATCH '/movies/<int:id>'

- modify specific movie by id
- Request Arguments: json object contains at least one of these values (title, year)
- Returns: Json opject contains {'succes': True, 'movie': {}}
- **sample :** `curl -X PATCH hhttp://127.0.0.1:5000/movies/1 -H "Authorization: Bearer <ACCESS_TOKEN>" -d '{"title": "Big Head"}'`

```bash
{
  "movie":
  {
    "id": 1,
    "release_date": "2020",
    "title": "Big Head"
  },
  "success": true
}
```

### PATCH '/actors/<int:id>'

- modify specific actor by id
- Request Arguments: json object contains at least one of these values (name, age, gender)
- Returns: Json opject contains {'succes': True, 'actor': {}}
- **sample :** `curl -X PATCH hhttp://127.0.0.1:5000/actors/1 -H "Authorization: Bearer <ACCESS_TOKEN>" -d '{"name": "Sammy E"}'`

```bash
{
  "actor":
  {
    "id": 1,
    "name": "Sammy E",
    "age": "25",
    "gender": "male"
  },
  "success": true
}
```

