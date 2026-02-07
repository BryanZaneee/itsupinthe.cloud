# Migration Guide: Deploying Esmé Belle Portfolio to esmebelle.studio

This guide provides step-by-step instructions for deploying the archived Esmé Belle portfolio from `/var/www/itsupinthe.cloud/esme-archive/` to the esmebelle.studio domain.

## Prerequisites Checklist

Before beginning migration, ensure you have:

- [ ] Domain registration for esmebelle.studio completed
- [ ] DNS management access (registrar or Cloudflare)
- [ ] SSH access to the web server
- [ ] Root or sudo privileges on the server
- [ ] SSL certificate method chosen (Let's Encrypt recommended)
- [ ] Backup of current itsupinthe.cloud site

## Step 1: Server Setup

### 1.1 Create Directory Structure

```bash
sudo mkdir -p /var/www/esmebelle.studio
sudo chown www-data:www-data /var/www/esmebelle.studio
```

### 1.2 Copy Archive Content

```bash
cd /var/www/itsupinthe.cloud/esme-archive
sudo cp -r * /var/www/esmebelle.studio/
sudo chown -R www-data:www-data /var/www/esmebelle.studio
```

### 1.3 Verify Content

```bash
ls -la /var/www/esmebelle.studio
# Should show: index.html, about.html, assets/, CLAUDE.md, README.txt
```

## Step 2: Nginx Configuration

### 2.1 Create Virtual Host Configuration

```bash
sudo nano /etc/nginx/sites-available/esmebelle.studio
```

Add the following configuration:

```nginx
server {
    listen 80;
    listen [::]:80;
    server_name esmebelle.studio www.esmebelle.studio;

    root /var/www/esmebelle.studio;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    # Cache static assets
    location ~* \.(jpg|jpeg|png|gif|svg|ico|css|js)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }

    # Gzip compression
    gzip on;
    gzip_vary on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml image/svg+xml;
    gzip_min_length 1000;

    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
}
```

### 2.2 Enable Site

```bash
sudo ln -s /etc/nginx/sites-available/esmebelle.studio /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

## Step 3: DNS Configuration

### 3.1 Add A Records

In your DNS provider (registrar or Cloudflare), add:

```
Type: A
Name: @
Value: [YOUR_SERVER_IP]
TTL: Auto or 3600

Type: A
Name: www
Value: [YOUR_SERVER_IP]
TTL: Auto or 3600
```

### 3.2 Verify DNS Propagation

```bash
# Check DNS resolution (may take 5-60 minutes)
dig esmebelle.studio +short
dig www.esmebelle.studio +short
```

## Step 4: SSL/TLS Setup

### Option A: Let's Encrypt (Recommended)

```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d esmebelle.studio -d www.esmebelle.studio
```

Follow the prompts. Certbot will automatically update your nginx configuration.

### Option B: Cloudflare SSL

If using Cloudflare:

1. Enable SSL/TLS in Cloudflare dashboard
2. Set SSL mode to "Full (strict)"
3. Origin Server certificates: Create certificate in Cloudflare
4. Install origin certificate on server:

```bash
sudo nano /etc/nginx/ssl/esmebelle.studio.pem
# Paste certificate
sudo nano /etc/nginx/ssl/esmebelle.studio.key
# Paste private key
```

Update nginx config:

```nginx
listen 443 ssl http2;
listen [::]:443 ssl http2;
ssl_certificate /etc/nginx/ssl/esmebelle.studio.pem;
ssl_certificate_key /etc/nginx/ssl/esmebelle.studio.key;
```

## Step 5: Verification Steps

### 5.1 Test Domain Access

```bash
curl -I http://esmebelle.studio
# Should return 200 OK or 301 redirect to HTTPS

curl -I https://esmebelle.studio
# Should return 200 OK
```

### 5.2 Browser Testing

Visit in browser:
- [ ] https://esmebelle.studio - Portfolio loads correctly
- [ ] https://www.esmebelle.studio - Portfolio loads correctly
- [ ] About page works: https://esmebelle.studio/about.html
- [ ] All images load (check browser console for errors)
- [ ] Animations and interactive elements function
- [ ] Mobile responsive design works (test on phone)

### 5.3 Performance Check

```bash
curl -w "@-" -o /dev/null -s https://esmebelle.studio <<'EOF'
time_namelookup:  %{time_namelookup}\n
time_connect:     %{time_connect}\n
time_starttransfer: %{time_starttransfer}\n
time_total:       %{time_total}\n
EOF
```

Load time should be reasonable given the 520MB of assets.

## Step 6: Update Hub Page

Once esmebelle.studio is live and verified, update the hub page at itsupinthe.cloud.

### 6.1 Edit Hub Page

```bash
sudo nano /var/www/itsupinthe.cloud/index.html
```

Change the esmebelle.studio link from:

```html
<a href="/esme-archive/">esmebelle.studio</a>
```

To:

```html
<a href="https://esmebelle.studio">esmebelle.studio</a>
```

### 6.2 Test and Reload

```bash
sudo nginx -t
sudo systemctl reload nginx
```

Visit http://itsupinthe.cloud and verify the esmebelle.studio link now points to the live domain.

## Step 7: Cleanup (Optional)

After confirming esmebelle.studio works correctly for at least 2 weeks:

### 7.1 Remove Archive from itsupinthe.cloud

```bash
cd /var/www/itsupinthe.cloud
sudo rm -rf esme-archive/
```

### 7.2 Remove Assets from itsupinthe.cloud

```bash
cd /var/www/itsupinthe.cloud
sudo rm -rf assets/
```

This frees up 520MB of disk space.

### 7.3 Update Git Repository

```bash
cd /var/www/itsupinthe.cloud
git add index.html
git rm -r esme-archive/ assets/
git commit -m "Remove archive after successful esmebelle.studio migration"
git push origin main
```

## Rollback Procedures

### If esmebelle.studio has issues

1. Keep the archive at itsupinthe.cloud/esme-archive/ accessible
2. Fix issues at esmebelle.studio domain
3. Do not delete archive until esmebelle.studio is stable

### If you need to revert the hub page

```bash
cd /var/www/itsupinthe.cloud
sudo nano index.html
# Change link back to /esme-archive/
sudo systemctl reload nginx
```

## Git Repository Setup (Optional)

Consider creating a separate git repository for esmebelle.studio:

```bash
cd /var/www/esmebelle.studio
git init
git remote add origin https://github.com/BryanZaneee/esmebelle.studio.git
git add .
git commit -m "Initial commit: Esmé Belle portfolio"
git push -u origin main
```

## Troubleshooting

### Domain not resolving
- Check DNS propagation: https://dnschecker.org
- Verify A records point to correct IP
- Wait up to 48 hours for full propagation

### 502 Bad Gateway
- Check nginx is running: `sudo systemctl status nginx`
- Check nginx config: `sudo nginx -t`
- Check nginx error log: `sudo tail -f /var/log/nginx/error.log`

### SSL certificate issues
- Verify certificate installation: `sudo certbot certificates`
- Check certificate expiry
- Renew if needed: `sudo certbot renew`

### Assets not loading
- Check file permissions: `sudo ls -la /var/www/esmebelle.studio/assets/`
- Should be `www-data:www-data` with `644` for files, `755` for directories
- Fix: `sudo chown -R www-data:www-data /var/www/esmebelle.studio`

## Support

- Server Documentation: /var/www/esmebelle.studio/CLAUDE.md
- Archive Documentation: /var/www/itsupinthe.cloud/esme-archive/README.txt
- GitHub: https://github.com/BryanZaneee

## Migration Completion Checklist

- [ ] esmebelle.studio domain registered
- [ ] DNS configured and propagated
- [ ] Files copied to /var/www/esmebelle.studio
- [ ] Nginx virtual host created and enabled
- [ ] SSL certificate installed and working
- [ ] Portfolio loads correctly at https://esmebelle.studio
- [ ] All pages and assets verified
- [ ] Hub page at itsupinthe.cloud updated
- [ ] Archive kept for 2+ weeks before cleanup
- [ ] Git repository updated (optional)
- [ ] Monitoring configured (optional)

Once all items are checked, the migration is complete.
