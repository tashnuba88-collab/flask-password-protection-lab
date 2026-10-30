# Flask Password Protection Lab

A Flask API with a React frontend that lets users sign up, log in, and log out using hashed passwords and session-based authentication.

## What it does

- Users sign up with a username and password.
- Passwords are never stored in plain text. Flask-Bcrypt hashes each password and only the hash is saved in the database.
- Logging in checks the submitted password against the stored hash.
- The user's ID is kept in the Flask session, so the app can tell who is logged in.
- After login, the homepage greets the user by username.

## Setup

From the project root:

```
pipenv install && pipenv shell
npm install --prefix client
cd server
flask db upgrade head
```

## Running the app

Start the Flask server from the `server` folder:

```
python app.py
```

In a second terminal, start the React client from the project root:

```
npm start --prefix client
```

The API runs on port 5555.

## Running the tests

From the `server` folder, with the pipenv shell active:

```
pytest -x
```

## API endpoints

| Method | Route | What it does |
| ------ | ----- | ------------ |
| POST | /signup | Creates a user, saves the hashed password, logs the user in, and returns the user as JSON (201). |
| GET | /check_session | Returns the logged-in user as JSON, or an empty response with status 204 if nobody is logged in. |
| POST | /login | Checks the username and password, logs the user in, and returns the user as JSON. Returns 401 if the credentials are wrong. |
| DELETE | /logout | Removes the user ID from the session and returns 204. |
| DELETE | /clear | Clears the session (provided with the starter code). |

## How password protection works

The `User` model in `server/models.py` stores the hash in a `_password_hash` column.

- `password_hash` is a hybrid property. Reading it raises an exception, so the hash can't be viewed through the model.
- Setting `password_hash` runs the password through `bcrypt.generate_password_hash` and stores the result.
- `authenticate(password)` uses `bcrypt.check_password_hash` to compare a password with the stored hash.

## Project structure

```
client/            React frontend (provided, unchanged)
server/
  app.py           API resources and routes
  config.py        App, database, bcrypt, and API setup
  models.py        User model and UserSchema
  migrations/      Database migrations
  testing/         Test suite
```
