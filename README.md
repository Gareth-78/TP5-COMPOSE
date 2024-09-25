# TP5-COMPOSE
## app.py
```
from flask import Flask
from redis import Redis

app = Flask(__name__)
redis = Redis(host='redis-container', port=6379)

@app.route('/')
def hello():
    redis.incr('hits')
    return ' - - - This basic web page has been viewed {} time(s) - - -'.format(redis.get('hits'))


if __name__ == "__main__":
    app.run(host="0.0.0.0", debug=True)
```
## requirements.txt
```
flask
redis
```
## Dockerfile
```
FROM python:3.6
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD python app.py
```
## docker-compose.yml
```
version: '3'
services:
    web:
        build: ./app
        ports:
            - "5000:5000"
        volumes:
            - ./app:/app
        depends_on:
            - redis-container

    redis-container:
        image: redis
```

## tree
![Capture d’écran du 2024-09-25 16-28-43](https://github.com/user-attachments/assets/689566d0-5580-4ffe-833d-afba0da39955)

