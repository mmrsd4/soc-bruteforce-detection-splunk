##### Ubuntu Server Setup (Target System)



* Install SSH Server

sudo apt update

sudo apt install openssh-server -y



* Verify SSH Service

sudo systemctl status ssh



* Enable SSH (if not running)

sudo systemctl enable ssh

sudo systemctl start ssh



* Check Auth Logs

sudo tail -f /var/log/auth.log



#### Purpose



This system acts as the target machine where failed SSH login attempts are logged.

