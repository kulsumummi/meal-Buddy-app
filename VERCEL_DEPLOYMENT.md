# Vercel Deployment Guide

Your Meal Buddy app is now configured for Vercel deployment!

## Step-by-Step Vercel Deployment

### 1. Connect GitHub Repository to Vercel

1. Go to [Vercel Dashboard](https://vercel.com/dashboard)
2. Click **"Add New..."** → **"Project"**
3. Select **"Import Git Repository"**
4. Find and select `kulsumummi/meal-Buddy-app`
5. Click **"Import"**

### 2. Configure Environment Variables

After importing, Vercel will ask for environment variables. Add these:

```
DJANGO_SECRET_KEY=<generate-secure-key>
DJANGO_DEBUG=False
DJANGO_ALLOWED_HOSTS=<your-vercel-url>.vercel.app
RAZORPAY_KEY_ID=<your-razorpay-key>
RAZORPAY_KEY_SECRET=<your-razorpay-secret>
```

**To generate a secure SECRET_KEY:**
```bash
python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
```

Your Vercel domain will be shown during setup (usually `meal-buddy-app.vercel.app`)

### 3. Set Build Command (if needed)

Vercel should auto-detect from `vercel.json`, but if asked:
```
Build Command: pip install -r requirements.txt && python manage.py collectstatic --noinput
Output Directory: staticfiles
```

### 4. Deploy

Click **"Deploy"** and wait for the build to complete.

---

## After Deployment

### ✅ Test Your App

1. Open your Vercel URL: `https://<your-project>.vercel.app/`
2. You should see the Meal Buddy home page
3. Test sign-up and sign-in flows
4. Test restaurant browsing

### ⚠️ Important Notes

**Database Limitation:**
- Your app currently uses SQLite (`db.sqlite3`), which doesn't persist on Vercel
- **Each deployment creates a new database** - previous data is lost
- For production with persistent data, upgrade to Vercel Postgres or another database service

**To Use Vercel Postgres (Recommended):**

1. In Vercel Dashboard, go to **"Storage"** → **"Create"** → **"Postgres"**
2. Connect it to your project
3. Vercel will add connection variables automatically
4. Update `meal_buddy/settings.py` to use PostgreSQL

**Static Files:**
- CSS and images are served from the `staticfiles/` directory
- If they don't load, run `collectstatic` again

### 🔗 Custom Domain (Optional)

1. In Vercel Project Settings → **"Domains"**
2. Add your custom domain
3. Follow DNS configuration instructions
4. Update `DJANGO_ALLOWED_HOSTS` to include your domain

---

## Troubleshooting

### 500 Error
- Check Vercel logs: Project → **"Deployments"** → Click latest → **"Logs"**
- Verify all environment variables are set correctly
- Ensure `DJANGO_ALLOWED_HOSTS` includes your Vercel domain

### Static Files Not Loading
- Run: `python manage.py collectstatic --noinput`
- Push the `staticfiles/` directory changes to GitHub
- Redeploy from Vercel

### Database Issues
- SQLite doesn't persist on Vercel - implement Postgres as described above
- Or use a cloud database service (AWS RDS, Heroku Postgres, etc.)

### Razorpay Not Working
- Verify `RAZORPAY_KEY_ID` and `RAZORPAY_KEY_SECRET` are set in environment
- Use sandbox keys for testing, production keys for live

---

## Production Checklist

- [ ] Generate secure `DJANGO_SECRET_KEY`
- [ ] Set `DJANGO_DEBUG=False`
- [ ] Configure Razorpay credentials
- [ ] Set up persistent database (Postgres recommended)
- [ ] Test all features on deployed URL
- [ ] Set up custom domain (optional)
- [ ] Configure email notifications (optional)
- [ ] Monitor Vercel logs for errors

---

**Your Vercel Dashboard:** [kulsumummis-projects](https://vercel.com/kulsumummis-projects)

**Happy deploying! 🚀**
