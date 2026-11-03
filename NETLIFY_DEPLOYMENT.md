# Netlify Deployment Guide

## Pre-Deployment Checklist

### 1. Update Personal Information

#### Contact.vue
```typescript
// Update social links (lines ~27-47)
const socialLinks = [
  {
    name: 'GitHub',
    url: 'https://github.com/YOUR_USERNAME', // ← Update
  },
  {
    name: 'LinkedIn',
    url: 'https://linkedin.com/in/YOUR_USERNAME', // ← Update
  },
  {
    name: 'Twitter',
    url: 'https://twitter.com/YOUR_USERNAME', // ← Update
  },
  {
    name: 'Email',
    url: 'mailto:YOUR_EMAIL@example.com', // ← Update
  }
]

// Update contact info (lines ~49-67)
const contactInfo = [
  {
    icon: '📧',
    label: 'Email',
    value: 'YOUR_EMAIL@example.com', // ← Update
    link: 'mailto:YOUR_EMAIL@example.com'
  },
  {
    icon: '📱',
    label: 'Phone',
    value: '+1 (555) 123-4567', // ← Update
    link: 'tel:+15551234567'
  },
  {
    icon: '📍',
    label: 'Location',
    value: 'Your City, Country', // ← Update
    link: null
  }
]
```

#### Experience.vue
Update your work experience, education, and certifications (lines ~12-55).

#### Hero.vue
Update your name, title, and description.

#### About.vue
Update your bio and information.

### 2. Setup Contact Form (Formspree - Free)

#### Option A: Formspree (Recommended - Easy & Free)

