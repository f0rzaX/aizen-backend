# Aizen Backend

This project is the backend for a web application that allows users to securely upload and manage images with optional AI-powered image analysis. It includes user authentication, image storage using Amazon S3, and background task processing with Celery.

## Table of Contents

1. [Project Summary](#project-summary)
2. [Technology Stack](#technology-stack)
3. [Features](#features)
4. [System Architecture](#system-architecture)
5. [API Endpoints](#api-endpoints)
6. [Database Design](#database-design)
7. [Security](#security)
8. [How to Run the Project](#how-to-run-the-project)
9. [Deployment](#deployment)

## Project Summary

The backend provides APIs for user authentication, image upload and retrieval, and integrates with external services such as Amazon S3 for file storage and Redis with Celery for asynchronous task processing.

## Technology Stack

-   **Backend Framework:** Django
-   **API Framework:** Django REST Framework (DRF)
-   **Database:** PostgreSQL
-   **File Storage:** Amazon S3
-   **Background Tasks:** Celery with Redis
-   **Authentication:** JWT (using Simple JWT)

## Features

-   **User Authentication:** Register, login, logout with JWT authentication.
-   **Image Management:** Upload images to S3, retrieve and list images.
-   **AI Image Analysis (Optional):** Analyze images using AI services like ChatGPT (implemented as a background task).

## System Architecture

-   **Django REST Framework:** Handles API requests and responses.
-   **PostgreSQL:** Stores user data and image metadata.
-   **Amazon S3:** Stores uploaded image files.
-   **Celery with Redis:** Manages background processing tasks, such as image analysis.
-   **CORS Handling:** Configured to allow API access from different origins.

## API Endpoints

-   **Authentication:**
    -   `POST /api/auth/register/` - Register a new user.
    -   `POST /api/auth/login/` - Obtain JWT token.
    -   `POST /api/auth/token/refresh/` - Refresh JWT token.
    -   `POST /api/auth/logout/` - Blacklist the refresh token.
-   **User Information:**
    -   `GET /api/user/info/` - Retrieve information about the authenticated user.
-   **Images:**
    -   `POST /api/images/upload/` - Upload a new image.
    -   `GET /api/images/` - List all images for the authenticated user.
    -   `GET /api/images/status/<id>/` - Retrieve the status of a specific image.
    -   `GET /api/images/<id>/` - Retrieve details for a specific image.
    -   `DELETE /api/images/<id>/` - Delete a specific image.

## Database Design

-   **Users:** Stores user credentials and profile information.
-   **Images:** Stores metadata for each uploaded image (e.g., file name, upload date) linked to the user.

## Security

-   **HTTPS:** Secure communication over HTTPS.
-   **JWT Authentication:** Secure endpoints using JWT tokens.
-   **Password Hashing:** Passwords are hashed and validated using Django's built-in validators.
-   **AWS S3 Access Control:** S3 bucket configured to allow access only through signed URLs.
-   **Environment Variables:** Sensitive information (e.g., secret keys, AWS credentials) stored in environment variables.

## How to Run the Project

1. **Clone the Repository:**

    ```bash
    git clone https://github.com/f0rzaX/aizen-backend
    cd aizen-backend
    ```

2. **Set Up Virtual Environment:**

    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
    ```

3. **Install Dependencies:**

    ```bash
    pip install -r requirements.txt
    ```

4. **Configure Environment Variables:** Create a `.env` file in the project root and set the required variables:

    ```env
    DEBUG=True
    AWS_ACCESS_KEY_ID=your-aws-access-key
    AWS_SECRET_ACCESS_KEY=your-aws-secret-key
    AWS_STORAGE_BUCKET_NAME=your-s3-bucket-name
    AWS_S3_REGION_NAME=your-s3-region
    CELERY_BROKER_URL=your-redis-url
    CORS_ALLOW_ALL_ORIGINS=True
    ```

5. **Run Database Migrations:**

    ```bash
    python manage.py migrate
    ```

6. **Start the Development Server:**

    ```bash
    python manage.py runserver
    ```

7. **Start Celery Worker:**
    ```bash
    celery -A aizen_backend worker --loglevel=info
    ```

### Deployment

-   **Web Server:** Deploy using a WSGI server like Gunicorn behind a reverse proxy like Nginx.
-   **Environment Configuration:** Ensure all environment variables are set in the deployment environment.
-   **Static and Media Files:** Configure static and media file handling for production (e.g., using S3 for media files).
