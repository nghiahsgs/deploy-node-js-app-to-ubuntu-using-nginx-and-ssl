# deploy-node-js-app-to-ubuntu-using-nginx-and-ssl
deploy node js app to ubuntu using nginx and ssl

*Should stop apache 2 and follow this instruction

## step 1: install node js
```
sudo apt update
sudo apt install nodejs
node --version
```
## step 2: install npm and pm2
```
sudo apt install npm
npm --version
npm i pm2 -g
```

## step 3: create simple project nodejs use express js
index.js
```js
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`)
})
```

```
npm init --y
node index.js
```
Run app using pm2, pm2 will run in background
```
pm2 start "node index.js"
```

## step 4: instasll nginx and config file
```
sudo apt install nginx
sudo vi /etc/nginx/sites-available/default
```
edit that module
```
server_name yourdomain.com www.yourdomain.com;

location / {
    proxy_pass http://localhost:3000; #whatever port your app runs on
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
}
```

save and restart nginx
```
# Check NGINX config
sudo nginx -t

# Restart NGINX
sudo service nginx restart
``

## step 5: change dns of domain
AFter change dns => Open browser and go to yourdomain.com to make a test

## step 6: Using cerbot to install ssl for yourdomain.com
Follow this instruction of cerbot for ubuntu and nginx 
```
https://certbot.eff.org/lets-encrypt/ubuntufocal-nginx
```

```
sudo snap install core; sudo snap refresh core
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo certbot --nginx
#Test automatic renewal
sudo certbot renew --dry-run
```