1. **Sign up** at [https://formspree.io/](https://formspree.io/)
2. **Create a new form** project
3. **Get your form endpoint**: `https://formspree.io/f/YOUR_FORM_ID`
4. **Update Contact.vue** (line ~72):
   ```typescript
   const FORMSPREE_ENDPOINT = 'https://formspree.io/f/YOUR_FORM_ID'
   ```

**Features**:
- ✅ 50 submissions/month (free)
- ✅ Email notifications
- ✅ Spam filtering
- ✅ File uploads
- ✅ No backend needed

#### Option B: EmailJS (Alternative)

1. **Sign up** at [https://www.emailjs.com/](https://www.emailjs.com/)
2. **Create email service** (Gmail, Outlook, etc.)
3. **Create email template**
4. **Install EmailJS**:
   ```bash
   npm install @emailjs/browser
   ```
5. **Update Contact.vue**:
   ```typescript
   import emailjs from '@emailjs/browser'
   
   const handleSubmit = async (e: Event) => {
     e.preventDefault()
     if (!validateForm()) return
     
     isSubmitting.value = true
     
     try {
       await emailjs.send(
         'YOUR_SERVICE_ID',
         'YOUR_TEMPLATE_ID',
         {
           from_name: formData.value.name,
           from_email: formData.value.email,
           subject: formData.value.subject,
           message: formData.value.message
         },
         'YOUR_PUBLIC_KEY'
       )
       
       submitStatus.value = 'success'
       statusMessage.value = 'Message sent successfully!'
       
       // Reset form
       formData.value = { name: '', email: '', subject: '', message: '' }
     } catch (error) {
       submitStatus.value = 'error'
       statusMessage.value = 'Failed to send message.'
     } finally {
       isSubmitting.value = false
     }
   }
   ```

### 3. Add Images/Assets

Place all your images in `client/public/images/`:
- Profile photo
- Project screenshots
- Favicon
- Any other assets

Update image paths in components to match.

### 4. Build Configuration

Create `client/netlify.toml`:
```toml
[build]
  command = "npm run build"
  publish = "dist"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

[build.environment]
  NODE_VERSION = "18"
```

### 5. Environment Variables (if needed)

If using EmailJS or other services requiring API keys:

1. In Netlify Dashboard: **Site settings** → **Environment variables**
2. Add variables:
   - `VITE_EMAILJS_SERVICE_ID`
   - `VITE_EMAILJS_TEMPLATE_ID`
   - `VITE_EMAILJS_PUBLIC_KEY`

3. Access in code:
   ```typescript
   const serviceId = import.meta.env.VITE_EMAILJS_SERVICE_ID
   ```

## Deployment Methods

### Method 1: GitHub + Netlify (Recommended)

1. **Initialize Git** (if not already):
   ```bash
   cd client
   git init
   git add .
   git commit -m "Initial commit"
   ```

2. **Create GitHub Repository**:
   - Go to [github.com](https://github.com)
   - Create new repository
   - Don't initialize with README

3. **Push to GitHub**:
   ```bash
   git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
   git branch -M main
   git push -u origin main
   ```

4. **Deploy on Netlify**:
   - Go to [netlify.com](https://netlify.com)
   - Click "Add new site" → "Import an existing project"
   - Connect to GitHub
   - Select your repository
   - Configure build settings:
     - **Base directory**: `client`
     - **Build command**: `npm run build`
     - **Publish directory**: `client/dist`
   - Click "Deploy site"

5. **Automatic Deployments**:
   - Every push to `main` branch triggers automatic deployment
   - Preview deployments for pull requests

### Method 2: Drag & Drop (Quick Deploy)

1. **Build locally**:
   ```bash
   cd client
   npm run build
   ```

2. **Deploy**:
   - Go to [netlify.com](https://netlify.com)
   - Drag & drop the `client/dist` folder to Netlify

3. **Note**: Manual deployments required for updates

### Method 3: Netlify CLI

1. **Install Netlify CLI**:
   ```bash
   npm install -g netlify-cli
   ```

2. **Login**:
   ```bash
   netlify login
   ```

3. **Deploy**:
   ```bash
   cd client
   npm run build
   netlify deploy --prod
   ```

## Post-Deployment Configuration

### 1. Custom Domain (Optional)

1. **In Netlify Dashboard**: **Domain settings** → **Add custom domain**
2. **Configure DNS** at your domain registrar:
   ```
   Type    Name    Value
   A       @       75.2.60.5
   CNAME   www     YOUR_SITE.netlify.app
   ```
3. **Enable HTTPS**: Automatic with Netlify

### 2. Forms Setup (Netlify Forms - Alternative to Formspree)

#### If you want to use Netlify's built-in forms:

1. **Update form in Contact.vue**:
   ```html
   <form 
     name="contact" 
     method="POST" 
     data-netlify="true"
     data-netlify-honeypot="bot-field"
     @submit.prevent="handleNetlifySubmit"
   >
     <input type="hidden" name="form-name" value="contact" />
     <!-- Rest of form fields -->
   </form>
   ```

2. **Update submit handler**:
   ```typescript
   const handleNetlifySubmit = async (e: Event) => {
     if (!validateForm()) return
     
     isSubmitting.value = true
     
     const formElement = e.target as HTMLFormElement
     const formData = new FormData(formElement)
     
     try {
       const response = await fetch('/', {
         method: 'POST',
         headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
         body: new URLSearchParams(formData as any).toString()
       })
       
       if (response.ok) {
         submitStatus.value = 'success'
         statusMessage.value = 'Message sent!'
       }
     } catch (error) {
       submitStatus.value = 'error'
     } finally {
       isSubmitting.value = false
     }
   }
   ```

3. **View submissions**: Netlify Dashboard → **Forms**

### 3. Performance Optimization

Add to `client/vite.config.ts`:
```typescript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  build: {
    minify: 'terser',
    terserOptions: {
      compress: {
        drop_console: true // Remove console.logs in production
      }
    },
    rollupOptions: {
      output: {
        manualChunks: {
          'vendor': ['vue'],
        }
      }
    }
  }
})
```

### 4. SEO Optimization

Update `client/index.html`:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  
  <!-- SEO Meta Tags -->
  <title>Merawi Ayele - Full Stack Developer</title>
  <meta name="description" content="Full Stack Developer specializing in Vue.js, Laravel, and modern web technologies. View my portfolio and get in touch.">
  <meta name="keywords" content="Full Stack Developer, Vue.js, Laravel, Web Developer, Portfolio">
  <meta name="author" content="Merawi Ayele">
  
  <!-- Open Graph / Social Media -->
  <meta property="og:type" content="website">
  <meta property="og:title" content="Merawi Ayele - Full Stack Developer">
  <meta property="og:description" content="Full Stack Developer specializing in Vue.js, Laravel, and modern web technologies.">
  <meta property="og:image" content="/images/profile.jpg">
  <meta property="og:url" content="https://your-domain.com">
  
  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="Merawi Ayele - Full Stack Developer">
  <meta name="twitter:description" content="Full Stack Developer specializing in Vue.js, Laravel, and modern web technologies.">
  <meta name="twitter:image" content="/images/profile.jpg">
  
  <!-- Favicon -->
  <link rel="icon" type="image/x-icon" href="/favicon.ico">
  
  <link rel="preconnect" href="https://fonts.googleapis.com">
</head>
<body>
  <div id="app"></div>
  <script type="module" src="/src/main.ts"></script>
</body>
</html>
```

### 5. Analytics (Optional)

#### Google Analytics:

1. **Get tracking ID** from [analytics.google.com](https://analytics.google.com)
2. **Add to index.html** (in `<head>`):
   ```html
   <!-- Google Analytics -->
   <script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
   <script>
     window.dataLayer = window.dataLayer || [];
     function gtag(){dataLayer.push(arguments);}
     gtag('js', new Date());
     gtag('config', 'G-XXXXXXXXXX');
   </script>
   ```

## Testing Before Deployment

1. **Build locally**:
   ```bash
   cd client
   npm run build
   ```

2. **Preview build**:
   ```bash
   npm run preview
   ```

3. **Check**:
   - All links work
   - Contact form submits
   - Images load
   - Responsive on mobile
   - No console errors

## Troubleshooting

### Issue: 404 on Page Refresh
**Solution**: Ensure `netlify.toml` has redirects configured (see step 4 above)

### Issue: Build Fails
- Check Node version matches (use Node 18+)
- Clear cache: `rm -rf node_modules package-lock.json && npm install`
- Check build logs in Netlify dashboard

### Issue: Contact Form Not Working
- Verify Formspree endpoint is correct
- Check browser console for errors
- Test with curl:
  ```bash
  curl -X POST https://formspree.io/f/YOUR_FORM_ID \
    -H "Content-Type: application/json" \
    -d '{"email":"test@example.com","name":"Test","subject":"Test","message":"Test"}'
  ```

### Issue: Images Not Loading
- Ensure images are in `public/images/` folder
- Check paths start with `/images/` not `./images/`
- Verify file names match exactly (case-sensitive)

## Maintenance

### Update Content
1. Edit component files
2. Commit and push to GitHub
3. Netlify auto-deploys

### Monitor
- **Netlify Analytics**: Built-in visitor stats
- **Forms**: Check submissions in Netlify dashboard
- **Logs**: Check function logs for errors

## Cost

**Free Tier Includes**:
- ✅ 100GB bandwidth/month
- ✅ 300 build minutes/month
- ✅ Automatic HTTPS
- ✅ Form submissions (100/month)
- ✅ Custom domain
- ✅ Deploy previews

**Paid Plans** (if needed):
- Pro: $19/month (1TB bandwidth, more builds)
- Business: $99/month (team features)

## Security

✅ **Automatic HTTPS** via Let's Encrypt
✅ **DDoS protection** included
✅ **Form spam filtering** built-in
✅ **Client-side validation** implemented
✅ **No sensitive data** exposed (frontend only)

## Final Checklist

Before going live:
- [ ] Update all personal information
- [ ] Setup contact form (Formspree/EmailJS)
- [ ] Add real project data
- [ ] Update experience/education
- [ ] Add profile photo and project images
- [ ] Test contact form
- [ ] Test on mobile devices
- [ ] Check SEO meta tags
- [ ] Setup custom domain (optional)
- [ ] Enable analytics (optional)
- [ ] Test all navigation links
- [ ] Verify social media links

## Resources

- [Netlify Documentation](https://docs.netlify.com/)
- [Formspree Documentation](https://help.formspree.io/)
- [Vite Documentation](https://vitejs.dev/)
- [Vue.js Documentation](https://vuejs.org/)

---

**You're ready to deploy!** 🚀

Once deployed, share your portfolio link and start receiving opportunities!
