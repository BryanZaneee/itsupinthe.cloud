# itsupinthe.cloud Hub Page

Minimal hub page linking to Bryan Zane's projects. Static HTML served by nginx.

## Files

| Path | Purpose | When to read |
|------|---------|--------------|
| `index.html` | Hub page with centered text and links | Modifying hub design or links |
| `esme-archive/` | Archived Esmé Belle portfolio (2,775 lines, 520MB assets) | Deploying to esmebelle.studio |
| `esme-archive/README.txt` | Archive documentation and rollback instructions | Understanding archive or restoring portfolio |
| `MIGRATION-TO-ESMEBELLE.md` | Step-by-step guide for deploying archive to esmebelle.studio domain | Planning esmebelle.studio deployment |
| `package.json` | Project metadata and dev server script | Running local dev server |

## Architecture

**Type**: Static single-page hub
**Design**: Pure white background, centered text, system fonts, no external dependencies
**Links**: bryanzane.com, esmebelle.studio (/esme-archive/ until domain deployed), GitHub
**Server**: Nginx serving static HTML at /var/www/itsupinthe.cloud
**Performance**: <100ms load time (no external resources)

## Development

```bash
# Local preview
cd /var/www/itsupinthe.cloud
python3 -m http.server 8080
# Visit http://localhost:8080

# Deploy changes
sudo chown www-data:www-data index.html
sudo chmod 644 index.html
sudo nginx -t && sudo systemctl reload nginx
```

## Archive

Esmé Belle portfolio preserved in `esme-archive/` for future migration to esmebelle.studio domain.
See `MIGRATION-TO-ESMEBELLE.md` for deployment instructions.
Archive accessible at http://itsupinthe.cloud/esme-archive/ until migration completes.

## Git

Repository: https://github.com/BryanZaneee/esme
History preserved from original Esmé Belle portfolio.
