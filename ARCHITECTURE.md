# KnoxCalls Landing - Frontend Architecture

## Overview

KnoxCalls Landing is a static marketing website for **Night Watch**, an after-hours answering service targeting HVAC and plumbing contractors in Knoxville, TN. The site is built with vanilla HTML/CSS/JavaScript for simplicity, performance, and ease of deployment to Cloudflare Pages.

---

## Tech Stack

### Core Technologies
- **HTML5** - Semantic markup with SEO optimizations
- **CSS3** - Vanilla CSS with CSS custom properties (variables) for theming
- **JavaScript (Vanilla)** - Minimal client-side scripting for interactive elements
- **Google Fonts** - Typography (`Bebas Neue` for headings, `Source Sans 3` for body)

### Build & Deployment
- **Cloudflare Pages** - Static site hosting with global CDN
- **Wrangler** - Cloudflare CLI tool for deployment (`v4.58.0`)
- **GitHub Actions** - CI/CD pipeline for automated deployments

### Development Tools
- **Git/GitHub** - Version control and repository hosting
- **npm** - Package management for tooling only (no runtime dependencies)

---

## Project Structure

```
knoxcalls-landing/
├── .github/
│   └── workflows/
│       └── deploy.yml           # GitHub Actions deployment workflow
├── public/                      # Root deployment directory
│   ├── index.html              # Homepage (main landing)
│   ├── about.html              # About/company information page
│   ├── hvac-answering-service.html    # HVAC industry-specific page
│   ├── plumber-answering-service.html # Plumbing industry-specific page
│   ├── privacy-policy.html     # Privacy policy (A2P compliance)
│   ├── terms-of-service.html   # Terms of service (A2P compliance)
│   ├── LogoImage.jpg           # Favicon & nav logo icon
│   ├── LogoLetters.jpg         # Full logo with text (social sharing)
│   └── call1.wav              # Demo audio file (11:47 PM emergency call)
├── assets/                     # Currently empty (reserved for future)
├── config.js                   # Deployment trigger config
├── package.json               # npm configuration (devDependencies only)
└── package-lock.json          # npm lock file
```

---

## Design System

### Brand Colors (CSS Variables)
```css
--vols-orange: #FF8200;         /* Primary brand color (UT Vols orange) */
--vols-orange-dark: #e67300;    /* Hover/active states */
--deep-navy: #0a1628;           /* Dark backgrounds, text */
--warm-white: #faf9f7;          /* Page backgrounds */
--steel-gray: #4a5568;          /* Body text, secondary elements */
--light-gray: #e8e6e3;          /* Borders, dividers */
--success-green: #2d8a5f;       /* Success indicators */
```

### Typography
- **Headings**: `Bebas Neue` (bold, display font with 1px letter-spacing)
- **Body/UI**: `Source Sans 3` (clean, readable sans-serif)
- **Fallbacks**: System fonts (`-apple-system, BlinkMacSystemFont`)

### Responsive Breakpoints
- **Mobile**: `@media (max-width: 900px)`
  - Navigation collapses to vertical layout
  - Multi-column grids become single column
  - Hero padding adjusted to prevent overlap with expanded nav

### Layout Patterns
- **Container**: `max-width: 1100px` with `padding: 0 24px`
- **Sections**: `padding: 100px 0` (desktop), `80px 0` (mobile)
- **Grid Systems**: CSS Grid with `auto-fit` for responsive cards

---

## Page Details

### Homepage (`index.html`)
**Purpose**: Primary conversion funnel for pilot program signup

