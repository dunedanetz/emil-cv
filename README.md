# emil-cv

Personal page of Emil Iliev, served at **https://emil.chiliseeds-shop.com**.

## Stack
- Static HTML/CSS (Bootstrap 5 + GLightbox via CDN), no build step
- Nginx on a DigitalOcean VPS, separate server block and Let's Encrypt certificate
- GitHub Actions deploys with `rsync --delete` on every push to `main`

## Deploy security
The workflow logs in as a dedicated `deploy-cv` user whose SSH key is locked down with
`restrict` + `rrsync` in `authorized_keys`: it can only rsync into `/var/www/emil-cv`,
with no shell, no port forwarding and no access to the rest of the server.
The VPS host key is pinned via a secret instead of disabling host key checking.
