### Step-by-Step Guide to Setting Up an Amazon RDS Database, Amazon ECR, ECS Cluster, and Deploying a Spring Boot Application

---

### **1. Create a Database in Amazon RDS**

Amazon RDS (Relational Database Service) is a managed database service that helps you set up, operate, and scale a relational database in the cloud.

#### Steps to Create the Database:

1. **Login to AWS Console**: Go to [AWS Management Console](https://aws.amazon.com/console/).
2. **Navigate to RDS**:  
   `Services` → `RDS` → Click on **"Create database"**.

   ![Image](/Images/01.png)

3. **Choose Database Creation Method**:
   - Select **Standard Create**.  
     This option provides complete control over database configurations.
4. **Engine Options**:

   - Choose **MySQL** as the database engine.  
     MySQL is a popular open-source database that is secure, scalable, and reliable.

   ![Image](/Images/02.png)

5. **Template and Deployment Options**:

   - **Template**: Choose **Production** for higher performance and reliability.
   - **Deployment Option**: Choose **Single DB instance**.  
     This is the simplest setup with one database instance.

6. **Define Database Details**:

   - Enter a **DB cluster identifier** (use your name for easy identification).
   - **Credentials Settings**:
     - Set the **Master Username** (label it as `SPRING_DATASOURCE_USERNAME`).
     - Set a **Password** (label it as `SPRING_DATASOURCE_PASSWORD`).

7. **Additional Configuration**:

   - Under **Additional Configuration**, set the **Initial Database Name**.

8. **Save and Create the Database**:
   - Once the database is created, open it. You will find:
     - **Endpoint** (label it as `SPRING_DATASOURCE_URL`): This is the database connection string.
     - **Port**: Default is 3306.
     - **Security Groups**: These help control access to the database.

---

### **2. Create a Repository in Amazon ECR**

Amazon ECR (Elastic Container Registry) is a fully managed container image registry used to store and manage Docker images.

#### Why Use ECR?

It simplifies storing, managing, and deploying container images for ECS (Elastic Container Service) clusters.

#### Steps to Create a Repository:

1. **Navigate to ECR**:  
   `Services` → `Elastic Container Registry (ECR)` → Click on **"Create repository"**.

   ![Image](/Images/04.png)

2. **Fill Repository Details**:

   - **Repository Name**: Enter your desired repository name.
   - **Tag Mutability**: Choose **Mutable** (allows updates to image tags).
   - **Encryption**: Use the default **AES-256** encryption.

3. **Save and View Repository**:
   - Once created, open the repository.
   - Note down the **Repository URI** (label it as `ECR_REPOSITORY_URI`).

---

### **3. Create a Cluster in Amazon ECS**

Amazon ECS (Elastic Container Service) is a fully managed container orchestration service that runs Docker containers.

#### Steps to Create the Cluster:

1. **Navigate to ECS**:  
   `Services` → `Elastic Container Service (ECS)` → Click on **"Create cluster"**.

   ![Image](/Images/05.png)

2. **Define Cluster Details**:
   - **Cluster Name**: Enter your cluster name (label it as `ECS_CLUSTER_NAME`).
   - Save and create the cluster.

---

### **4. Create a Task Definition in Amazon ECS**

Task definitions describe how containers run within the ECS cluster.

#### Steps to Create the Task:

1. **Navigate to Task Definitions**:  
   `Services` → `ECS` → `Task Definitions` → Click on **"Create new task definition"**.

   ![Image](/Images/06.png)

2. **Set IAM Role**:

   - Go to the **IAM Console** and create a task role:
     - Attach the **AmazonECSTaskExecutionRolePolicy** permission.

   ![Image](/Images/08.png)

3. **Define Container Settings**:

   - **Container Name**: Enter a name.
   - **Image URI**: Use the value of `ECR_REPOSITORY_URI`.
   - **Container Port**: Set it (e.g., 8080).
   - **Environment Variables**:
     - `SPRING_DATASOURCE_USERNAME`: Your master username.
     - `SPRING_DATASOURCE_PASSWORD`: Your database password.
     - `SPRING_DATASOURCE_URL`:
       ```
       jdbc:mysql://database-endpoint:3306/initial-database-name?createDatabaseIfNotExist=true
       ```

4. **Save the Task Definition**:
   - You can view this task in the **Task Definition Dashboard**.

---

### **5. Create a Service in Amazon ECS**

ECS services help run and maintain a set of tasks.

#### Steps to Create the Service:

1. **Navigate to Your Cluster**:  
   `Services` → `ECS` → Select your created cluster.

   ![Image](/Images/09.png)

2. **Create the Service**:

   - **Service Name**: Enter the name (label it as `ECS_SERVICE_NAME`).
   - **Task Definition**: Choose the created task definition.
   - **Load Balancer**:
     - Select **Application Load Balancer**.
     - Create a new load balancer and listener.
     - Create a new target group for the container.

   ![Image](/Images/10.png)  
   ![Image](/Images/11.png)  
   ![Image](/Images/12.png)

3. **Save the Service**.

---

### **Deploying a Spring Boot Application to AWS ECS Using Docker, GitHub Actions, and ECR**

This guide explains how to containerize a Spring Boot application, push it to Amazon ECR, and deploy it to ECS using GitHub Actions. It also explains how to configure your GitHub repository to automate this process.

---

### **Step 1: Create a Dockerfile**

A `Dockerfile` is used to containerize your Spring Boot application. Place this file in the root folder of your Spring Boot project.

```dockerfile

# Copy Maven wrapper to container
COPY mvnw .
# Copy Maven wrapper's .mvn folder
COPY .mvn .mvn
# Copy the project's pom.xml for dependency management
COPY pom.xml .

# Install all Maven dependencies offline for faster builds
RUN ./mvnw dependency:go-offline -B

# Copy the source code of the Spring Boot application
COPY src src
# Build the Spring Boot application, skipping tests
RUN ./mvnw package -DskipTests

# Use a minimal Amazon Corretto (Java 21) image as the base for runtime
FROM --platform=linux/amd64 amazoncorretto:21-alpine-jdk

# Set the working directory inside the container
WORKDIR /app

# Copy the built WAR file from the builder stage to the runtime stage
COPY --from=builder /app/target/shapefit-0.0.2-SNAPSHOT.war /app/shapefit-0.0.2-SNAPSHOT.war

# Specify the command to run the application
CMD ["java", "-jar", "/app/shapefit-0.0.2-SNAPSHOT.war"]

```

**Explanation of Key Commands:**

1. **COPY mvnw, .mvn, and pom.xml**: These files are required to install project dependencies.
2. **RUN ./mvnw dependency:go-offline**: Ensures Maven dependencies are cached for offline builds.
3. **COPY src src**: Copies the source code to the container.
4. **RUN ./mvnw package -DskipTests**: Builds the application without running tests.
5. **FROM amazoncorretto:21-alpine-jdk**: Uses a lightweight Java runtime environment.
6. **WORKDIR /app**: Sets the working directory in the container.
7. **CMD**: Runs the Spring Boot application as a JAR file.

---

### **Step 2: Configure GitHub Actions Workflow**

GitHub Actions automates the deployment process by building, pushing the Docker image, and updating ECS tasks.

#### Create a Workflow File

1. In your GitHub repository, create the file:  
   `.github/workflows/deploy-dev.yml`

#### Add the Following Workflow Code

```yml
name: Deploy Dev Env

on:
  workflow_dispatch: # Allows manual workflow triggering

jobs:
  build:
    runs-on: ubuntu-latest # Runs on the latest Ubuntu instance

    env:
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }} # AWS Access Key
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }} # AWS Secret Key
      AWS_REGION: ${{ secrets.AWS_REGION }} # AWS Region

    steps:
      - name: Checkout code
        uses: actions/checkout@v4 # Pulls the source code from the repository

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3 # Sets up a Docker builder for multi-platform builds

      - name: Login to Amazon ECR
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v2 # Logs into Amazon ECR

      - name: Build and Push Docker image
        id: build-image
        run: |
          docker build -t ${{ secrets.ECR_REPOSITORY_URI }}:${{ github.sha }} . # Builds Docker image
          docker push ${{ secrets.ECR_REPOSITORY_URI }}:${{ github.sha }} # Pushes Docker image to ECR

  deploy-dev:
    needs: build # Runs after the build job
    runs-on: ubuntu-latest

    env:
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
      AWS_REGION: ${{ secrets.AWS_REGION }}

    steps:
      - name: Download task definition
        run: |
          aws ecs describe-task-definition --task-definition ${{ secrets.DEV_ECS_TASK_DEFINITION }} --query taskDefinition > old-task-definition.json # Downloads the current ECS task definition

      - name: Prepare dev task definition
        id: task-def
        uses: aws-actions/amazon-ecs-render-task-definition@v1
        with:
          task-definition: old-task-definition.json # Modifies the task definition
          container-name: dev-shapefit-app # Name of the container
          image: ${{ secrets.ECR_REPOSITORY_URI }}:${{ github.sha }} # Updates the image URI in the task definition

      - name: Deploy to ECS
        uses: aws-actions/amazon-ecs-deploy-task-definition@v1
        with:
          task-definition: ${{ steps.task-def.outputs.task-definition }} # Uses the updated task definition
          service: ${{ secrets.DEV_ECS_SERVICE_NAME }} # ECS service name
          cluster: ${{ secrets.DEV_ECS_CLUSTER_NAME }} # ECS cluster name

      - name: Retrieve and Deregister Previous Task Definition Revision
        id: deregister-task
        run: |
          revisions=$(aws ecs list-task-definitions --family-prefix ${{ secrets.DEV_ECS_TASK_DEFINITION }} --sort DESC --max-items 2 --query "taskDefinitionArns" --output text) # Lists the task revisions
          previous_revision=$(echo "$revisions" | awk '{print $2}') # Retrieves the previous task definition revision

          if [ -n "$previous_revision" ]; then
            aws ecs deregister-task-definition --task-definition $previous_revision > /dev/null 2>&1 # Deregisters the old task definition
          else
            echo "No previous task definition revision found."
          fi
```

---

### **Step 3: Configure GitHub Secrets**

1. **Navigate to GitHub Secrets**:  
   In your repository, go to `Settings` → `Secrets and variables` → `Actions`.

2. **Create the Following Secrets**:
   - `AWS_ACCESS_KEY_ID`: Your AWS access key.
   - `AWS_SECRET_ACCESS_KEY`: Your AWS secret key.
   - `AWS_REGION`: The AWS region (e.g., `us-east-1`).
   - `ECR_REPOSITORY_URI`: Your ECR repository URI.
   - `ECS_TASK_DEFINITION`: The task definition name for development.
   - `ECS_SERVICE_NAME`: The ECS service name.
   - `ECS_CLUSTER_NAME`: The ECS cluster name.

---

### **Step 4: Trigger the Workflow**

1. **Navigate to GitHub Actions**:  
   Go to `Actions` in your GitHub repository.

2. **Run the Workflow**:  
   Select **Deploy Dev Env** → Click **Run workflow**.

![Images](/Images/15.png)

3. **Verify the Deployment**:  
   After the workflow completes, go to the ECS console and find the public IP of the service. Use it to access your API endpoints.

   Example:

   ```
   http://65.0.120.222:8080/api/users
   ```

---

### **Conclusion**

By following this guide, you:

- Built and containerized your Spring Boot application using Docker.
- Pushed the Docker image to Amazon ECR.
- Deployed the application to Amazon ECS using GitHub Actions.
- Successfully accessed your API endpoints.

🎉 **Congratulations! Your Spring Boot application is live in AWS ECS.** 🎉
