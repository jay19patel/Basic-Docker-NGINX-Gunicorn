FROM python:3.11.3

WORKDIR /app

COPY . .

RUN pip install flask gunicorn
CMD gunicorn --bind 0.0.0.0:5000 wsgi:app