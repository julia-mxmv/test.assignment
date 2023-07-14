# test.assignment
So, first of all, you need to install docker. After that, go to the next step

We need to start the container with an app in Kubernetes cluster. As an example we'll take our _ExampleApp_, let it accept on the port 8800

Let's create catalogs and subcatalogs

    
    mkdir quickstart_docker
    mkdir quickstart_docker/application
    mkdir quickstart_docker/docker
    mkdir quickstart_docker/docker/application
Moving next 

    quickstart_docker/ # The whole project cathalog
    ├──application/ # Here will be the code
    └──docker/ # Here will be some stuff for docker
     └──application/ # ... here we will put Dockerfile for the app

---
_It can be all put in the root, but the structure is important_

---

Moving on 
To deploy an app, we're gonna need the app. As an example, we'll use this. But it should be replaced afterwards

In _quickstart_docker/application_ we need to put _application.py_ file 
   
    import http.server
    import socketserver
    PORT = 8000
    Handler = http.server.SimpleHTTPRequestHandler

    httpd = socketserver.TCPServer((&quot;&quot;, PORT), Handler)
    print(&quot;serving at port&quot;, PORT)
    httpd.serve_forever()
The app needs the environment. At least, we'll need python with its OS with all dependencies. All that you can find in the dockerhub.

So, in _quickstart_docker/docker/application_ we put the file with the name _Dockerfile_ with the next contents:

    # Use base image from the registry
    FROM python:3.5
    # Set the working directory to /app
    WORKDIR /app
    # Copy the &#39;application&#39; directory contents into the container at /app
    COPY ./application /app
    # Make port 8000 available to the world outside this container
    EXPOSE 8000
    # Execute &#39;python /app/application.py&#39; when container launches
    CMD [&quot;python&quot;, &quot;/app/application.py&quot;]
The next step. In the Terminal you enter:

    docker build . -f-docker/application/Dockerfile -t exampleapp
Arguments: 
* . - main catalog, contexts of deployment; 
* -f docker/application/Dockerfile - docker-file;
*  -t exampleapp - tagging the image, to easily find it later.

More about compilation for Docker is [here](https://docs.docker.com/engine/reference/builder/.)

And here is the list of images
REPOSITORY | TAG | IMAGE ID| CREATED SIZE| 
:----------|-----|---------|-------------|
exampleapp | latest | 83wse0edc28a | 2 seconds ago | 153MB
python | 3.6 | 05sob8636w3f | 6 weeks ago | 153 MB

Next we put the environment in the repository 
