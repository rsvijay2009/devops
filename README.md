# <span style="color:HotPink">**Steps to dockerize a sample project and deployed using kubernetes**</span>

## <span style="color:#17a2b8">**Step 1: Create Your Static HTML Site**<span>

### 1. Create a new directory for your project and navigate into it28a745
```bash
    mkdir simple-html-site && cd simple-html-site
```
### 2. Create an index.html file with your content

```html
    <!-- index.html -->
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Simple HTML Site</title>
    </head>
    <body>
        <h1>Hello, Kubernetes!</h1>
        <p>This is a simple static HTML site served by Nginx.</p>
    </body>
    </html>
```

## <span style="color:#17a2b8">**Step 2: Dockerize Your Site with Nginx**</span>
### 1. Install docker or dockerhub in your local machine and start the service

```dockerfile
    # Use an official NGINX image as a base image
    FROM nginx:latest

    # Copy static website files to the NGINX document root
    COPY . /usr/share/nginx/html

    # Expose port 80 to allow external access
    EXPOSE 80

    # Command to start NGINX when the container runs
    CMD ["nginx", "-g", "daemon off;"]
```

### 2. Build your Docker image:

```
    docker build -t your-dockerhub-username/simple-html-site:latest .
```
Replace your-dockerhub-username with your actual Docker Hub username

## <span style="color:#17a2b8">**Step 3: Push the Image to Docker Hub**</span>

### 1. Log in to Docker Hub:
```
    docker login
```

### 2. Push your Docker image to Docker Hub:

```
    docker push <your-dockerhub-username>/simple-html-site:latest
```

## <span style="color:#17a2b8">**Step 4: Deploy Your Site on Kubernetes**<span>

### 1. Create a deployment.yaml file for Kubernetes:
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: simple-html-site
spec:
  selector:
    matchLabels:
      app: simple-html-site
  replicas: 1
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
  template:
    metadata:
      labels:
        app: simple-html-site
    spec:
      containers:
      - image: your-dockerhub-username>/simple-html-site:latest
        imagePullPolicy: Always
        name: simple-html-site
        ports:
        - containerPort: 80
```

### 2. Create a service.yaml file to expose your deployment:

```yml
apiVersion: v1
kind: Service
metadata:
  name: docker-test-service
spec:
  type: NodePort
  ports:
  - port: 80
  selector:
    app: docker-test-site
```

### 3. Apply your Kubernetes configurations:

```
    kubectl apply -f deployment.yaml
    kubectl apply -f service.yaml
```

### 4. Verify your pod status by running the below command

```
    kubectl get pods
```


## <span style="color:#17a2b8">**Step 5: Verify your website by hitting the </span><a href="http://localhost:80" style="color: green; text-decoration: underline;text-decoration-style: dotted;">http://localhost:80**</a><span>


--

Author : $\textcolor{HotPink}{Vijayakumar}$ created on 13-02-2024
