# ***How To Set Up Wireguard Using Docker***

## ****Create a Droplet****

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

## ***Download Docker***

1. **Launch the console for your new Droplet**

1. **Download Docker & Docker Compose**
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

## ***Setup Wireguard***

1. **Make Wireguard directories**
```sh
# mkdir -p wireguard/config/
```
2. **Create .yml file for docker compose and add the following content**
- *Command*
```sh
# nano wireguard/docker-compose.yml
```
- *Content (Edit TZ to your timezone, SERVEURL to your Droplet server IP)*
```
    version: '3.8'
    services:
    wireguard:
        container_name: wireguard
        image: linuxserver/wireguard
        environment:
        - PUID=1000
        - PGID=1000
        - TZ=America/Chicago
        - SERVERURL=165.227.79.88
        - SERVERPORT=51820
        - PEERS=pc1,pc2,phone1
        - PEERDNS=auto
        - INTERNAL_SUBNET=10.0.0.0
        ports:
        - 51820:51820/udp
        volumes:
        - type: bind
            source: ./config/
            target: /config/
        - type: bind
            source: /lib/modules
            target: /lib/modules
        restart: always
        cap_add:
        - NET_ADMIN
        - SYS_MODULE
        sysctls:
        - net.ipv4.conf.all.src_valid_mark=1
```

1. **test**