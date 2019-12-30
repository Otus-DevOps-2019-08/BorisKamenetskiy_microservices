FROM python:3.6.0-alpine

WORKDIR /app
ADD . /app

RUN apk upgrade
RUN apk update && apk add --virtual build-dependencies build-base gcc wget git
RUN pip install -r /app/requirements.txt

ENV POST_DATABASE_HOST post_db
ENV POST_DATABASE posts

CMD ["python3", "post_app.py"]

