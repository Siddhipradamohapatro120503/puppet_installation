# Apache Web Server Installation Guide using Puppet

This guide provides instructions for installing and running Apache web server using Puppet.

## Create the Puppet Manifest

1. Open the site.pp file for editing:

```bash
sudo nano /etc/puppetlabs/code/environments/production/manifests/site.pp
```

2. Add the following content to the file:

```puppet
# execute 'apt-get update'
exec { 'apt-update': # exec resource named 'apt-update'
  command => '/usr/bin/apt-get update' # command this resource will run
}

# install apache2 package
package { 'apache2':
  require => Exec['apt-update'], # require 'apt-update' before installing
  ensure => installed,
}

# ensure apache2 service is running
service { 'apache2':
  ensure => running,
}
```

3. Save and close the file.

## Run the Puppet Manifest

To initiate the Puppet agent and apply the manifest from the slave server, run the following command:

```bash
sudo /opt/puppetlabs/bin/puppet agent --test
```

This command will execute the Puppet agent, which will apply the configurations specified in the manifest, including updating the package list, installing Apache2, and ensuring the Apache2 service is running.
