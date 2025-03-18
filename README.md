# Homie - Your Virtual Home Assistant

Homie is a web-based virtual home assistant application built with React (frontend) and Django (backend). This application provides an interactive home tour experience where users can live stream their view and engage in real-time conversation with an AI-powered real estate assistant. The system uses multimodal models to analyze visual and audio input, providing comprehensive property insights and information about location, neighborhood, and school districts.

## Prerequisites

Before starting, make sure you have the following installed:
- Python 3.8 or higher
- Node.js 18 or higher
- npm (comes with Node.js)
- Git
- FFmpeg (required for audio processing)

## Backend Setup

### 1. Clone the Repository
```bash
git clone <repository-url>
cd <repository-name>
```

### 2. Backend Environment Setup

#### Windows
```bash
cd backend
python -m venv django_env
django_env\Scripts\activate
pip install -r requirements.txt
```

#### macOS/Linux
```bash
cd backend
python3 -m venv django_env
source django_env/bin/activate
pip install -r requirements.txt
```

### 3. Environment Variables

#### Backend Environment Variables
Create a `.env` file in the `/backend` directory with the following variables:

```plaintext
# API Keys
GEMINI_API_KEY=your_gemini_api_key_here    # Required for AI features

# Authentication
DJANGO_SECRET_KEY=your_django_secret_key    # Long random string for Django security

# Security Settings
DEBUG=False                                 # Set to True for development
ALLOWED_HOSTS=localhost,127.0.0.1           # Comma-separated list of allowed hosts
CORS_ALLOWED_ORIGINS=http://localhost:5173  # Frontend URL for CORS

# Rate Limiting (Optional - defaults shown)
RATE_LIMIT_AUTHENTICATED=100                # Requests per minute for authenticated users
RATE_LIMIT_ANONYMOUS=30                     # Requests per minute for anonymous users
RATE_LIMIT_DAILY=1000                      # Daily request limit
```

#### Frontend Environment Variables
Create a `.env` file in the `/frontend` directory:

```plaintext
# API Configuration
VITE_API_URL=http://localhost:8000          # Backend API URL
VITE_WS_URL=ws://localhost:8000             # Backend WebSocket URL (if used)

# Security
VITE_USE_HTTPS=true                         # Enable HTTPS for local development
```

#### Environment Variable Details

1. **Backend Variables:**
   - `GEMINI_API_KEY`: Get from Google AI Studio
   - `DJANGO_SECRET_KEY`: Generate using `python -c 'from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())'`
   - `DEBUG`: Set to `False` in production
   - `ALLOWED_HOSTS`: Add your domain in production
   - `CORS_ALLOWED_ORIGINS`: Add your frontend URL

2. **Frontend Variables:**
   - `VITE_API_URL`: Backend API URL (use HTTPS in production)
   - `VITE_WS_URL`: WebSocket URL (if implementing real-time features)
   - `VITE_USE_HTTPS`: Enable for secure camera access

#### Security Notes:
- Never commit `.env` files to version control
- Use different values for development and production
- Regularly rotate sensitive credentials
- In production, use a secure secret management service

### 4. Database Setup
```bash
python manage.py migrate
```

### 5. Running the Backend Server

#### Windows
```bash
django_env\Scripts\activate
python manage.py runserver
```

#### macOS/Linux
```bash
source django_env/bin/activate
python manage.py runserver
```

The backend server will start at `http://localhost:8000`

## Frontend Setup

### 1. Install Dependencies
```bash
cd frontend
npm install
```

### 2. Environment Setup
Create a `.env` file in the `/frontend` directory:
```plaintext
VITE_API_URL=http://localhost:8000
VITE_WS_URL=ws://localhost:8000
```

### 3. SSL Setup for Local Development
For secure camera access, you'll need SSL certificates:

#### Windows/macOS/Linux
```bash
cd frontend
mkdir ssl
cd ssl
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365 -nodes
```

When prompted, you can press Enter for all fields except Common Name (CN) - use "localhost".

### 4. Running the Frontend Development Server
```bash
npm run dev
```

The frontend development server will start at `https://localhost:5173`

## Security Considerations

1. **Environment Variables**
   - Never commit `.env` files to version control
   - Use strong, unique passwords for admin credentials
   - Regularly rotate API keys and credentials

2. **API Security**
   - All API endpoints are JWT-protected
   - Access tokens expire after 1 hour
   - Refresh tokens expire after 24 hours and rotate with each use
   - Rate limiting implemented at multiple levels:
     * Global limits:
       - Authenticated users: 100 requests/minute
       - Anonymous users: 30 requests/minute
       - Daily limit: 1000 requests/day for all users
     * Per-endpoint limits:
       - Frame Analysis: 60/min (auth), 10/min (anon)
       - Chat: 120/min (auth), 20/min (anon)
       - Audio Transcription: 30/min (auth), 5/min (anon)
   - HTTPS required in production
   - CORS and CSRF protection enabled

