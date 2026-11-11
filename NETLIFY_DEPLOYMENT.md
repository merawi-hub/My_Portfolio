# Netlify Deployment Guide

## Pre-Deployment Checklist

### 1. Update Personal Information

#### Contact.vue
The Contact component has been simplified. Update the contact information:
```typescript

const contactCards = [
  {
    icon: '📧',
    label: 'Email',
    value: 'merawi@example.com', 
    link: 'mailto:merawi@example.com'
  },
  {
    icon: '📱',
    label: 'Phone',
    value: '+251 912 345 678', 
    link: 'tel:+251912345678'
  }
]

const socialLinks = [
  { 
    name: 'GitHub', 
    icon: 'github-icon',
    url: 'https:
  },
  { 
    name: 'LinkedIn', 
    icon: 'linkedin-icon',
    url: 'https:
  },
  { 
    name: 'Twitter', 
    icon: 'twitter-icon',
    url: 'https:
  }
]
```

#### Projects.vue
Update your 4 projects with real data:
```typescript
const allProjects = [
  {
    id: 1,
    title: 'Online Grocery Delivery App',
    description: 'Your description here...',
    image: '/images/project-grocery.jpg', 
    tags: ['Laravel', 'Vue.js', 'MySQL', 'REST API'],
    github: 'https:
    demo: 'https:
  },
  
]
```

#### About.vue
Update your personal information:
```typescript

"I'm Merawi Ayele, a software engineer..." 
"I enjoy working across frontend and backend..." 
"I'm currently working with Laravel and Vue.js" 
```

Also update:
- Resume download link: `href="/resume.pdf"` → Add your resume PDF to `public/resume.pdf`
- About photo: `src="/images/about.jpg"` → Add your photo

#### Hero.vue  
Update your name and tagline:
```vue
<h1 class="name-title">Merawi Ayele</h1> 
<h3 class="tagline-text">BUILDING INNOVATIVE SOLUTIONS WITH</h3> 
<div class="tagline-highlight">MODERN TECHNOLOGIES</div> 
```

Update hero image: `src="/images/profile.jpg"` → Add your image

#### Skills.vue
Update your tech stack across three sections:
```typescript

const frontendSkills = [
  { name: 'Vue.js', level: 90, icon: '⚡' },
  
]

const backendSkills = [
  { name: 'Laravel', level: 85, icon: '🔧' },
  
]

const tools = [
  { name: 'Git', level: 88, icon: '🔀' },
  
]
```

#### Interests.vue
Update your personal interests/hobbies:
```typescript
const interests = [
  { 
    name: 'Music', 
    icon: '🎵', 
    description: 'Your description...' 
  },
  
]
```

### 2. Add Images/Assets

Place all your images in `client/public/images/`:
- **Profile photo**: `/images/profile.jpg` (used in Hero section)
- **About photo**: `/images/about.jpg` (used in About section)  
- **Project screenshots**: 
  - `/images/project-grocery.jpg`
  - `/images/project-clinic.jpg`
  - `/images/project-taskmanager.jpg`
  - `/images/project-portfolio.jpg`
- **Resume PDF**: `/resume.pdf` (for download button in About section)
- **Favicon**: `/favicon.ico`

**Important Notes**:
- Image paths start with `/` (e.g., `/images/profile.jpg`)
- File names are case-sensitive
- Recommended image sizes:
  - Profile/About photos: 800x800px (square, 1:1 aspect ratio)
  - Project screenshots: 1200x600px or actual app screenshots
  - Optimize images before uploading (use tools like TinyPNG)

### 3. Contact Form Setup

**Note**: This portfolio currently uses a client-side only contact form. To receive actual messages, you need to integrate a form service.

#### Option A: Formspree (Recommended - Easy & Free)

