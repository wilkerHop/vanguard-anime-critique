# The Aesthetics of Subversion

A GitHub Pages website featuring a comprehensive methodology for Vanguard Anime Critique, with a detailed case study of Fullmetal Alchemist.

## Overview

This website presents **"The Aesthetics of Subversion"** - a framework for exhaustive, ideological, and structural anime franchise discourse. The application is designed as a "Critical Cockpit" with modular sections for exploring analytical methodology, case studies, and synthesis tools.

## Features

### Interactive Sections

1. **The Methodology** - Establishes the Vanguard framework with:
   - The Vanguard Manifesto (Ideological Subversion, Subjective-Objective Harmony, Institutional Critique)
   - Facet Analysis Framework with radar chart visualization
   - Rhetorical Shift comparator showing standard vs. vanguard critique styles

2. **FMA Case Study** - Applies the methodology to Fullmetal Alchemist:
   - Franchise metrics with bar chart visualization
   - Production & Authority analysis (Auteur vs. Fidelity)
   - Comparative Divergence Matrix exploring philosophical differences between FMA 2003 and Brotherhood

3. **Vanguard Synthesis** - Summary toolkit including:
   - Critique checklist
   - Critical vocabulary
   - Final output format guidelines

### Technical Stack

- **HTML/CSS/JavaScript** - Pure vanilla implementation
- **Tailwind CSS** - Utility-first CSS framework (via CDN)
- **Chart.js** - Interactive data visualizations
- **Plotly** - Advanced plotting library
- **Google Fonts** - Playfair Display (serif) and Inter (sans-serif)

### Design Palette

**"Academic Avant-Garde"** theme:
- Warm Neutral (#F2F0E9) - Background
- Dark Charcoal (#2D2D2A) - Primary text
- Muted Red (#8C2F2F) - Accent for subversion
- Slate Blue (#4B5563) - Structural elements

## Local Development

Simply open `index.html` in any modern web browser. No build process required.

```bash
# Open in default browser (macOS)
open index.html

# Or serve with Python
python3 -m http.server 8000
# Then visit http://localhost:8000
```

## Deployment

This website is configured for automatic deployment to GitHub Pages using GitHub Actions.

### Setup Instructions

1. **Create a GitHub repository** for this project
2. **Push the code** to the repository:
   ```bash
   git init
   git add .
   git commit -m "Initial commit: Vanguard Anime Critique website"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
   git push -u origin main
   ```

3. **Enable GitHub Pages**:
   - Go to repository Settings → Pages
   - Under "Build and deployment", select **Source: GitHub Actions**

4. **Automatic Deployment**:
   - The included workflow (`.github/workflows/deploy.yml`) will automatically deploy on every push to main
   - Your site will be available at: `https://YOUR_USERNAME.github.io/YOUR_REPO_NAME/`

## Project Structure

```
.
├── index.html              # Main application file
├── README.md              # This file
└── .github/
    └── workflows/
        └── deploy.yml     # GitHub Actions deployment workflow
```

## Features Breakdown

### Data Visualizations

- **Radar Chart**: Compares Vanguard Analysis vs. Standard Review across 5 facets (Aesthetic Function, Narrative Structure, Thematic/Ideological, Production/Auteur, Historical Context)
- **Bar Chart**: Displays Fullmetal Alchemist franchise metrics (ratings and episodes)

### Interactive Elements

- **Tab Navigation**: Seamless switching between three main sections
- **Rhetorical Shift Toggles**: Compare standard review vs. vanguard critique approaches
- **Matrix Buttons**: Explore ideological differences across Truth, Sin, and History themes

## Browser Compatibility

Compatible with all modern browsers:
- Chrome/Edge (recommended)
- Firefox
- Safari

## License

© 2023 Vanguard Analytical Framework. Based on "The Aesthetics of Subversion".

## Contributing

This is a demonstration project. Feel free to fork and adapt the methodology for your own critical analysis frameworks.
