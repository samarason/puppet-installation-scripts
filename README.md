# puppet-installation-scripts

=====================================================================================

## Bash scripts to install a Puppet Master and Puppet Agent on Linux based OS.

One script for the Puppet Master install  
One script for Puppet Agent install

It is assumed that a Linux host has been setup and is ready for the Puppet Master/s and the Agents installs.

# Puppet Overview

Puppet is a configuration management tool that uses a master-agent architecture. The Puppet Master manages configurations for multiple agents (client nodes).

Prerequisites  
    System Requirements

Operating System: Puppet Master must run on a UNIX variant (Linux)
            Memory and Disk space:
            See the official specs -- https://help.puppet.com/pe/current/topics/hardware_requirements.htm#hardware-requirements
            But, for a Small Standard installation of a Puppet master, a Minimum of 8 GB RAM is recommended and ca 60Gb of disk space for /opt and /var combined
            For the Puppet Agent, a Minimum of 4Gb RAM and ca 15Gb disk space for /opt and /var combined

        Network Configuration

            The install script ensures proper hostname resolution for the Puppet Master and agents using the  /etc/hosts file
            Set up NTP (Network Time Protocol) to keep time synchronized between the master and agents
            Make sure port 8140 is open in the firewall for communication between Master and Agents

=====================================================================================

# Installing Puppet Master

The Puppet Master is installed using the installer tarball and the pe.conf file for configuration.  
[Link: install-pe-using-installer-tarball](https://help.puppet.com/pe/current/topics/install-pe-using-installer-tarball.htm)

The template installs eyaml for yaml config encryption

[Link: hiera-eyaml](https://github.com/voxpupuli/hiera-eyaml)

[Link: howto eyaml](https://simp.readthedocs.io/en/master/HOWTO/20_Puppet/Hiera_eyaml.html)

And the script also sets up an example hiera configuration file that can be modified and used.  
[Link: eyaml-hiera-data](https://www.puppet.com/blog/puppet-eyaml-hiera-data)

## Install Puppet Master

This example uses onctl to create a Puppet Master VM with the hostname puppetmaster01 using the puppet-master.sh template

```bash
/bin/bash puppet-master.sh
```

After installation, remember to log on to the Puppet Master Console and change the admin password ! ( see the puppet-master.sh script for the default password)

=====================================================================================

# Install Puppet Agent

ONLY create a Puppet Agent instance **AFTER** a Puppet Master is installed & up and running.

### Agent Install procedure

In Puppet, the certificate name by default is the fqdn hostname.  
But, it can be configured with the certname parameter in Puppet.  
Setting the certname to a different value than the fqdn hostname won't break the certificate when the fqdn is changed,  
or the VM is deleted and the hostname and/or IP number is re-used by the cloud provider.  
Script Install procedure

1. Obtain the first 8 numbers of the servers swap UUID and use it with the hostname as a unique identifier for the certificate name
2. Create a csr attributes file and set the values for the certificate to identify the server
3. Install Puppet Agent repositories and install the Puppet Agent from a package
4. Set values in puppet.conf
5. Run the Puppet Agent to contact the Puppet Master
6. Now the sysadmin needs to sign the certificate on the Puppet Master

This example uses the puppet-agent.sh script to install a Puppet Agent with the MASTERIP and MASTER environment variables pointing to the Puppet Master that was previously installed.

```bash
/bin/bash puppet-agent.sh -i 192.168.0.22 -m puppetmaster.local
```

=====================================================================================

# Contributions

Fork it, clone it and submit.  
See instructions [Here](https://github.com/firstcontributions/first-contributions)
