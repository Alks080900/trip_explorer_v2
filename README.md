# Trip Explorer Web App - Version 2

This version of trip explorer web app is powered by React as frontend and Spring Boot as it's backend, it is intentionally built to run it via
Dockerfile and docker-compose.yml in AWS. You may refer to this link of its version 1 https://github.com/Alks080900/trip_explorer_v1.

## Prerequisites
- Node.js and npm
- Maven
- Docker and Docker Compose
- EC2 (Linux Ubuntu)
- S3 Bucket
- AWS CLI (for managing EC2 instances via SSH key connection)

## Getting Started

### Frontend
Note: Make sure before building the frontend modifications for .env (REACT_APP_API_URL) has been change.
1. Navigate to the `frontend` folder:
   ```bash
   cd frontend
   ```
2. Install the required dependencies:
   ```bash
   npm install
   ```
3. Build the production-ready files:
   ```bash
   npm run build
   ```
4. Copy the build files to your S3 bucket:
   - Log in to your AWS account and navigate to the S3 service.
   - Create or select an S3 bucket.
   - Upload the contents of the `build` folder to your S3 bucket.

5. Enable static website hosting on the S3 bucket:
   - Go to the "Properties" tab of the bucket.
   - Enable "Static website hosting" and set the index document to `index.html`.
   - Set `index.html` as an error page to avoid 404 not found during web refresh.

6. Set up CORS configuration under the "Permissions" tab with the following:
   ```json
   [
       {
           "AllowedHeaders": [
               "*"
           ],
           "AllowedMethods": [
               "GET",
               "POST",
               "PUT",
               "DELETE",
               "HEAD"
           ],
           "AllowedOrigins": [
               "http://<s3_url>"
           ],
           "ExposeHeaders": []
       }
   ]
   ```

### Backend
Note: Add the S3 bukcet URL under the .allowedOrigins of CorsConfig.java before building and transferring to EC2
1. Modify the `application.properties` file in the backend.
2. Build the Spring Boot application:
   ```bash
   mvn clean package
   ```
3. Transfer the JAR file to the EC2 instance:
   ```bash
   scp -i <ssh_key_name>.pem target/backend-0.0.1-SNAPSHOT.jar <name>@<ec2_public_ip>:/home/path/
   ```

### Docker Setup
1. Transfer the `Dockerfile` and `docker-compose.yml` to the EC2 instance:
    - The Dockerfile and docker-compose.yml is inside the backend folder.
   ```bash
   scp -i <ssh_key_name>.pem Dockerfile <name>@<ec2_public_ip>:/home/path
   scp -i <ssh_key_name>.pem docker-compose.yml <name>@<ec2_public_ip>:/home/path
   ```
2. Ensure Docker services are installed on the EC2 instance.
   - If not, install Docker using:
     ```bash
     sudo apt-get update
     sudo apt-get install docker.io
     sudo systemctl start docker
     sudo systemctl enable docker
     ```

3. Start the application using Docker Compose:
   ```bash
   docker compose up --build
   ```

## Deployment Notes
- **Frontend**: Hosted on AWS S3 with static website hosting.
- **Backend**: Runs on an AWS EC2 instance with Docker containers.


