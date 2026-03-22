---
name: firebase-web-pro
description: >
  Production-ready Firebase + Vite + React + Tailwind website development. Ensures Claude
  never forgets the details that make a site feel finished: Open Graph tags (PNG, not SVG),
  page titles, 404 pages, Firebase security rules, mobile navigation, and polished visual design.
  Use this skill whenever the user wants to build, scaffold, or plan a website or web app using
  Firebase, Vite, React, or Tailwind — even if they don't mention all four. Also trigger when the
  user asks to "build a site," "create a web app," "set up a project," or "make a landing page"
  and Firebase or React is part of their stack. If in doubt, use the skill — it's better to have
  the checklist and not need it than to forget OG tags again.
---

# Firebase + Vite + React + Tailwind — Production Web Skill

You're building a website with Firebase, Vite, React, and Tailwind CSS. This skill exists because
it's easy to get the core features working and then forget all the "finishing touches" that make a
site feel real — things like Open Graph tags, a 404 page, Firebase security rules, or decent mobile
navigation. Users end up having to follow up repeatedly about stuff that should've been there from
the start.

This skill has two jobs:
1. **During planning**: Make sure the plan includes everything below so nothing gets forgotten.
2. **After building**: Self-check the codebase against a checklist to catch anything that slipped through.

---

## The Stack

Always use this stack unless the user says otherwise:
- **Vite** as the build tool
- **React** (functional components, hooks)
- **Tailwind CSS** for styling
- **Firebase** for backend (Firestore, Auth, Hosting, etc. as needed)

When scaffolding a new project: `npm create vite@latest` with the React template, then add Tailwind and Firebase.

---

## Things Claude Forgets (And Must Not)

### 1. Open Graph Tags

Social platforms use OG tags to generate link previews. Without them, shared links look like bare URLs — which makes the site look unfinished or sketchy.

**Every page needs these in the `<head>`:**
```html
<meta property="og:title" content="Page Title — Site Name" />
<meta property="og:description" content="A compelling 1-2 sentence description" />
<meta property="og:image" content="https://example.com/og-image.png" />
<meta property="og:url" content="https://example.com/page" />
<meta property="og:type" content="website" />
<meta name="twitter:card" content="summary_large_image" />
```

**Critical rules for OG images:**
- Use **PNG or JPG only**. Never SVG — most social platforms (Facebook, Twitter/X, LinkedIn, Slack, Discord) cannot render SVG in previews. This is a common mistake. If you need to generate an OG image programmatically, output it as PNG.
- Recommended size: **1200×630px** (works across all major platforms).
- Always use an absolute URL for the image, not a relative path.

For React SPAs, use `react-helmet-async` or a similar library to manage `<head>` tags per route. If using server-side rendering or static generation, set them at build time.

**Write good descriptions.** Don't just echo the page title. Think about what would make someone click the link when they see it in a Slack channel or group chat. Be specific and appealing.

### 2. Page Titles

Every route needs a meaningful `<title>`. Format: `Page Name — Site Name` (or `Site Name | Page Name`, just be consistent). Use `react-helmet-async` so each route sets its own title.

Don't leave the default "Vite + React" title. This is the first thing someone sees in their browser tab, and it signals whether the site is amateur or professional.

### 3. The 404 Page

