# Secure Docker Build Instructions

This guide provides step-by-step instructions to create a secure Dockerfile, build an image, and run it as an isolated container using Docker Desktop and Windows PowerShell.

## Step 1: Open the Terminal
Launch the Docker Desktop application and open the integrated terminal tab (which uses Windows PowerShell).

## Step 2: Create the Project Directory
Create a dedicated folder for your image and navigate into it so all your files stay organized.

```powershell
mkdir my-secure-project
cd my-secure-project
```

## Step 3: Create the `.dockerignore` File
Block sensitive local files and build artifacts from being copied into your image. Run this command to create the file cleanly with standard UTF-8 encoding:

```powershell
@"
.git
.env
node_modules
"@ | Out-File -Encoding utf8 .dockerignore
```

## Step 4: Create the `Dockerfile`
Define your minimal base image (Alpine) and enforce least privilege by setting up a non-root user. Run this command to generate the `Dockerfile`:

```powershell
@"
FROM alpine:3.19
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
"@ | Out-File -Encoding utf8 Dockerfile
```

## Step 5: Build the Image
Compile your `Dockerfile` into a static, read-only image template. 

```powershell
docker build -t my-secure-base:1.0 .
```
> **Note:** You can verify this step by clicking the **Images** tab in the Docker Desktop interface to see `my-secure-base:1.0` listed.

## Step 6: Run the Secure Container
Launch a live instance of your image while explicitly dropping all default root kernel capabilities and injecting any required secrets dynamically.

```powershell
docker run -d --name secure-app-instance --cap-drop=ALL -e SECRET_KEY="my_dynamic_secret" my-secure-base:1.0
```
> **Note:** You can monitor this running instance, view its logs, or stop it by checking the **Containers** tab in the Docker Desktop interface.