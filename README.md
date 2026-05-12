# Meal Buddy - Food Delivery App

A Django-based food delivery application with admin restaurant management and customer ordering with Razorpay payment integration.

## Features

- **Customer Management**
  - User registration and authentication
  - Browse restaurants and menus
  - Add items to cart and adjust quantities
  - Checkout with Razorpay payment integration
  - Order tracking

- **Admin Features**
  - Add, update, and delete restaurants
  - Manage restaurant menus
  - Add items with descriptions, prices, and dietary info

- **Payment Integration**
  - Razorpay payment gateway integration
  - Secure order checkout

## Tech Stack

- **Backend**: Django 5.2.5
- **Database**: SQLite (development) / PostgreSQL (production recommended)
- **Payment**: Razorpay
- **Frontend**: Django templates with HTML/CSS

## Project Structure

```
FinalMealmate/
├── delivery/                  # Main app
│   ├── migrations/           # Database migrations
│   ├── static/css/           # CSS files
│   ├── templates/delivery/   # HTML templates
│   ├── models.py             # Database models
│   ├── views.py              # View logic
│   └── urls.py               # URL routing
├── meal_buddy/               # Project configuration
│   ├── settings.py           # Django settings
│   ├── urls.py               # Main URL config
│   ├── wsgi.py              # Production WSGI
│   └── asgi.py              # Production ASGI
├── manage.py                # Django management
└── requirements.txt         # Python dependencies
```

## Installation

### Local Development Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/kulsumummi/meal-Buddy-app.git
   cd meal-Buddy-app
   ```

2. **Create and activate virtual environment**
   ```bash
   # Windows
   python -m venv venv
   venv\Scripts\activate

   # macOS/Linux
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Run migrations**
   ```bash
   python manage.py migrate
   ```

5. **Create superuser (optional, for admin)**
   ```bash
   python manage.py createsuperuser
   ```

6. **Start development server**
   ```bash
   python manage.py runserver
   ```

   Access the app at: `http://127.0.0.1:8000/`

## Configuration

### Development vs Production

**Important**: The current settings are configured for development only.

#### For Production Deployment:

1. **Update `meal_buddy/settings.py`**:
   ```python
   DEBUG = False  # Turn off debug mode
   ALLOWED_HOSTS = ['yourdomain.com', 'www.yourdomain.com']
   SECRET_KEY = 'your-secure-random-key-here'
   ```

2. **Set environment variables** (recommended for sensitive data):
   ```bash
   export DJANGO_SECRET_KEY='your-secret-key'
   export RAZORPAY_KEY_ID='your-razorpay-key'
   export RAZORPAY_KEY_SECRET='your-razorpay-secret'
   export DEBUG='False'
   export ALLOWED_HOSTS='yourdomain.com,www.yourdomain.com'
   ```

3. **Collect static files**:
   ```bash
   python manage.py collectstatic --no-input
   ```

4. **Run migrations on production**:
   ```bash
   python manage.py migrate
   ```

5. **Use a production WSGI server** (not Django's runserver):
   - Gunicorn
   - uWSGI
   - Waitress

## Razorpay Setup

1. Get your Razorpay API keys from [Razorpay Dashboard](https://dashboard.razorpay.com/)

2. Add them to `meal_buddy/settings.py`:
   ```python
   RAZORPAY_KEY_ID = 'your_razorpay_key_id'
   RAZORPAY_KEY_SECRET = 'your_razorpay_key_secret'
   ```

   Or set as environment variables for production.

## Database Models

### Customer
- username
- password (stored in plain text - consider hashing for production)
- email
- mobile
- address

### Restaurant
- name
- picture (URL)
- cuisine
- rating

### Item
- restaurant (ForeignKey)
- name
- description
- price
- vegetarian (boolean)
- picture (URL)

### Cart & CartItem
- Customer has one Cart
- Cart contains multiple CartItems
- CartItem has quantity tracking

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Home page |
| `/open_signin` | GET | Sign in page |
| `/open_signup` | GET | Sign up page |
| `/signup` | POST | Create new customer |
| `/signin` | POST | Customer login |
| `/open_show_restaurant` | GET | Browse restaurants |
| `/view_menu/<id>/<username>` | GET | View restaurant menu |
| `/add_to_cart/<item_id>/<username>` | GET | Add item to cart |
| `/show_cart/<username>` | GET | View cart |
| `/checkout/<username>/` | GET | Checkout page |
| `/orders/<username>/` | GET | View orders |

## Default Test Accounts

### Admin
- **Username**: `admin`
- **Password**: (any password, admin login just checks username)

## Deployment Checklist

Before deploying to production:

- [ ] Set `DEBUG = False` in settings
- [ ] Configure `ALLOWED_HOSTS` with your domain
- [ ] Generate a secure `SECRET_KEY`
- [ ] Set up Razorpay credentials
- [ ] Run `collectstatic` command
- [ ] Set up a production database (PostgreSQL recommended)
- [ ] Use a production WSGI server (Gunicorn, uWSGI)
- [ ] Set up SSL/HTTPS
- [ ] Configure CSRF_TRUSTED_ORIGINS for your domain
- [ ] Set up proper logging and error tracking
- [ ] Consider hashing passwords in the Customer model (currently stored plain)

## Common Issues

### Static Files Not Loading
- Run: `python manage.py collectstatic --no-input`
- Ensure `STATIC_URL` and `STATIC_ROOT` are properly configured
- Verify web server is configured to serve static files

### 404 Errors
- Verify templates exist in `delivery/templates/delivery/`
- Check URL configuration in `delivery/urls.py`
- Ensure all view functions are defined in `views.py`

### Database Errors
- Ensure migrations are applied: `python manage.py migrate`
- Check database file permissions
- For production, consider using PostgreSQL

## Security Notes

⚠️ **Current Security Limitations** (for development/learning only):
- Passwords stored in plain text (use Django's auth system for production)
- No HTTPS enforcement
- Secret key is visible in settings
- Limited input validation

For production use, consider:
- Using Django's built-in User authentication
- Implementing proper password hashing
- Adding comprehensive input validation
- Setting up SSL/TLS certificates
- Using environment variables for secrets

## Contributing

Feel free to fork and submit pull requests.

## License

This project is provided as-is for educational purposes.

## Support

For issues or questions, please open an issue in the GitHub repository.

---

**Happy ordering! 🍔🍕🍜**