3. **Data Privacy**
   - Audio and video streams are processed in real-time and not stored
   - Temporary files are automatically cleaned up
   - User sessions are properly managed and terminated

## Dependencies

### Backend Dependencies
- Django 5.0.2
- Django REST Framework 3.14.0
- Django CORS Headers 4.3.1
- Google Generative AI 0.3.2
- Pillow 10.2.0
- python-dotenv 1.0.1
- SpeechRecognition 3.10.1
- PyAudio 0.2.14
- djangorestframework-simplejwt 5.3.0
- FFmpeg (system dependency)

### Frontend Dependencies
- React 18.2.0
- Material-UI (MUI) 5.15.10
- React Webcam 7.2.0
- Vite 5.1.0
- JWT Decode 4.0.0

## Troubleshooting

1. **SSL Certificate Issues**
   - If you see certificate warnings in the browser, add the self-signed certificate to your system's trusted certificates
   - For development, you can click "Advanced" and "Proceed anyway" in your browser

2. **Audio Processing Issues**
   - Ensure FFmpeg is installed and accessible from the command line
   - Windows: Download FFmpeg from official website and add to PATH
   - macOS: Install via `brew install ffmpeg`
   - Linux: Install via `sudo apt-get install ffmpeg`

3. **Camera Access Issues**
   - Ensure you're using HTTPS (required for camera access)
   - Grant camera permissions in your browser
   - Try a different browser if issues persist

4. **Authentication Issues**
   - Clear browser cookies and local storage if login issues occur
   - Ensure environment variables are properly set
   - Check backend logs for detailed error messages

## Development

- Backend API: `http://localhost:8000`
- Frontend development server: `https://localhost:5173`
- Both servers must run simultaneously
- Use the Django admin interface at `http://localhost:8000/admin` for user management

## Production Deployment

Additional steps for production deployment:

1. Configure proper SSL certificates
2. Set up proper user authentication system
3. Implement rate limiting
4. Configure CORS settings appropriately
5. Set up proper database (PostgreSQL recommended)
6. Configure proper static file serving
7. Set up proper logging
8. Implement proper error handling and monitoring

## Deployment Instructions

This section outlines the general steps for deploying updates to your web application (React frontend and Django backend).  The specific commands might need slight adjustments based on your server configuration.

**General Steps (Regardless of Platform):**

1.  **Build your React frontend:**

    * Navigate to your frontend directory in your terminal:

        ```bash
        cd frontend
        ```

    * Build your production-ready React application. The command for this usually depends on your React setup (e.g., Create React App, Next.js, etc.). Common commands are:

        * **Create React App:**

            ```bash
            npm run build
            # or
            yarn build
            ```

        * **Next.js:**

            ```bash
            npm run build
            # or
            yarn build
            ```

        This will typically create a `build` folder containing your static HTML, CSS, and JavaScript files.

2.  **Collect Django static files:**

    * Navigate to your Django project's root directory (the one containing `manage.py`):

        ```bash
        cd .. # Assuming you were in the frontend directory
        ```

    * Collect all static files from your Django apps into your `staticfiles` directory (make sure this directory is configured in your `settings.py`):

        ```bash
        python manage.py collectstatic
        ```

        You'll likely be asked to confirm overwriting existing files. Type `yes` if you want to proceed.

3.  **Make Django migrations:**

    * Apply any new database migrations you've created since your last deployment:

        ```bash
        python manage.py makemigrations
        python manage.py migrate
        ```

4.  **Restart your Django application server:**

    * If you're running Django behind a web server like Nginx or Apache, you might need to restart the web server or the specific application server processes (like Gunicorn).

    * **Nginx:**

        ```bash
        sudo vim /etc/nginx/sites-available/dev.myhomie.homes # confirm if the setup is correct
        sudo nginx -t # test config
        sudo systemctl restart nginx
        ```

5.  **Identify and Restart the Gunicorn Service:**

    * You need to identify the correct systemd service name for your Gunicorn process.

    * List the services in `/etc/systemd/system/` and find the one related to gunicorn:

        ```bash
        ls /etc/systemd/system/
        ```

    * Check the status of running services and filter for Gunicorn:

        ```bash
        sudo systemctl status | grep gunicorn
        ```

        This will show you a list of services and their current status.  The output will help you confirm the exact name of the Gunicorn service (e.g., `gunicorn.service`).

    * Restart the Gunicorn service:

        ```bash
        sudo systemctl restart gunicorn.service
        ```

    * Check the status of the Gunicorn service to confirm it restarted successfully:
        ```bash
        sudo systemctl status gunicorn.service
        ```