**Sections**:
1. **Hero** - Value proposition, CTA buttons, stats (avg emergency job, won't leave VM, setup time)
2. **Problem** (#01) - Three pain points contractors face
3. **How It Works** (#02) - 3-step process + demo audio player + feature checklist
4. **Comparison** (#03) - Night Watch vs. typical answering services
5. **ROI Calculator** (#04) - Interactive calculator for missed call costs
6. **Pricing** (#05) - Pilot program pricing ($300/mo)
7. **Add-ons** - Daytime overflow, premium analytics, Twilio integration
8. **FAQ** (#06) - Common questions
9. **Local Story** (#07) - Knoxville-specific branding
10. **Waitlist/Lead Form** (#08) - Pilot access request form

**Key Features**:
- Fixed navigation with dropdown ("Who We Serve")
- Inline JavaScript for calculator, form handling, scroll-to-section
- Structured data (JSON-LD) for SEO
- A2P SMS compliance with opt-in consent

### Industry Pages
- `hvac-answering-service.html`
- `plumber-answering-service.html`

**Purpose**: SEO-optimized landing pages targeting specific contractor trades

**Structure**:
- Industry-specific hero headlines
- Trade-specific pain points (e.g., "compressor failures" for HVAC)
- CTA to main homepage
- Reused CSS from main site (embedded styles)
- JSON-LD structured data for local business SEO

### Compliance Pages
- `privacy-policy.html`
- `terms-of-service.html`

**Purpose**: Legal compliance for A2P SMS campaign registration with carriers

**Content**: Text-heavy legal documents with minimal styling

### About Page (`about.html`)
**Purpose**: Company background and trust-building content

---

## Styling Architecture

### CSS Organization (Embedded in `<style>` tags)
All pages use **embedded CSS** (not external stylesheets). This reduces HTTP requests and simplifies deployment.

**CSS Structure** (in order):
1. **CSS Variables** (`:root`)
2. **Reset/Base Styles** (`*`, `body`, headings)
3. **Layout** (`.container`)
4. **Navigation** (`nav`, `.nav-logo`, `.nav-links`, dropdown)
5. **Hero Sections** (`.hero`, `.industry-hero`)
6. **Content Sections** (`.section`, `.problem-section`, etc.)
7. **Components** (cards, buttons, forms, grids)
8. **Footer**
9. **Responsive Overrides** (`@media`)
10. **Utilities** (`html { scroll-behavior: smooth }`, `::selection`)

### Mobile Responsiveness Strategy
- **Desktop-first approach** with mobile overrides at `900px` breakpoint
- **Fixed nav on desktop**, **relative nav on mobile** (prevents content overlap)
- **Fluid typography** using `clamp()` for headings
- **Grid collapsing** from multi-column to single column

---

## JavaScript Features

### Inline Scripts (Homepage)
```javascript
// Configuration Object
const SITE_CONFIG = {
    companyName: "KnoxCalls",
    productName: "Night Watch",
    pilot: { spotsTotal: 15, spotsClaimed: 12 }
};

// Functions:
scrollToSection(id)          // Smooth scroll navigation
updateCalculator()           // ROI calculator logic
validateLeadForm()           // Form validation before submission
handleFormSubmit()           // Lead capture + SMS opt-in tracking
```

### Interactive Components
- **Audio Player**: HTML5 `<audio>` for demo call playback
- **ROI Calculator**: Real-time cost calculation based on user inputs
- **Lead Form**: Multi-field form with consent checkbox (A2P compliance)

---

## Deployment

### Hosting Platform
**Cloudflare Pages** - https://knoxcalls.com

**Benefits**:
- Global CDN with edge caching
- Automatic HTTPS
- Git-based deployments
- Zero configuration builds (static HTML)

### CI/CD Pipeline
**GitHub Actions** (`.github/workflows/deploy.yml`)

**Trigger**: Push to `main` branch

**Deployment Process**:
1. Checkout repository
2. Run Wrangler CLI to deploy `public/` directory to Cloudflare Pages
3. Cloudflare serves files from edge network

### Deployment Trigger
`config.js` contains deployment comments (acts as a dummy file to force redeployments)

---

## SEO Optimization

### On-Page SEO
- **Meta Tags**: Descriptive titles, descriptions, keywords for each page
- **Open Graph**: Social media preview cards
- **Canonical URLs**: Prevent duplicate content
- **Favicon**: `LogoImage.jpg`
- **Semantic HTML**: Proper heading hierarchy (`<h1>` per page)
- **Alt Text**: Descriptive image attributes

### Structured Data (JSON-LD)
- **Homepage**: `LocalBusiness` + `Service` schema
- **Industry Pages**: Service-specific schema with location data
- **Target**: Improve visibility in local "Knoxville" + "HVAC/Plumbing" searches

### Performance
- **No external CSS/JS dependencies** (except Google Fonts)
- **Small page sizes** (59KB for homepage HTML)
- **Embedded audio** (`call1.wav` - 4.5MB) loaded on-demand

---

## Branding

### Visual Identity
- **Logo**: Hybrid icon + text ("KnoxCalls" with orange "Calls")
- **Primary Color**: Vols Orange (#FF8200) - local branding (University of Tennessee)
- **Aesthetic**: Modern, contractor-focused, premium but approachable

### Voice/Tone
- **Direct, no-nonsense** language for contractors
- **Problem-aware**: Acknowledges pain points (exhaustion, missed revenue)
- **Local pride**: Knoxville-specific references

---

## Known Technical Debt

1. **CSS Duplication**: Each HTML file has embedded styles (no shared stylesheet)
   - **Impact**: Harder to maintain consistent styles across pages
   - **Fix**: Extract to `styles.css` or use CSS build step

2. **Mobile Nav Overlap (FIXED)**: Navigation was covering hero content on mobile
   - **Status**: Resolved by changing nav to `position: relative` on mobile

3. **No JavaScript Build Process**: All JS is inline, no minification/bundling
   - **Impact**: Larger HTML files, harder to debug
   - **Fix**: Add Vite or Parcel for build step

4. **No CSS Preprocessing**: No SCSS/Less/PostCSS
   - **Impact**: Can't use mixins, nesting, or autoprefixer
   - **Fix**: Add PostCSS or SCSS compiler

5. **Hardcoded Content**: No CMS or content management
   - **Impact**: Updates require code changes
   - **Fix**: Consider headless CMS (Contentful, Sanity) for marketing content

---

## Future Enhancements

### Short-term
- [ ] Extract CSS to external stylesheet (`styles.css`)
- [ ] Add form submission backend (Cloudflare Workers/Functions)
- [ ] Implement analytics (Plausible/Fathom for privacy)
- [ ] Add mobile hamburger menu animation

### Medium-term
- [ ] Migrate to static site generator (Astro, 11ty) for reusable components
- [ ] Add blog/resources section for SEO content
- [ ] Implement A/B testing for CTA buttons
- [ ] Add live chat widget integration

### Long-term
- [ ] Migrate to Next.js/Nuxt for dynamic content
- [ ] Build contractor dashboard integration
- [ ] Add real-time pilot spots counter (API-driven)

---

## Development Workflow

### Local Development
1. Clone repository: `git clone https://github.com/NanaHarith/knoxcalls-landing.git`
2. Open `public/index.html` in browser (no build step required)
3. Edit HTML/CSS/JS directly
4. Test responsiveness with browser dev tools

### Deployment
1. Commit changes to `main` branch
2. Push to GitHub: `git push origin main`
3. GitHub Actions automatically deploys to Cloudflare Pages
4. Verify at https://knoxcalls.com

### Branch Strategy
- **`main`**: Production branch (auto-deploys)
- Feature branches: Create for major changes, merge via PR

---

## Browser Support

### Target Browsers
- **Modern browsers** (Chrome, Firefox, Safari, Edge - last 2 versions)
- **Mobile browsers** (iOS Safari, Chrome Mobile)

### Key Features Used
- CSS Grid (widely supported)
- CSS Custom Properties (IE11 not supported - acceptable tradeoff)
- HTML5 Audio (fallback message for unsupported browsers)

---

## Contact & Ownership

**Project Owner**: KnoxCalls  
**Repository**: [NanaHarith/knoxcalls-landing](https://github.com/NanaHarith/knoxcalls-landing)  
**Product**: Night Watch (after-hours answering service)  
**Target Market**: Knoxville, TN contractors (HVAC, Plumbing, Refrigeration)

---

## Changelog

### 2026-01-17
- Fixed mobile navigation overlap issue (changed to relative positioning)
- Created technical architecture documentation

### Prior Releases
- A2P compliance updates (privacy policy, terms of service, opt-in consent)
- Logo updates (hybrid icon + text format)
- Dashboard integration references
