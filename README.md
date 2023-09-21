# postgresql-postgrest-tutoral

Based on the tutorials Tutorial 0 https://postgrest.org/en/stable/tutorials/tut0.html and Tutorial 1 https://postgrest.org/en/stable/tutorials/tut1.html

I have provided a docker-compose file with all the configuration mentioned in the tutorials.

The sql statements in tutorials.sql are executed when the container starts.

## Run postgrest example

     docker-compose up -d

### Swagger UI

     http://localhost:8080/

### Postgrest

     http://127.0.0.1:3000

## Authentication

### Secret
As described in the tutorials clients authenticate with the API using JSON Web Tokens. First generate a JWT secret:

     # Allow "tr" to process non-utf8 byte sequences
     export LC_CTYPE=C

     # read random bytes and keep only alphanumerics
     < /dev/urandom tr -dc A-Za-z0-9 | head -c32

And set the screat in the docker-compose.yml as PGRST_JWT_SECRET.

### Token
Then go to https://jwt.io/#debugger-io and sign a token.

1. Enter the secret
2. Enter as payload with or without a expiry (epoch)
```
     {
          "role": "todo_user",
          "exp": 1695303750
     }
``````
3. Copy the resulting token and set the token as an environment variable, e.g.

     export TOKEN="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJyb2xlIjoidG9kb191c2VyIn0.vvYLPo24WJxbwEn1J-VEEd3B4dsBESzBlFzlZShKR9M"

## Examples

### Curl

GET

     curl http://localhost:3000/todos

POST

     curl http://localhost:3000/todos -X POST \
          -H "Authorization: Bearer $TOKEN"   \
          -H "Content-Type: application/json" \
          -d '{"task": "learn how to auth"}'

PATCH

     curl http://localhost:3000/todos -X PATCH \
          -H "Authorization: Bearer $TOKEN"    \
          -H "Content-Type: application/json"  \
          -d '{"done": true}'

## Stop postgrest example

     docker-compose stop

     or

     docker-compose down -v
     