1. **Sign up** at [https:
2. **Create a new form** project
3. **Get your form endpoint**: `https:
4. **Update Contact.vue** (line ~72):
   ```typescript
   const FORMSPREE_ENDPOINT = 'https:
   ```

**Features**:
- ✅ 50 submissions/month (free)
- ✅ Email notifications
- ✅ Spam filtering
- ✅ File uploads
- ✅ No backend needed

#### Option B: EmailJS (Alternative)

1. **Sign up** at [https:
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

### 4. Animation Settings (Optional)

The portfolio includes scroll-based card morphing animations. Current settings:
- **Scroll threshold**: 500px (4-5 scroll actions needed)
- **Lock timeout**: 1 second after reaching full rectangle
- These are optimized values - only change if explicitly needed

Settings are in `client/src/App.vue`:
```typescript
const SCROLL_THRESHOLD = 500 
const LOCK_TIMEOUT = 1000 
```

### 5. Build Configuration

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

### 6. Environment Variables (if needed)

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

## Current Project Structure

```
My_Portfolio/
├── client/                          # Vue.js frontend
│   ├── public/
│   │   ├── images/                 # Place all images here
│   │   │   ├── profile.jpg         # Hero section image
│   │   │   ├── about.jpg           # About section image
│   │   │   ├── project-*.jpg       # Project screenshots
│   │   │   └── ...
│   │   ├── resume.pdf              # Your resume for download
│   │   └── favicon.ico
│   ├── src/
│   │   ├── components/
│   │   │   ├── Hero.vue           # Home page with name
│   │   │   ├── About.vue          # About section with bio & resume button
│   │   │   ├── Skills.vue         # Skills in 3 sections
│   │   │   ├── Projects.vue       # 4 projects with tabs
│   │   │   ├── Contact.vue        # Contact form & social links
│   │   │   ├── Interests.vue      # Personal interests popup
│   │   │   └── Navbar.vue         # Navigation menu
│   │   ├── App.vue                # Main app with scroll animations
│   │   └── main.ts
│   ├── package.json
│   ├── vite.config.ts
│   └── netlify.toml               # Netlify configuration
└── server/                         # Laravel backend (not deployed to Netlify)
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
   - Go to [github.com](https:
   - Create new repository
   - Don't initialize with README

3. **Push to GitHub**:
   ```bash
   git remote add origin https:
   git branch -M main
   git push -u origin main
   ```

4. **Deploy on Netlify**:
   - Go to [netlify.com](https:
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
   - Go to [netlify.com](https:
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
        drop_console: true 
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

  <title>Merawi Ayele - Full Stack Developer</title>
  <meta name="description" content="Full Stack Developer specializing in Vue.js, Laravel, and modern web technologies. View my portfolio and get in touch.">
  <meta name="keywords" content="Full Stack Developer, Vue.js, Laravel, Web Developer, Portfolio">
  <meta name="author" content="Merawi Ayele">

  <meta property="og:type" content="website">
  <meta property="og:title" content="Merawi Ayele - Full Stack Developer">
  <meta property="og:description" content="Full Stack Developer specializing in Vue.js, Laravel, and modern web technologies.">
  <meta property="og:image" content="/images/profile.jpg">
  <meta property="og:url" content="https:

  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="Merawi Ayele - Full Stack Developer">
  <meta name="twitter:description" content="Full Stack Developer specializing in Vue.js, Laravel, and modern web technologies.">
  <meta name="twitter:image" content="/images/profile.jpg">

  <link rel="icon" type="image/x-icon" href="/favicon.ico">
  
  <link rel="preconnect" href="https:
</head>
<body>
  <div id="app"></div>
  <script type="module" src="/src/main.ts"></script>
</body>
</html>
```

### 5. Analytics (Optional)

#### Google Analytics:

1. **Get tracking ID** from [analytics.google.com](https:
2. **Add to index.html** (in `<head>`):
   ```html
   
   <script async src="https:
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
  curl -X POST https:
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
- [ ] Update Hero section with your name and tagline
- [ ] Update About section with your bio and current tech stack
- [ ] Add your profile photo (`/images/profile.jpg`)
- [ ] Add your about photo (`/images/about.jpg`)
- [ ] Add your resume PDF (`/resume.pdf`)
- [ ] Update all 4 projects with real data and screenshots
- [ ] Update Skills section with your actual skills
- [ ] Update Interests section with your hobbies
- [ ] Update Contact section with your email and phone
- [ ] Update social media links (GitHub, LinkedIn, Twitter)
- [ ] Setup contact form (Formspree/EmailJS/Netlify Forms)
- [ ] Test contact form submission
- [ ] Test all navigation (Home, About, Skills, Projects, Contact)
- [ ] Test "Things I enjoy" button and interests popup
- [ ] Test resume download button
- [ ] Test project tabs switching (Project 1-4)
- [ ] Verify scroll animations work smoothly
- [ ] Test on mobile devices and different screen sizes
- [ ] Check all images load correctly
- [ ] Verify no console errors in browser
- [ ] Test all external links (GitHub, LinkedIn, etc.)
- [ ] Check SEO meta tags in `index.html`
- [ ] Setup custom domain (optional)
- [ ] Enable analytics (optional)

## Portfolio Features Summary

**Current Features**:
- ✅ Smooth scroll-based card morphing animations
- ✅ 5 main sections: Home, About, Skills, Projects, Contact
- ✅ Tabbed projects showcase (4 projects)
- ✅ Skills organized in 3 categories (Frontend, Backend, Tools)
- ✅ Personal interests popup overlay
- ✅ Resume download functionality
- ✅ Contact form with social links
- ✅ Responsive hamburger navigation menu
- ✅ No scrolling required (except Contact page)
- ✅ Modern gradient backgrounds and animations
- ✅ Fully responsive design

**Technical Stack**:
- Vue 3 + TypeScript
- Vite build tool
- CSS3 with custom animations
- No external UI libraries (custom components)

## Resources

- [Netlify Documentation](https:
- [Formspree Documentation](https:
- [Vite Documentation](https:
- [Vue.js Documentation](https:

---

**You're ready to deploy!** 🚀

Once deployed, share your portfolio link and start receiving opportunities!
