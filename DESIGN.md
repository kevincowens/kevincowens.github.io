---
name: Monokai Pro
description: A high-contrast, professional developer-focused theme inspired by Monokai Pro.
colors:
  bg: "#2d2a2e"
  surface: "#3b383e"
  border: "#4a474d"
  fg: "#fcfcfa"
  subtle: "#939293"
  green: "#a9dc76"
  yellow: "#ffd866"
  orange: "#fc9867"
  pink: "#ff6188"
  purple: "#ab9df2"
  cyan: "#78dce8"
typography:
  main:
    fontFamily: '"Fira Code", "JetBrains Mono", Consolas, monospace'
    fontSize: 1.05rem
    lineHeight: 1.6
  h1:
    fontSize: 2.4rem
    fontWeight: 600
  h2:
    fontSize: 1.9rem
    fontWeight: 600
  h3:
    fontSize: 1.5rem
    fontWeight: 600
  code:
    fontSize: 0.95rem
    fontFamily: '"Fira Code", "JetBrains Mono", Consolas, monospace'
rounded:
  sm: 4px
  md: 6px
spacing:
  xs: 0.4rem
  sm: 0.6rem
  md: 1rem
  lg: 2rem
components:
  heading-h1:
    textColor: "{colors.pink}"
  heading-h2:
    textColor: "{colors.orange}"
  heading-h3:
    textColor: "{colors.green}"
  link:
    textColor: "{colors.green}"
  link-hover:
    textColor: "{colors.yellow}"
  code-block:
    backgroundColor: "{colors.surface}"
    textColor: "{colors.green}"
    padding: "{spacing.md}"
    rounded: "{rounded.md}"
  button-primary:
    backgroundColor: "{colors.purple}"
    textColor: "{colors.bg}"
    rounded: "{rounded.md}"
    padding: 0.6rem 1.2rem
  button-primary-hover:
    backgroundColor: "{colors.pink}"
    textColor: "{colors.bg}"
---

## Overview
A professional, high-contrast theme designed for optimal developer focus and clarity, leveraging the classic Monokai Pro color palette.

## Colors
The palette utilizes deep, dark tones to reduce eye strain, punctuated by vibrant accents for high-priority syntax and interactive elements.

## Typography
Monospaced fonts are the standard to ensure consistent alignment and readability for code-heavy content, with scaled heading sizes to establish clear hierarchy.

## Layout & Spacing
A centered layout with a maximum width of 900px ensures comfortable reading lines. Components utilize a consistent spacing scale to maintain structural rhythm.

## Components
- **Headings:** Color-coded for quick visual scanning of documentation structure.
- **Buttons:** High-visibility primary actions using purple as the base, transitioning to pink on hover to provide clear interactive feedback.
- **Code:** Blocks are distinct from body text, using a surface-level background and a dedicated green accent for syntax highlighting.
