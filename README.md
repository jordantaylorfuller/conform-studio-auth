# Conform Studio OAuth Callback

This repository hosts the OAuth callback handler for Conform Studio desktop application.

## Live URL
https://auth.conform.studio

## Purpose
Handles OAuth redirects from authentication providers (Google, Apple, etc.) and redirects users back to the desktop application via deep links.

## Local Testing
Open `index.html` in a browser and append test parameters:
- Success: `?code=test123`
- Error: `?error=access_denied&error_description=User cancelled`

## Deployment
This site is automatically deployed via GitHub Pages when changes are pushed to the main branch.