Create a proper 404 page with:
- A clear "page not found" message
- Navigation back to the home page (or other key pages)
- Visual design consistent with the rest of the site (don't just render a plain text error)

In React Router, this means a catch-all route:
```jsx
<Route path="*" element={<NotFound />} />
```

### 4. Favicon

The default Vite favicon (the Vite logo) screams "this developer didn't finish the job." Replace it before anything else ships.

- Replace `public/vite.svg` with a real favicon — either one the user provides, or generate a simple, on-brand one.
- Use PNG format (at least 32×32, ideally include 16×16, 32×32, and 180×180 for Apple touch icon).
- Update `index.html` to reference the new favicon:
  ```html
  <link rel="icon" type="image/png" href="/favicon.png" />
  <link rel="apple-touch-icon" href="/apple-touch-icon.png" />
  ```
- Remove any leftover `vite.svg` from `public/`.
- If generating a favicon, keep it simple — an initial, a geometric shape in the brand color, or a small icon that reads well at 16px. Don't try to cram a detailed illustration into 32 pixels.

### 5. Firebase Security Rules

This is the one Claude forgets most often, and it's the most dangerous to forget. An app with no security rules (or with `allow read, write: if true`) is wide open to anyone with the Firestore URL.

**Always create `firestore.rules`** with rules scoped to the app's auth model. At minimum:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Default: deny everything
    match /{document=**} {
      allow read, write: if false;
    }

    // Then open up specific collections based on auth
    // Example: users can read/write their own data
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

Think about the data model and write rules that actually match it. Don't leave placeholder rules. If you're unsure about the auth model, ask the user before shipping open rules.

Also create `storage.rules` if Firebase Storage is used. Same principle — deny by default, open up specifically.

Include rules deployment in `firebase.json`.

### 6. Mobile-First Design & Navigation

Build mobile-first. Tailwind makes this easy — start with the mobile layout and use `sm:`, `md:`, `lg:` breakpoints to enhance for larger screens.

**Mobile navigation is its own thing.** Don't just hide the desktop nav behind a hamburger menu and call it done. Think about:
- A hamburger or slide-out menu that feels native
- Touch-friendly tap targets (minimum 44×44px)
- Smooth open/close transitions
- The menu should close when a link is tapped
- Consider a bottom navigation bar for apps with 3-5 main sections (this is what users expect from mobile apps)

Test the layout mentally at 375px wide (iPhone SE) — if it breaks there, it breaks for a lot of people.

### 7. Visual Design & Polish

The user cares about aesthetics. Don't build something that "works but looks like a bootstrap template from 2015."

**Home page matters most.** It's the first impression. Make it attractive:
- Clear hero section with a compelling headline
- Visual hierarchy — the eye should flow naturally
- Intentional use of whitespace
- Consistent color palette (pick 1 primary, 1 accent, and stick to them)
- Nice typography — consider using a Google Font that fits the vibe

**General design principles:**
- Use Tailwind's built-in design tokens (colors, spacing, shadows) for consistency
- Add subtle transitions and hover states — they make things feel alive
- Loading states: don't show blank screens while data loads. Use skeletons or spinners.
- Rounded corners, soft shadows, and generous padding go a long way
- Icons from `lucide-react` or `react-icons` rather than raw text where appropriate

---

## Planning Phase

When the user asks to build a site (or you're planning one), make sure the plan explicitly includes:

1. **Project setup**: Vite + React + Tailwind + Firebase init
2. **Routing**: All pages including the 404 catch-all
3. **Head management**: OG tags and page titles for every route
4. **Favicon**: Replace the Vite default with a real favicon
5. **Firebase security rules**: Written for the actual data model
6. **Mobile navigation**: A real mobile nav component, not just a hidden desktop nav
7. **Home page design**: Hero, layout, visual appeal
8. **OG image**: Generate or source a real PNG image (1200×630)

If any of these aren't in the plan, add them before writing code.

---

## Self-Check (After Building)

After the code is written, go through this checklist by actually reading the files:

- [ ] Every route has a `<title>` set via react-helmet-async (or equivalent)
- [ ] Every route has OG meta tags (title, description, image, url, type)
- [ ] OG image is PNG or JPG, not SVG — and the URL is absolute
- [ ] OG descriptions are compelling, not just the page title repeated
- [ ] Favicon has been replaced — no Vite logo remains in `public/`
- [ ] `index.html` references the new favicon (not `vite.svg`)
- [ ] There's a 404 catch-all route with a styled NotFound component
- [ ] `firestore.rules` exists and denies by default
- [ ] Security rules are specific to the data model (not `allow read, write: if true`)
- [ ] `storage.rules` exists if Firebase Storage is used
- [ ] Mobile nav exists and is a proper component (hamburger/slide-out/bottom nav)
- [ ] Layout looks good at 375px width
- [ ] Home page has a clear hero and visual hierarchy
- [ ] No default "Vite + React" titles left anywhere
- [ ] Loading states exist for async data
- [ ] `firebase.json` includes rules deployment config

Read the actual files and verify each item. If something's missing, fix it — don't just report it.
