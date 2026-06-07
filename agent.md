# Quarto Workshop Website — Full Build Specification

## Project Overview

A static Quarto website to manage and display workshop materials for a 4-year project. The site is read-only for visitors — no backend, no authentication, no interactivity beyond navigation, search, and media playback. All content is authored in `.qmd` files and rendered to static HTML.

---

## Technology Stack

- **Framework:** [Quarto](https://quarto.org/) (static site generator)
- **Slide format:** Quarto Revealjs (`.qmd` with `format: revealjs`)
- **Hosting:** GitHub Pages or Quarto Pub (both free, both static)
- **Version control:** Git + GitHub
- **Video hosting:** YouTube (embedded via Quarto shortcode)
- **No database, no backend, no CMS**

---

## Project File Structure

```
project/
├── _quarto.yml                  # site config, navbar, global options
├── index.qmd                    # landing page
├── license.qmd                  # license page
├── styles.css                   # custom styling
│
├── assets/
│   └── images/
│       ├── educator-01.jpg
│       ├── educator-02.jpg
│       └── educator-03.jpg
│
└── workshops/
    ├── workshop-01/
    │   ├── index.qmd            # workshop overview + all materials
    │   ├── slides.qmd           # revealjs slides
    │   ├── analysis.R           # downloadable R script
    │   └── reference.pdf        # embedded PDF
    │
    ├── workshop-02/
    │   ├── index.qmd
    │   ├── slides.qmd
    │   ├── analysis.R
    │   └── reference.pdf
    │
    └── workshop-NN/
        ├── index.qmd
        ├── slides.qmd
        ├── analysis.R
        └── reference.pdf
```

**Notes:**
- Not every workshop needs all files. Omit any file that does not apply.
- Multiple R scripts per workshop: name them `script-01.R`, `script-02.R`, etc.
- Multiple PDFs per workshop: name them `reference-01.pdf`, `reference-02.pdf`, etc.
- Videos are YouTube links embedded in `index.qmd` — no local video files.

---

## `_quarto.yml` — Full Configuration

```yaml
project:
  type: website

website:
  title: "Project Name"
  search: true
  navbar:
    left:
      - href: index.qmd
        text: Home
      - text: Workshops
        menu:
          - href: workshops/workshop-01/index.qmd
            text: "Workshop 01 - Topic Name"
          - href: workshops/workshop-02/index.qmd
            text: "Workshop 02 - Topic Name"
          - href: workshops/workshop-03/index.qmd
            text: "Workshop 03 - Topic Name"
          # Add one entry per workshop
    right:
      - href: license.qmd
        text: License
      - icon: github
        href: https://github.com/your-org/your-repo
        aria-label: GitHub

format:
  html:
    css: styles.css
    toc: true
    toc-depth: 2
    code-tools: true          # enables download button on code blocks
    code-copy: true           # copy button on all code blocks
  revealjs:
    theme: default
    slide-number: true
    chalkboard: false
    preview-links: true
```

---

## `index.qmd` — Landing Page

The landing page has the following sections in order:

1. Hero / introduction
2. Intro video embed
3. Educator profiles
4. Student / participant list
5. Workshop schedule (auto-generated listing)

```markdown
---
title: "Project Name"
subtitle: "A 4-year workshop series on [topic]"
listing:
  id: workshop-listing
  contents: workshops/*/index.qmd
  type: table
  fields: [date, title, facilitator]
  sort: "date asc"
  date-format: "DD MMM YYYY"
---

## About the Project

Brief description of the project, its goals, duration, and funding body if applicable.

---

## Introduction Video

{{< video https://www.youtube.com/watch?v=VIDEO_ID >}}

---

## Educators

::: {.grid}

::: {.g-col-4}
![Jane Doe](assets/images/educator-01.jpg){width=150 style="border-radius: 50%;"}

**Jane Doe**  
*Lead Facilitator*  
Short bio. Affiliation, expertise, contact or link.
:::

::: {.g-col-4}
![John Smith](assets/images/educator-02.jpg){width=150 style="border-radius: 50%;"}

**John Smith**  
*Data Science Instructor*  
Short bio. Affiliation, expertise, contact or link.
:::

::: {.g-col-4}
![Maria Lopez](assets/images/educator-03.jpg){width=150 style="border-radius: 50%;"}

**Maria Lopez**  
*R Programming Instructor*  
Short bio. Affiliation, expertise, contact or link.
:::

:::

---

## Participants

| Name | Affiliation | Cohort |
|---|---|---|
| Alice Brown | University A | 2024 |
| Bob Chen | Institute B | 2024 |
| Carol Davis | Organisation C | 2025 |

---

## Workshop Schedule

::: {#workshop-listing}
:::
```

---

## Workshop `index.qmd` — Per Workshop Page

Each workshop has its own `index.qmd` with YAML front matter driving the listing and a body containing all materials.

```markdown
---
title: "Workshop 01 - Topic Name"
date: 2024-03-01
facilitator: "Jane Doe"
categories: [r-basics, data-cleaning]
description: "Short one-line description shown in the schedule listing."
---

## Schedule

| Time | Topic | Facilitator |
|---|---|---|
| 09:00 - 09:30 | Introduction | Jane Doe |
| 09:30 - 10:30 | Data Cleaning in R | John Smith |
| 10:30 - 10:45 | Break | |
| 10:45 - 12:00 | Hands-on Exercise | Jane Doe |

---

## Video Recording

{{< video https://www.youtube.com/watch?v=VIDEO_ID >}}

---

## Slides

[View Slides](slides.qmd){.btn .btn-primary}

---

## R Code

```{.r filename="analysis.R"}
library(tidyverse)

df <- read_csv("data.csv")

df |>
  ggplot(aes(x = var1, y = var2)) +
  geom_point()
```

[Download R Script](analysis.R){.btn .btn-outline-primary}

---

## Reference Document

<iframe src="reference.pdf" width="100%" height="600px" style="border: none;"></iframe>

[Download PDF](reference.pdf){.btn .btn-outline-primary}
```

**YAML front matter fields explained:**
- `title` — displayed on the page and in the listing table
- `date` — used to sort workshops chronologically in the listing
- `facilitator` — displayed as a column in the listing table
- `categories` — enables filtering if you add a filter bar to the listing
- `description` — shown as a subtitle in the listing table

---

## `slides.qmd` — Revealjs Slides

Each workshop's slides are a separate `.qmd` file with `format: revealjs`.

```markdown
---
title: "Workshop 01 - Topic Name"
subtitle: "Subtitle if needed"
author: "Jane Doe"
date: 2024-03-01
format:
  revealjs:
    theme: default
    slide-number: true
    logo: ../../assets/images/logo.png   # optional
    footer: "Project Name — Workshop 01"
---

## Introduction

- Point one
- Point two
- Point three

---

## Data Cleaning

Explain the concept here.

```{.r}
df |> 
  drop_na() |>
  filter(value > 0)
```

---

## Visualisation

```{.r}
df |>
  ggplot(aes(x, y)) +
  geom_point() +
  theme_minimal()
```

---

## Summary

- Key takeaway one
- Key takeaway two
- Key takeaway three
```

**Notes:**
- Each `##` heading creates a new horizontal slide
- Use `---` alone on a line for a blank slide separator
- Code blocks in slides display only — they do not execute at runtime
- The slides page is linked from the workshop `index.qmd` via a button

---

## `license.qmd` — License Page

```markdown
---
title: "License"
---

## License

This work is licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

You are free to share and adapt the material for any purpose, provided appropriate credit is given.

© Project Name, 2024–2028.
```

---

## `styles.css` — Custom Styling

```css
/* Educator profile grid spacing */
.grid {
  gap: 2rem;
  margin-bottom: 2rem;
}

/* Educator photo alignment */
.g-col-4 img {
  display: block;
  margin: 0 auto 1rem auto;
}

.g-col-4 {
  text-align: center;
}

/* Button styling */
.btn {
  margin-right: 0.5rem;
  margin-bottom: 0.5rem;
}

/* PDF iframe */
iframe {
  margin-bottom: 1rem;
}

/* Workshop listing table */
.listing-table {
  font-size: 0.95rem;
}
```

---

## Adding a New Workshop — Checklist

When a new workshop is added, do the following:

1. Create folder `workshops/workshop-NN/`
2. Create `index.qmd` with correct YAML front matter (title, date, facilitator, description)
3. Create `slides.qmd` with revealjs format
4. Add `.R` script files if applicable
5. Add `.pdf` files if applicable
6. Add YouTube video ID to `index.qmd` once recording is uploaded
7. Add the workshop entry to the `Workshops` dropdown menu in `_quarto.yml`
8. Run `quarto render` to rebuild the site
9. Push to GitHub — site redeploys automatically if GitHub Actions is configured

---

## GitHub Actions — Auto Deploy to GitHub Pages

Create `.github/workflows/publish.yml`:

```yaml
on:
  push:
    branches: [main]

name: Render and Publish

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Check out repository
        uses: actions/checkout@v4

      - name: Set up Quarto
        uses: quarto-dev/quarto-actions/setup@v2

      - name: Render and Publish
        uses: quarto-dev/quarto-actions/publish@v2
        with:
          target: gh-pages
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

This renders and deploys the site automatically on every push to `main`.

---

## Rendering Locally

```bash
# Install Quarto: https://quarto.org/docs/get-started/
quarto preview        # live preview in browser
quarto render         # full render to _site/
```

---

## Constraints and Limitations to Communicate to the AI

- Do not introduce any backend, database, or server-side logic
- Do not use JavaScript frameworks (React, Vue, etc.)
- Do not use `format: html` for slides — always `format: revealjs`
- Do not use `shiny` or interactive R — all code is display-only
- PDF embedding uses raw HTML `<iframe>` — this is intentional
- Video embedding uses Quarto native shortcode `{{< video url >}}` — not raw iframe
- All image paths are relative to the file they are referenced from
- The listing block in `index.qmd` auto-generates from workshop front matter — do not hardcode the schedule table manually
- `code-tools: true` in `_quarto.yml` enables the download button on code blocks globally — do not add custom download logic for R scripts shown in code blocks
- Keep all files within the project directory — no absolute paths
