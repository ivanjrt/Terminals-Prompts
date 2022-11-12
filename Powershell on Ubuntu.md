Go to this link: https://learn.microsoft.com/en-us/powershell/scripting/install/install-ubuntu
<br/>
follow the **_sh_** Instructions Middle part.<br/>
This might change, therefore going to the website source is encouraged
```bash
# Update the list of packages
sudo apt-get update
# Install pre-requisite packages.
sudo apt-get install -y wget apt-transport-https software-properties-common
# Download the Microsoft repository GPG keys
wget -q "https://packages.microsoft.com/config/ubuntu/$(lsb_release -rs)/packages-microsoft-prod.deb"
# Register the Microsoft repository GPG keys
sudo dpkg -i packages-microsoft-prod.deb
# Update the list of packages after we added packages.microsoft.com
sudo apt-get update
# Install PowerShell
sudo apt-get install -y powershell
# Start PowerShell
pwsh
```

Tip: When a newer version notification, you can ctrl + click , and it will take you to the repo<br/>
![image](https://user-images.githubusercontent.com/44326428/201497941-22ae6052-c974-451a-a5ae-edb7f67fd7a1.png)
