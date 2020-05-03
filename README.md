# Sample chatbot with rasa stack

It uses the rasa stack (rasa core/nlu/actions) to implement a simple bot which responds user messages.

## Run on local machine

#### Using docker
Both action server and rasa-core runs as separate processes in the same container
```
docker build -t rasa-ibot .
docker run -it --rm -p 8080:8080 -e PORT=8080 rasa-ibot
```
It starts a webserver with rest api and listens for messages at localhost:5005

#### Test over REST api

```bash
curl --request POST \
  --url http://localhost:8080/webhooks/rest/webhook \
  --header 'content-type: application/json' \
  --data '{
    "message": "Hi"
  }'
```
**Response**
```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 59
Access-Control-Allow-Origin: *

[{
  "recipient_id": "default",
  "text": "Hi, how is it going?"
}]
```

#### Run using docker compose
Optionally to run the actions server in separate container start the services using docker-compose. The action server runs on http://action_server:5055/webhook (docker's internal network). The rasa-core services uses the config/endpoints.local.yml to find this actions server

```
docker-compose up
```
#### Train Rasa model
The repository already contains a trained Rasa model at models directory. To retrain the model you can run:
```bash
docker run --rm --volume $(pwd):/app \
          --workdir /app rasa-chatbot \
          rasa train --config config/config.yml
```

## Deploy to GCP

```bash
```

## Deploy to Heroku

```bash
```

## Integration with Web
rasa supports integration with multiple channels. Like exposing the REST api over http

