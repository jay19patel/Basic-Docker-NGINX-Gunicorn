# DOCKER - NGINX - GUNICORN

### 1. Step 1:
    - Create One folder (Here, app)
    - In Folder Create Flask / Django File 

    Here:

    ```python
    # file name : wsgi.py
        from flask import Flask
        import socket
        app = Flask(__name__)

        @app.route('/')
        def hello():
            return f" Welcome Here HOST ID :{socket.gethostbyname()}"

        if __name__ == '__main__':
            app.run(debug=True)

    ```
### 2. Step 2:
    - Create/ genrate requirements.txt using 
    ```python
    python -m pip freeze > requirements.txt #create dependency
    python -m pip install -r requirements.txt # install dependency
    ``` 
### 3. Step 3:
    - Create file name : Dockerfile
``` Dockerfile
        #  Docker file 
            FROM python:3.11.3

            WORKDIR /app

            COPY . .

            RUN pip install flask gunicorn
            CMD gunicorn --bind 0.0.0.0:5000 wsgi:app
```
    - Create File in outside of "app" folder : docker-compose.yml

```docker-compose
            version: "3"

        services:
        app:
            build: 
            context : app
            ports:
            - "5000"

        nginx:
            image: nginx:latest
            volumes:
            - ./nginx.conf:/etc/nginx/nginx.conf:ro

            depends_on:
            - app
            
            ports:
            - "80:80"

```
    - Create NGINX file : nginx.conf(all file if big project or mine file )
```nginx
            events {
            worker_connections  1024;
        }

        http {
            server {
                listen       80;

                location / {
                    proxy_pass http://app:5000;
                }

            }
        }

```
### 4. Step 4:
 - docker-compose up : RUN
 - docker-compose down : STOP
 - docker-compose up -d --build --scale app=3 : RUN As GLOBEL WITH 3 CONTAINER
 - app= 3 means 3 workers are working now (3 container /servers)
 - docker ps : WATCH RUNNING IMAGES
 - docker run hello-world   : CHECK ALL ARE GOOD 

- docker run is used to run a single container.
- docker-compose up, on the other hand, is used to start and manage multiple containers as defined in a docker-compose.yml
- every time when we refrese page also change container / server change .

![Screenshot 2023-04-24 222748](https://user-images.githubusercontent.com/107461719/234066108-09a08717-6201-45a8-a1a0-3c60bb62936f.jpg)
## change Server / Container 
![server 2](https://user-images.githubusercontent.com/107461719/234066219-176ec7b2-d853-463c-977f-dd4f6cfdcf2f.jpg)
![server1](https://user-images.githubusercontent.com/107461719/234066275-71df6b14-3569-434c-9893-c4c695ec5e72.jpg)

