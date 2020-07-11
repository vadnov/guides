# How to run ASP.NET Core Website inside a VM

I use Ubuntu 20.04 (LTS) x64 as a base image.

### 1. Add ASP.NET Core Runtime

First, add a repository using these two commands in the terminal
```
wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb && \
sudo dpkg -i packages-microsoft-prod.deb
```
Next commands allow you to instal the runtime
```
sudo apt-get update; \
  sudo apt-get install -y apt-transport-https && \
  sudo apt-get update && \
  sudo apt-get install -y aspnetcore-runtime-3.1
```

You can verify the version of the runtime using the following command
```
dotnet --info
```

### 2. Enabling Kestrel as a web server

Once ASP.NET Core Runtime is installed we are ready to move forward with the web app.
As we are going to use Kestrel as a web server we need to allow it to bind on port 433. Let's do this with the following command:
```
setcap 'cap_net_bind_service=+ep' /usr/share/dotnet/dotnet
```

Now we need to copy our application to the server to run. There are mupliple ways to do so.
Let's assume you have your application copied to `/var/www/app`. We will create a service to run the app automatically.
Here is an example of the service description:
```
[Unit]
Description=Kesterl Web Service

[Service]
WorkingDirectory=/var/www/app/
ExecStart=/usr/bin/dotnet /var/www/app/%yourapp%.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-mailmock
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

Let's put this file to `/etc/systemd/system/kestrel-app.service`. Once we have the file in place we can install and start the service:
```
sudo systemctl daemon-reload && \
systemctl enable kestrel-app && \
systemctl start kestrel-app
```

Check the status of the service using the following command:
```
sudo systemctl status kestrel-app
```
