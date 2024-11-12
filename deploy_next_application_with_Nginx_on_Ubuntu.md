# Steps to deploy a Next.js full stack app on nginx

we are using `pm2` to manage and keep running the application up.

### Step 1: install required dependencies </br>

install required dependencies, here I am using ubuntu server:

- install nginx: `sudo apt install nginx`
- install node using nvm: [follow this](https://github.com/Neamul01/LinuxHub/blob/main/install_node.md)
- install pm2: `npm install pm2 -g`

### Step 2: Install dependencies and build the project.

- start the project using `pm2` using below command. </br>

```bash
pm2 start npm --name project_name -- start
```

This command means pm2 will run npm start command and give a name our project as project_name so we can find this project among others easily.
</br>

To start the application whenever server restarts run the command `pm2 save`

### NB: Check the the pm2 log to ensure the application is running successfully.

- check list `pm2 list`
- check log `pm2 logs <id>` (update id with the id running your app)

### Step 3: Configure nginx file

create a file inside the `sites-available` file by using below command:

```bash
/etc/nginx/sites-available/<file_name>`
```

NB: make sure the file name and your domain name/ project name are same for future usability.
</br>

configure the file:

```bash
server{
    root /var/www/<file_name>;
    index index.html index.nginx-debian.html;

    server_name <domain_name> www.<domain_name>;

    access_log /var/log/nginx/<domain_name>.access.log;
	error_log /var/log/nginx/<domain_name>.error.log;

    location /_next/static/ {
        alias /var/www/<file_name>/.next/static/;
    }

    location / {
        proxy_pass http://<server_ip>:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

NB: don't forgot to update the `file_name`, `server_ip` and `domain_name`

NB: Don't forgot to remove the `default` file from `sites-available` and `sites-enabled` or it will cause unnecessary time.

### Congratulations we have now deployed our application, now visit the domain or ip to see live site.
