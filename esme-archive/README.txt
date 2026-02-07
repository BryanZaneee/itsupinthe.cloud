# Esmé Belle Portfolio Archive

This directory contains the complete Esmé Belle portfolio website that was originally hosted at itsupinthe.cloud.

## Archive Date
February 7, 2026

## Contents
- index.html (2,775 lines) - Main portfolio page
- about.html (304 lines) - About page
- assets/ (520MB) - All artwork images, decorative elements, and animations
- CLAUDE.md - Original documentation

## Purpose
This content is preserved for future migration to esmebelle.studio domain.
See /var/www/itsupinthe.cloud/MIGRATION-TO-ESMEBELLE.md for deployment instructions.

## Temporary Access
Until esmebelle.studio is deployed, this archive is accessible at:
http://itsupinthe.cloud/esme-archive/

## Rollback Instructions
If you need to restore the Esmé Belle portfolio as the main site:

```bash
cd /var/www/itsupinthe.cloud
cp esme-archive/index.html ./
cp esme-archive/about.html ./
cp esme-archive/CLAUDE.md ./
chown www-data:www-data index.html about.html CLAUDE.md
chmod 644 index.html about.html CLAUDE.md
nginx -t && systemctl reload nginx
```

Then visit http://itsupinthe.cloud to verify the portfolio is restored.

## Git History
All git history is preserved in the parent repository at /var/www/itsupinthe.cloud/.git

## Questions
Contact: Bryan Zane
GitHub: https://github.com/BryanZaneee
