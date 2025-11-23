# linux-firewall-daily-driver


<pre>
                                                         
                                                         
    ++=\\    //=++                                       
    ||  \\==//  ||                                       
    ||          ||                                       
    \\          //                                       
    \\          //                                       
     \\_      _//                                        
        \-__-/                                           

</pre>


## DETAILS
 **config:** linux firewall daily driver\
 **Author:** Alexander Edemalm\
 **Created:** June 14, 2025                                  

 
## ABOUT
This config establishes a default-deny firewall on your Linux system,\
blocking incoming connections by default. It also blocks outgoing\
connections on known dangerous ports to improve security for daily\
users.                                

### IMPORTANT: 
This configuration is intended for personal, personal systems and is not\
suitable for server or production setups. It aims to enhance security\
for individual users who may not have extensive firewall experience.                                             
                                                         
### DISCLAIMER: 
By using this configuration, you assume full responsibility for the\
security and integrity of your system. The author is not liable for any\
damage or loss caused by the use of this config.                       
                                                        
The user agrees to indemnify, defend, and hold harmless the author from\
any claims, damages, or legal liabilities arising from the use or misuse\
of this config.                                                 

## ⚙️ PREREQUSITES

Before using this config, ensure you have the following prerequisites\
installed:

- Linux system: Arch, Debian, Fedora Ubuntu, etc.

- nftables: This config uses nftables for firewall configuration.\
Make sure nftables is installed and active on your system.

### Install nftables (if not installed):
<pre>
  sudo pacman -S nftables                        # For: Arch
</pre>
<pre>
  sudo apt update && sudo apt install nftables   # For: Debian/Ubuntu
</pre>
<pre>
  sudo dnf install nftables                      # For: Fedora
</pre>

## 📥 INSTALLATION

### 1. Install nftables
First install nftables. Make sure to install it using the appropriate\
package manager for your system:


### 2. Install config file
The [nftables_conf](nftables_conf.nft) configuration file can be downloaded from this\
GitHub repository. To obtain it, simply clone the repository\
or download the file directly.


### 3. Edit config file
The nftables_conf.nft configuration requires you to specify\
your network interface. Follow these steps to configure it.

#### 3.1 check LOCAL IFACE
Use the following command to list all network devices and check\
which one is currently active:
<pre>
  nmcli device status
</pre>

**NOTE:** 
Look under the DEVICE column and identify the network device that\
is responsible for your active network connection (e.g.,wlps0). 

#### 3.2 Paste device name
1. Open the [nftables_conf](nftables_conf.nft) file in a text editor of your choice.

2. Scroll down to the section labeled "GLOBAL VARIABLES".

3. Replace the placeholder "\<YOUR LOCAL INTERFACE\>" with the actual\
network device name that you copied in step 3.1.\
\
For Example:
<pre>
  LOCAL_IFACE="wlp2s0"
</pre>

**NOTE:** Don't forget to save your changes.

### 4. Move file
The file should be stored safely, without, risk of being removed or\
accidentally modified.
<pre>
  mv /path/to/nftables_conf.nft /desired/location/nftables_conf.nft
</pre>
**NOTE**: Replace /path/to and /desired/location/ with the actual\
path where the configuration file located and you wish to\
move to.


### 5. Configure Systemd
This edit of nftables service, creates an override in which can be\
undone and reset simply by, removing the override.\

1. Edit to make override
<pre>
  sudo systemctl edit nftables.service
</pre>

**NOTE:** Repeat following step to undo changes, of override and\
restore to default. Simply, remove the content pasted in step 5,2.

2. Systemd Override
<pre>
  [Service]
  # Clear previous ExecStart and ExecReload
  ExecStart=
  ExecReload=

  # New path for nftables configuration
  ExecStart=/usr/sbin/nft -f /desired/location/nftables_conf.nft
  ExecReload=/usr/sbin/nft -f /desired/location/nftables_conf.nft
</pre>
**NOTE**: Replace /path/to and /desired/location/ with the actual\
path where the configuration file located and you wish to\
move to.


3. Restart systemd service
<pre>
  sudo systemctl daemon-reload
  sudo systemctl restart nftables
</pre>


### 6. Start on boot(optional)
<pre>
  sudo systemctl enable nftables
</pre>


### 7. Verify changes
<pre>
  sudo nft list ruleset
</pre>
**NOTE**: This should result in a large print...\
Look if rules matches, the configs files rules to be sure.

## 📜 LICENSE

This project is licensed under the MIT License. 
See the [LICENSE](LICENSE) file. file for more information.
