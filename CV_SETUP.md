# CV/Resume Download Setup

## How the Download CV Button Works

When clicked, the "Download CV" button will download your resume/CV file from the portfolio.

## Setup Instructions

### Step 1: Prepare Your CV
1. Create or update your CV/Resume
2. Save it as a PDF file
3. Name it: `Merawi_Ayele_CV.pdf` (or any name you prefer)

### Step 2: Add CV to Your Project

**Create the folder and add your CV:**

```bash
# Navigate to your project
cd client/public

# Create cv folder
mkdir cv

# Copy your CV file into this folder
# Place: client/public/cv/Merawi_Ayele_CV.pdf
```

**Folder structure should look like:**
```
client/
├── public/
│   ├── cv/
│   │   └── Merawi_Ayele_CV.pdf    ← Your CV goes here
│   ├── images/
│   └── favicon.ico
└── src/
```

### Step 3: Test the Download

1. Start your dev server: `npm run dev`
2. Go to the homepage
3. Click "Download CV" button
4. Your CV should download automatically

## Alternative: Use a Different Filename

If you want to use a different filename, update `Hero.vue`:

```typescript
const downloadCV = () => {
  const link = document.createElement('a')
  link.href = '/cv/YOUR_CUSTOM_NAME.pdf'  // ← Change this
  link.download = 'YOUR_CUSTOM_NAME.pdf'   // ← And this
  document.body.appendChild(link)
  link.click()
  document.body.removeChild(link)
}
```

## Alternative: Link to External CV

If your CV is hosted elsewhere (Google Drive, Dropbox, etc.):

```typescript
const downloadCV = () => {
  window.open('https://your-cv-url.com/cv.pdf', '_blank')
}
```

## If You Don't Have a CV Yet

The button is already set up. When you don't have a CV:

1. The button will try to download but show a 404 error
2. You can temporarily disable it or change it to scroll to Contact

**To make it scroll to Contact instead:**

In `Hero.vue`, comment out the download code and uncomment the scroll code:

```typescript
const downloadCV = () => {
  // Comment out download code:
  // const link = document.createElement('a')
  // link.href = '/cv/Merawi_Ayele_CV.pdf'
  // ...
  
  // Uncomment scroll to contact:
  const contactSection = document.getElementById('contact')
  if (contactSection) {
    const navbarHeight = 80
    const elementPosition = contactSection.getBoundingClientRect().top + window.pageYOffset
    const offsetPosition = elementPosition - navbarHeight
    window.scrollTo({ top: offsetPosition, behavior: 'smooth' })
  }
}
```

## Tips for Creating a Great CV

### What to Include:
- Contact information (email, phone, LinkedIn)
- Professional summary
- Work experience
- Education
- Skills (technical skills, languages, etc.)
- Projects (link to portfolio)
- Certifications

### Format:
- PDF format (universal, preserves formatting)
- Professional font (Arial, Calibri, Times New Roman)
- 1-2 pages maximum
- Clear section headings
- Bullet points for easy reading
- Consistent formatting
- No photos (unless common in your country)

### Tools to Create CV:
- **Canva** - https://www.canva.com/resumes/templates/
- **Resume.io** - https://resume.io/
- **Novoresume** - https://novoresume.com/
- **Google Docs** - Professional templates available
- **Microsoft Word** - Built-in resume templates
- **LaTeX** - For technical resumes (Overleaf.com)

## Security Note

✅ **Safe to include in portfolio:**
- Name
- Email
- Phone number
- LinkedIn profile
- GitHub profile
- Professional summary
- Work experience
- Education
- Skills

❌ **Don't include:**
- Full home address (city is fine)
- Social security number
- Date of birth
- Passport number
- Bank information
- Personal ID numbers

---

Once you add your CV file to `client/public/cv/`, the download will work automatically! 🎉
