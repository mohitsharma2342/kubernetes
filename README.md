# kubernetes


How To create Docker Image

To build the image we can run:

$ docker build . --tag demo
Then we can test it:

$ docker run -it -p8080:8080 demo:latest
