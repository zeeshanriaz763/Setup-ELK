# TASK: Setup and document ELK with metric beats.

### Course Title: DeVops and Cloud Computing
### Group Members: Sammreen Amir    Zeeshan Riaz


## Step 1: Update All installed packages on a system 
bash
yum update --nogpgcheck

## Step 2: Install and run Docker (running commands one by one)
bash
sudo dnf install -y yum-utils device-mapper-persistent-data lvm2
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf install -y docker-ce docker-ce-cli containerd.io
sudo systemctl start docker
sudo systemctl enable docker
sudo docker run hello-world
docker ps



## Step 3: Install and run nginx
bash
sudo dnf update -y
sudo dnf install -y nginx
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx


## Step 4: Check Memory Usage
bash
free -m


## Step 5: Check Disk Space
bash
df -h


Note: Ensure you have enough disk space available for the installation.
if Not, then Create and Enable a Swap File.

## Step 6: Create a Swap File
bash
sudo fallocate -l 2G /swapfile



## Step 7: Set the Correct Permissions
bash
sudo chmod 600 /swapfile


## Step 8: Set Up the Swap Space
bash
sudo mkswap /swapfile


## Step 9: Enable the Swap Space
bash
sudo swapon /swapfile


## Step 10: Make the Change Permanent
bash
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

## Step 11: Check and Verify Swap Space
bash
sudo swapon --show
free -m


## Step 12: Install Java (Elasticsearch requirement)
bash
sudo dnf install java-11-openjdk-devel -y


## Step 13: Add the Elasticsearch GPG Key and Repository
bash
sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
cat <<EOF | sudo tee /etc/yum.repos.d/elasticsearch.repo
[elasticsearch]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
EOF



## Step 14: Install Elasticsearch
bash
sudo dnf install elasticsearch -y


## Step 15: Start and Enable Elasticsearch
bash
sudo systemctl enable elasticsearch --now



## Step 16: Add the Kibana repository
bash
cat <<EOF | sudo tee /etc/yum.repos.d/kibana.repo
[kibana]
name=Kibana repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
EOF


## Step 17: Install Kibana
bash
sudo dnf install kibana -y


## Step 18: Start and Enable Kibana
bash
sudo systemctl enable kibana --now


## Step 19: Add the Metricbeat repository
bash
cat <<EOF | sudo tee /etc/yum.repos.d/metricbeat.repo
[metricbeat]
name=Metricbeat repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
EOF



## Step 20: Install Metricbeats
bash
sudo dnf install metricbeat -y



## Step 21: Configure Metricbeats
To set up the connection to Elasticsearch and Kibana. You can open the file using a text editor:
bash
sudo vi /etc/metricbeat/metricbeat.yml


Ensure the following lines are configured properly:
output.elasticsearch:
  hosts: ["http://localhost:9200"]
setup.kibana:
  host: "http://localhost:5601"

## Step 22: Start and Enable Metricbeat
bash
sudo systemctl enable metricbeat --now



## Step 23: Install and Use firewalld
bash
sudo dnf install firewalld -y
sudo systemctl start firewalld
sudo systemctl enable firewalld
sudo firewall-cmd --permanent --add-port=9200/tcp
sudo firewall-cmd --permanent --add-port=5601/tcp
sudo firewall-cmd --reload



## Step 24: Verify Installation
bash
sudo systemctl status elasticsearch
sudo systemctl status kibana
sudo systemctl status metricbeat



## Step 25: Accessing Kibana
You should now be able to access Kibana by navigating to http://<your-ip>:5601 in your web browser.

 Now check these on your web browser
103.151.111.237:9200
103.151.111.237:5601
