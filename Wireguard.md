# ***How To Set Up Wireguard Using Docker***

1. **Create a DigitalOcean account or login with an existing account**

2. **When on the welcome screen, click the DigitalOcean logo to go to your dashboard**

3. **From the dashboard, select your project and select "Get started with a Droplet"**

4. **Configure Droplet:**
- *Change the version of Ubuntu to "20.04 (LTS) x64"*
- *Change the CPU option to "Regular with SSD"*
- *Choose "New York" as the datacenter region used*
- *Create password (used Version2000SysAdminBot)*
- ***Create Droplet or refer to Optional configuration***

***Optional:*** 
- *Change hostname to "Wireguard-Lab"*

5. **Launch the console for your new Droplet**

6. **Download Docker & Docker Compose**
- *Install required tools*
```sh
# sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
```
- *Add Docker key*
```sh
# curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
- *Add Docker repository*
```sh
# sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
- *Switch to the Docker repository*
```sh
# apt-cache policy docker-ce
```
- *Install Docker*
```sh
# sudo apt install docker-ce -y
```
- *Install Docker Compose*
```sh
# sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
- *Set execute permissions for docker-compose*
```sh
# sudo chmod +x /usr/local/bin/docker-compose
```