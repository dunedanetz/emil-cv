# Nginx config: emil-cv

`emil-cv.conf` is the source of truth for `/etc/nginx/sites-available/emil-cv` on the VPS
(`root@chili-shop`, 207.154.254.174). It includes the TLS lines managed by Certbot.

This file is **not deployed automatically**: applying Nginx config needs root, and CI
does not get root on the server. Apply changes by hand.

## Check for drift (📍 ubuntu-devops)
```bash
ssh root@207.154.254.174 "cat /etc/nginx/sites-available/emil-cv" | diff - nginx/emil-cv.conf && echo "in sync"
```

## Apply a change (📍 ubuntu-devops)
```bash
scp nginx/emil-cv.conf root@207.154.254.174:/etc/nginx/sites-available/emil-cv
ssh root@207.154.254.174 "nginx -t && systemctl reload nginx"
```
`nginx -t` validates the config first; if it fails, Nginx keeps running the old config.
If a reload does break something, restore the previous version with `git show HEAD~1:nginx/emil-cv.conf`.

## After Certbot changes the file
`certbot --nginx` edits the file on the server. Copy it back into git afterwards:
```bash
ssh root@207.154.254.174 "cat /etc/nginx/sites-available/emil-cv" > nginx/emil-cv.conf
```
