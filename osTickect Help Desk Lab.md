# osTicket Help Desk
### Purpose
We will be deploying osTicket, an open-source help desk ticketing system using Docker.

We'll have a fully deployed help desk portal where users can submit tickets
A fully working help desk portal where users can submit tickets
■ An admin panel where you can triage, respond to, and resolve tickets
■ Real hands-on experience with Docker containers and MySQL databases
■ A portfolio project you can show employers and put on GitHub

### Requirements
Windows: at least Windows 10 22H2
<!--Mac: -->

### Part 1 - Docker Install
#### Step 1- Install Docker Desktop
We're going to run two Docker containers - one for the osTicket application itself and the other to hold a MySQL database to store all tickets and user data

Docker runs like mini-computers that you can turn on and off.
You can download the installer file here: [Docker Installer for Windows](https://docs.docker.com/desktop/setup/install/windows-install/)

***Note:*** You may need to install Hyper-V or WSL (Windows Subsystem for Linux) for Docker to run because it requires a native Linux environment to run. Installing either of these satisfies this requirement for Windows installs. Mac doesn't require this since it is a Unix-based system.

<!-- ### Part 1 - Install Docker
**Personal use:** To change drive, type in `[DRIVE_LETTER]:` e.g changing from `C:` to do `D:`, open command prompt as administrator and type `D:`

Install WSL if using Windows since Docker runs on a Linux backend
#### Installing Docker on a different drive
Open command prompt as an administrator and change the drive. Navigate to the where the Docker Installer file is located
-->

<!--
``` powershell

# 1. Redirect temp files off C: so the installer doesn't need C: space
New-Item -ItemType Directory -Path "D:\Temp" -Force
$env:TEMP = "D:\Temp"
$env:TMP = "D:\Temp"

# 2. Make sure WSL2 is the default
wsl --set-default-version 2

# 3. Go to wherever you downloaded the installer, e.g.:
cd $env:USERPROFILE\Downloads

# command in number 3 may switch the folder where you downloaded it, so navigate back to folder with installer file

# 4. Run the installer pointing everything to D:
.\"Docker Desktop Installer.exe" install --accept-license --backend=wsl-2 --installation-dir="D:\Docker\Program" --wsl-default-data-root="D:\Docker\wsl"
```
-->

#### Step 2 - Verify Docker Install
To verify docker is installed, open command prompt/terminal and run `docker --version` and `docker-compose --version` 

![](attachments/Pasted%20image%2020260807032516.png)

### Part 2 - Project Folder Setup
#### Step 3 - Create osticket-lab Folder
Navigate to where you want to create and save your folder. Use the command `cd` to ensure you're in the correct folder

![](attachments/Pasted%20image%2020260807033355.png)

#### Step 4 - Create .env file
Create a new file and in the file, put in:
```
MYSQL_ROOT_PASSWORD=osticketpass
MYSQL_DATABASE=osticket
MYSQL_USER=osticketuser
MYSQL_PASSWORD=osticketpass
EOF
```

Save the file with the name `.env`
Make sure it is saved in the osticket-lab folder

#### Step 5- Create docker-compose.yml File
Next create another file and paste in:
``` yml
services:

  osticket:

    image: devinsolutions/osticket:latest

    platform: linux/amd64

    ports:

      - "8080:80"

    environment:

      MYSQL_HOST: mysql

      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}

      MYSQL_DATABASE: ${MYSQL_DATABASE}

      MYSQL_USER: ${MYSQL_USER}

      MYSQL_PASSWORD: ${MYSQL_PASSWORD}

    depends_on:

      - mysql

  

  mysql:

    image: mysql:5.7

    platform: linux/amd64

    environment:

      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}

      MYSQL_DATABASE: ${MYSQL_DATABASE}

      MYSQL_USER: ${MYSQL_USER}

      MYSQL_PASSWORD: ${MYSQL_PASSWORD}

    volumes:

      - mysql_data:/var/lib/mysql

  

volumes:

  mysql_data:
```

Save it with the name `docker-compose.yml`
Make sure it is saved in the osticket-lab folder

*Insert explanation of what the file is actually doing*

### Part 4: Launch Lab
#### Step 8 - Start the containers
Use `cd` to make sure you're in the project folder. Then run `docker-compose up -d`
![](attachments/Pasted%20image%2020260807035653.png)

#### Step 9 - Verify Containers Are Running
Use `docker ps` to verify
![](attachments/Pasted%20image%2020260807040554.png)

Status should be showing 'Up (healthy)' 

### Part 5 - Access osTicket in Browser
#### Step 10 - Open user Portal
The link for the user portal is http://localhost:8080/ 
Below is what the support center homepage looks like:
![](attachments/Pasted%20image%2020260807041745.png)

#### Step 11 - Open the Admin Panel
The link for the admin panel is http://localhost:8080/scp/login.php
Username: `ostadmin` Password: `Admin1`
![](attachments/Pasted%20image%2020260807041707.png)