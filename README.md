# iBILLBOARD API

## Local development

### Building up the app

build your docker containers in root folder of the project

```
    docker-compose up -d --build
```

install composer dependencies

```
    docker-compose exec app composer install
```

application is waiting fo requests on url (port may differ, check `/docker-compose.yml` in case of problems)

    http://localhost:8081/

### Configuration

rewrite default config by creating `config-local.json` file in `/app/config`. Use `/config-local.json.dist` as a template.

```
    cp config-local.json.dist ./app/config/config-local.json
```

## Deployment to production

- set app.show_error_details to false in config-local.json file in production
- redis must not be set up in docker container on server. Failed container would cause data loss. Configure connection
to redis in configuration file accordingly.

## API

in local development, the application is receiving request on url `http://localhost:8081/`.

- the API documentation can be managed using OpenAPI, when the application grows
- use i.e. Postman application to send requests

### GET "/count"

returns value of "count" key in redis

### POST "/track"

receives "application/json" requests and stores content in `/storage/tracking-file`.
If "count" parameter is present, it increments value of "count" key in redis.

The route simulates strictly typed/organized json.

- extra unexpected parameters will cause Err 400
- missing required parameters will cause Err 400

#### Example json string - all supported parameters 

```
{
    "id": 22, 
    "count": 2,
    "requiredString": "required-string",
    "optionalStatistic": {
        "name": "name",
        "value": 250
    }
}
```

#### Example json string - required parameters only

```
{
    "id": 22, 
    "requiredString": "required-string",
}
```
