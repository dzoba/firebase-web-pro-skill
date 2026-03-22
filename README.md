# Firebase Web Pro Skill

A Claude Code skill that ensures your Firebase + Vite + React + Tailwind websites ship with all the finishing touches — OG tags, favicons, 404 pages, security rules, mobile nav, and polished design.

## Install

**For all your projects (personal scope):**

```bash
git clone https://github.com/dzoba/firebase-web-pro-skill.git ~/.claude/skills/firebase-web-pro
```

**Or for a single project only:**

```bash
git clone https://github.com/dzoba/firebase-web-pro-skill.git .claude/skills/firebase-web-pro
```

That's it — no config needed. Claude Code automatically discovers skills from these directories. The skill will activate when you're building a web app with Firebase, React, or similar stacks.

## What it does

When you ask Claude Code to build a website, this skill ensures nothing gets forgotten:

- **Open Graph tags** on every page (PNG images only — never SVG)
- **Page titles** per route (no leftover "Vite + React" defaults)
- **404 page** with proper styling and navigation
- **Favicon** replacement (no Vite logo shipping to prod)
- **Firebase security rules** that deny by default
- **Mobile navigation** that actually works on phones
- **Visual polish** — hero sections, loading states, transitions

It works in two phases: during planning it adds missing items to your plan, and after building it self-checks the codebase against a checklist.
