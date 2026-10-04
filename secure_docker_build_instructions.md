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

## Step 4: Create the Base `Dockerfile`

Define your minimal base image (Alpine) and enforce least privilege by setting up a non-root user. 

```powershell
@"
FROM alpine:3.19
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
"@ | Out-File -Encoding utf8 Dockerfile
```

## Step 5: Build the Base Image

Compile your `Dockerfile` into a static, read-only image template.

```powershell
docker build -t my-secure-base:1.0 .
```

> **Note:** You can verify this step by clicking the **Images** tab in the Docker Desktop interface to see `my-secure-base:1.0` listed.

## Step 6: Run the Secure Container (Empty Base)

Launch a live instance of your base image while explicitly dropping all default root kernel capabilities and injecting any required secrets dynamically.

```powershell
docker run -d --name secure-app-instance --cap-drop=ALL -e SECRET_KEY="my_dynamic_secret" my-secure-base:1.0
```

---

## Step 7: Create a Test Application

To actually run an app, create a simple shell script. We use `Set-Content` with standard Ascii encoding so the Linux container doesn't fail due to hidden Windows formatting characters.

```powershell
Set-Content -Path my_app.sh -Value "echo 'Hello from the secure container!'" -Encoding Ascii
```

## Step 8: Update the Dockerfile for the App

Overwrite your `Dockerfile` to include a working directory, copy your application script, and set the command to run it. Crucially, use `--chown` so the non-root user owns the files.

```powershell
@"
FROM alpine:3.19
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Set a dedicated folder for your app
WORKDIR /app

# Copy your app into the image and give ownership to appuser
COPY --chown=appuser:appgroup . .

# Enforce least privilege
USER appuser

# Execute the application
CMD ["sh", "my_app.sh"]
"@ | Out-File -Encoding utf8 Dockerfile
```

## Step 9: Rebuild the Application Image

Package your new application code into a new image version. We tag it as `1.1` to differentiate it from the empty base image.

```powershell
docker build -t my-secure-app:1.1 .
```

## Step 10: Run the Live Application

Run the container in the foreground so you can see its output. The `--rm` flag ensures the container deletes itself cleanly once the script finishes executing.

```powershell
docker run --rm --name live-app --cap-drop=ALL my-secure-app:1.1
```

> **Expected Output:** The terminal should successfully print `Hello from the secure container!` and immediately return you to your PowerShell prompt.