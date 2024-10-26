# Steps to deploy a on nginx

Steps:

- navigate to site-available folder and create a configuration file(same name as the site).
- link the file to enabled folder `sudo ln -s /etc/nginx/sites-available/test.xyz /etc/nginx/sites-enabled/`.
- create a folder and place the project the folder inside `/var/www/`.
- Copy and move the folder inside the folder (zip, unzip)`scp`
- configure the project file inside `site-available` folder.
- remove default file.
- install pm2 and run
- check by `sudo ngnix -t` command.
- restart ngnix by `sudo systemctl restart ngnix`

## install pm2

`sudo npm install -g pm2 // Install PM2 globally`

- build the project first then run

`pm2 start --name=my-next-app npm -- start // Start Next.js app with PM2`

`pm2 start "yarn start" --name e-commerce-Luxify --cwd /var/www/e-commerce-Luxify`

`pm2 startup systemd // Set up PM2 to start on boot`
