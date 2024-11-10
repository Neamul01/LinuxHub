# Steps to deploy Spring Boot Application

## Basic flow to deploy a application

As we need database connection in the backend application so we need to deploy database first and took the connection on the backend configuration. </br>
Then Deploy backend as Front-end need backend url or credentials. </br>
And at last Deploy the front-end application.

- Deploy database
- Deploy backend
- Deploy front end

## Stage A: Install Dependencies and configure Database

### Step 1. Clone the project.

Copy paste or clone your project to `/var/www` folder or local folder.

### Step 2: Install mysql database and create user

Install mysql in the vm, the [Digital Ocean document](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04) document is good to follow as it is updated.
</br>
Now create user and give required privileges,

```bash
eCREATE DATABASE studentdb;
CREATE USER 'student_user'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Example#4321';
GRANT ALL PRIVILEGES ON studentdb.* TO 'student_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### Step 3. Install OpenJDK v18 or higher.

Install OpenJDK 18 or higher. You can follow [this documentation](https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-22-04).

### Step 4. Install Maven

Now we have to install Maven. You can install using [this documentation](https://www.digitalocean.com/community/tutorials/install-maven-linux-ubuntu), or use the below commands.

```bash
wget https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz
 tar -xvf apache-maven-3.9.9-bin.tar.gz
 mv apache-maven-3.9.9 /opt
 sudo mv apache-maven-3.9.9 /opt
 M2_HOME='/opt/apache-maven-3.9.9/'
 PATH="$M2_HOME/bin:$PATH"
```

## Stage B: Install Dependencies and Deploy Backend

Before deploying Backend make sure some things, </br>

- make sure all `.env` are updated as we setup Database ex. 'user', 'password' of Database.

### Step 5: Build the Backend Application

Now go to project directory and `run mvn clean install -DskipTests`, it will create a jar file
after making .jar file find the file and move the jar file to `"mv student-0.0.1-SNAPSHOT.jar /var/www/html"`

### Step 6: Systemd file for the backend app

Now we can run and test the backend application locally.
</br>

To up and run the backend application always we have to create a systemd file and and `start` and `enable` the application.
</br>

- Create systemd file, feel free to follow [this doc]()
- Note Make sure your JDK and Maven path `which java` `which mvn` for systemd service.
- Create the systemd file, below command and code.
  </br>
  `sudo vim /etc/systemd/system/lendingapp.service
`

```bash
[Unit]
Description=Lending Application

[Service]
Type=simple
Restart=always
User=root
Group=www-data

# Define the log files
StandardOutput=append:/var/log/lendingapp.service.log
StandardError=append:/var/log/lendingapp.service.error.log

# Project root path
WorkingDirectory=/var/www/html/

# Command to execute the JAR file
ExecStart=/usr/local/jdk-18/bin/java -jar /var/www/html/student-0.0.1-SNAPSHOT.jar

[Install]
WantedBy=multi-user.target
```

Now reload systemd deamon.</br>
`sudo systemctl daemon-reload`

Enable the service so that it can automatically run if server restart or any other issue faced.</br>
`sudo systemctl enable lendingapp.service`

Start the service </br>
`sudo systemctl start lendingapp.service`

Check the status of the service </br>
`sudo systemctl status lendingapp.service`

## Stage C: Build and run Front-end

Now the last step, we have to run and deploy the front-end
</br>

Before doing anything we have to first check and fix the environment files and update them with new credentials.

### Step 7: Install dependencies and run, build, deploy the Application.

Now install node, we can install it using NVM (node version manager). </br>
Follow the steps below or follow the [documentation](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04)

- Download: `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash`
- Source (you can use your shell, here using bash): `source ~/.bashrc`
- Check node remote versions: `nvm list-remote`
- Install specific version as your requirement: `nvm install v18.18.2`
- All done!! now check the version: `node -v`

NB: We can install multiple versions then use specific version `nvm use v18.10.0`

### Install dependencies:

Here angular: `sudo npm install -g @angular/cli`

### check and build

- Now Install the dependencies by running `npm install`
- And build the application `npm run build`

### Move the dist file to nginx directory and restart nginx

- Move the dist file: `sudo mv dist/ /var/www/html/`
- Restart nginx server: `systemctl restart nginx.service`
  </br>
  </br>
  </br>
  `Congratulations!!! now our deployment is done.`
