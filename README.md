# centos-nginx-setup
Setting up Nginx as reverse proxy and configuring firewall on CentOS
# Lab for Setting up Nginx as Reverse Proxy and Configuring Firewall on CentOS

## Objective:
This lab involves setting up Nginx as a reverse proxy for a Go application and configuring the firewall on CentOS to allow specific incoming ports (22, 80, and 8081).

## Steps:

### 1. Update System and Install Necessary Packages

```bash
sudo yum update -y
sudo yum install -y nginx firewalld git
2. Install and Configure Nginx
•	Start and enable nginx:
•	sudo systemctl start nginx
•	sudo systemctl enable nginx
•	Check Nginx status:
•	sudo systemctl status nginx
•	Edit the /etc/nginx/nginx.conf file to add a reverse proxy configuration:
•	sudo nano /etc/nginx/nginx.conf
Add the following within the server block:
location / {
    proxy_pass http://localhost:8081;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
•	Restart Nginx:
•	sudo systemctl restart nginx
3. Configure Firewall
•	Start and enable firewalld:
•	sudo systemctl start firewalld
•	sudo systemctl enable firewalld
•	Add rules for SSH (port 22), HTTP (port 80), and GoApp (port 8081):
•	sudo firewall-cmd --zone=public --add-port=22/tcp --permanent
•	sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
•	sudo firewall-cmd --zone=public --add-port=8081/tcp --permanent
•	Reload the firewall to apply the changes:
•	sudo firewall-cmd --reload
•	Verify firewall settings:
•	sudo firewall-cmd --list-all
4. Start Go Application
•	Navigate to your Go application directory:
•	cd /home/bob/go-app/
•	Start the Go application:
•	nohup go run main.go &
•	Check if the application is running by tailing the nohup.out:
•	tail -f nohup.out
5. Test Application Access
•	Once the GoApp is running, open a browser and navigate to: http://<your-server-ip>
•	Login with the credentials username: test, password: test.
Conclusion:
This lab successfully set up Nginx as a reverse proxy for the GoApp and configured a firewall on CentOS to allow specific incoming ports. The GoApp is accessible via port 8081, and the firewall ensures that only the necessary ports (22, 80, and 8081) are open.

