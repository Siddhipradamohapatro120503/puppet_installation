# Puppet Installation and Configuration Guide

## Introduction

Puppet is an open-source configuration management tool that helps system administrators automate the provisioning, configuration, and management of servers. This guide will walk you through the installation and basic configuration of Puppet on both the master and client nodes.

## Prerequisites

- Ubuntu 22.04 (Jammy) or compatible system
- Sudo privileges on both master and client nodes
- Stable network connection between master and client nodes

## Puppet Master Installation

### Step 1: Set up hostnames

On both the Puppet master and client, edit the `/etc/hosts` file:

```bash
sudo vim /etc/hosts
```

Add the following lines, replacing the IP addresses with your actual server IPs:

```
[puppet master ip] puppetmaster
[puppet client ip] puppetclient
```

### Step 2: Install Puppet Server

On the Puppet master node:

```bash
wget https://apt.puppetlabs.com/puppet8-release-jammy.deb
sudo dpkg -i puppet8-release-jammy.deb
sudo apt update
sudo apt install puppetserver
```

### Step 3: Configure Puppet Server

Edit the Puppet server configuration:

```bash
sudo nano /etc/default/puppetserver
```

Adjust the memory allocation if needed.

### Step 4: Start Puppet Server

```bash
sudo systemctl start puppetserver
sudo systemctl enable puppetserver
```

Verify the status:

```bash
sudo systemctl status puppetserver
```

### Step 5: Configure Firewall

Allow incoming connections on port 8140:

```bash
sudo ufw allow 8140/tcp
```

### Step 6: Manage Certificates

List all certificates:

```bash
sudo /opt/puppetlabs/bin/puppetserver ca list --all
```

Sign all pending certificate requests:

```bash
sudo /opt/puppetlabs/bin/puppetserver ca sign --all
```

## Puppet Agent Installation

### Step 1: Install Puppet Agent

On the Puppet client node:

```bash
wget https://apt.puppetlabs.com/puppet8-release-jammy.deb
sudo dpkg -i puppet8-release-jammy.deb
sudo apt update
sudo apt install puppet-agent
```

### Step 2: Configure Puppet Agent

Edit the Puppet configuration file:

```bash
sudo nano /etc/puppetlabs/puppet/puppet.conf
```

Add the following lines:

```ini
[main]
server = puppetmaster
```

### Step 3: Start Puppet Agent

```bash
sudo systemctl start puppet
sudo systemctl enable puppet
```

Verify the status:

```bash
sudo systemctl status puppet
```

### Step 4: Test the Connection

Run the Puppet agent to test the connection to the master:

```bash
sudo /opt/puppetlabs/bin/puppet agent --test
```

## Troubleshooting

If you encounter issues with certificate signing:

1. On the Puppet master, list pending certificate requests:

   ```bash
   sudo /opt/puppetlabs/bin/puppetserver ca list
   ```

2. Sign the specific certificate:

   ```bash
   sudo /opt/puppetlabs/bin/puppetserver ca sign --certname <agent-hostname>
   ```

3. On the Puppet agent, re-run the test:

   ```bash
   sudo /opt/puppetlabs/bin/puppet agent --test
   ```

## Conclusion

You have now set up a basic Puppet infrastructure with a master and client node. From here, you can start defining resources and creating modules to manage your infrastructure configuration.

Remember to consult the official Puppet documentation for more advanced configurations and best practices.
