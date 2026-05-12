# Meal Buddy Deployment Guide

This guide covers deploying FinalMealmate to various platforms.

## Platform-Specific Guides

### 1. PythonAnywhere

#### Steps:
1. Upload your code to PythonAnywhere
2. Create a web app with Python 3.11+
3. Set up a virtual environment
4. Configure WSGI file
5. Add static files mapping

#### Configuration:
```python
# Update settings.py
ALLOWED_HOSTS = ['yourusername.pythonanywhere.com']
DEBUG = False
```

#### Static Files:
```
URL: /static/
Directory: /home/yourusername/FinalMealmate/staticfiles/
```

### 2. Heroku

#### Procfile:
```
web: gunicorn meal_buddy.wsgi --log-file -
release: python manage.py migrate
```

#### runtime.txt:
```
python-3.11.8
```

### 3. Railway / Render

#### Deployment Steps:
1. Connect GitHub repository
2. Set environment variables
3. Configure build and start commands

#### Start Command:
```
gunicorn meal_buddy.wsgi
```

### 4. AWS / DigitalOcean / Linode

Use Gunicorn with Nginx reverse proxy.

#### Install Gunicorn:
```bash
pip install gunicorn
```

#### Run Gunicorn:
```bash
gunicorn meal_buddy.wsgi:application --bind 0.0.0.0:8000
```

#### Nginx Configuration:
```nginx
server {
    listen 80;
    server_name yourdomain.com;

    location /static/ {
        alias /path/to/FinalMealmate/staticfiles/;
    }

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

## Environment Variables

Create a `.env` file (not tracked by git):
```
DEBUG=False
SECRET_KEY=your-secret-key-here
RAZORPAY_KEY_ID=your-razorpay-key
RAZORPAY_KEY_SECRET=your-razorpay-secret
ALLOWED_HOSTS=yourdomain.com,www.yourdomain.com
DATABASE_URL=postgresql://user:password@localhost/dbname
```

## Production Checklist

- [ ] Set up PostgreSQL database
- [ ] Configure SSL/TLS certificate
- [ ] Set up email service for notifications
- [ ] Configure CORS if needed
- [ ] Set up monitoring and logging
- [ ] Configure backup strategy
- [ ] Test payment flow with Razorpay sandbox
- [ ] Set up error tracking (Sentry)
- [ ] Configure CDN for static files (optional)

## Troubleshooting

### Static Files Not Loading
```bash
python manage.py collectstatic --no-input
```

### Database Connection Issues
- Verify DATABASE_URL format
- Check database credentials
- Ensure migrations are applied

### Razorpay Integration Issues
- Verify API keys are correct
- Check payment capture settings
- Test with Razorpay sandbox first
