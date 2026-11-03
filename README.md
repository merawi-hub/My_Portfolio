# Portfolio Website - Frontend

Modern, responsive portfolio website built with Vue.js 3, TypeScript, and Vite.

## 🚀 Features

- ✅ **Hero Section** - Animated introduction with profile photo
- ✅ **About Section** - Personal information with interests modal
- ✅ **Skills Section** - Interactive tech stack with 3D cubes and popup display
- ✅ **Projects Section** - Showcase of work with categories
- ✅ **Journey Section** - Timeline of experience, education, and certifications
- ✅ **Contact Section** - Form with validation (Formspree integration)
- ✅ **Fully Responsive** - Works on all devices
- ✅ **Performance Optimized** - Fast loading, smooth animations
- ✅ **SEO Ready** - Meta tags and semantic HTML

## 🛠️ Tech Stack

- **Framework**: Vue.js 3 (Composition API)
- **Language**: TypeScript
- **Build Tool**: Vite
- **Styling**: CSS3 with CSS Variables
- **Deployment**: Netlify
- **Form Handling**: Formspree

## 📦 Installation

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

## 🌐 Deployment

### Deploy to Netlify

1. **Update personal information** in components:
   - `Contact.vue` - Social links, email, phone
   - `Experience.vue` - Work history, education
   - `Hero.vue` - Name and title
   - `About.vue` - Bio

2. **Setup Formspree** for contact form:
   - Sign up at [formspree.io](https://formspree.io)
   - Create new form
   - Update `FORMSPREE_ENDPOINT` in `Contact.vue`

3. **Deploy**:
   ```bash
   # Option 1: Connect GitHub repo to Netlify (recommended)
   # - Push to GitHub
   # - Connect repository in Netlify dashboard
   # - Automatic deployments on push
   
   # Option 2: Manual deployment
   npm run build
   # Drag & drop 'dist' folder to Netlify
   
   # Option 3: Netlify CLI
   npm install -g netlify-cli
   netlify login
   npm run build
   netlify deploy --prod
   ```

See `NETLIFY_DEPLOYMENT.md` for detailed instructions.

## 📁 Project Structure

```
client/
├── public/
│   ├── images/          # Images and assets
│   └── favicon.ico
├── src/
│   ├── assets/
│   │   └── styles/      # Global styles and variables
│   ├── components/      # Vue components
│   │   ├── Hero.vue
│   │   ├── Navbar.vue
│   │   ├── About.vue
│   │   ├── Skills.vue
│   │   ├── Projects.vue
│   │   ├── Experience.vue
│   │   ├── Contact.vue
│   │   └── Interests.vue
│   ├── App.vue          # Root component
│   └── main.ts          # Application entry
├── index.html           # HTML template
├── vite.config.ts       # Vite configuration
├── netlify.toml         # Netlify configuration
└── package.json

```

## 🎨 Customization

### Colors
Edit `src/assets/styles/variables.css`:
```css
--color-accent-primary: #a855f7;    /* Primary purple */
--color-accent-secondary: #8b5cf6;  /* Secondary purple */
--color-bg-primary: #0a0a0f;        /* Dark background */
```

### Content
1. **Hero Section**: Update name, title in `Hero.vue`
2. **About Section**: Update bio in `About.vue`
3. **Skills**: Update tech stacks in `Skills.vue`
4. **Projects**: Update projects array in `Projects.vue`
5. **Experience**: Update experience data in `Experience.vue`
6. **Contact**: Update contact info in `Contact.vue`

### Images
Place images in `public/images/`:
- `profile.jpg` - Profile photo
- `cartoon.jpg` - About section image
- Project screenshots

## 📝 Environment Variables

No environment variables needed for basic deployment.

Optional (for EmailJS alternative):
```env
VITE_EMAILJS_SERVICE_ID=your_service_id
VITE_EMAILJS_TEMPLATE_ID=your_template_id
VITE_EMAILJS_PUBLIC_KEY=your_public_key
```

## 🐛 Troubleshooting

### Build fails
```bash
# Clear cache and reinstall
rm -rf node_modules package-lock.json
npm install
npm run build
```

### Contact form not working
- Verify Formspree endpoint is correct
- Check browser console for errors
- Ensure form validation is passing

### Images not loading
- Verify images are in `public/images/`
- Check paths start with `/images/`
- File names are case-sensitive

## 📄 License

This project is open source and available under the MIT License.

## 👤 Author

**Merawi Ayele**
- Portfolio: [Your URL]
- GitHub: [@yourusername](https://github.com/yourusername)
- LinkedIn: [Your Profile](https://linkedin.com/in/yourusername)

---

Built with ❤️ using Vue.js and TypeScript
