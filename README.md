# Description

Installs and configures Network UPS Tools.

* [NUT](http://www.networkupstools.org)

# Requirements

* Chef 0.10.10+.

## Platform


* Debian, Ubuntu

Tested on:

* Debian 6.0 (Squeeze)
* Ubuntu 12.04 (Precise Penguin)

# Attributes

The main attributes are listed below. The complete list is documented in the metadata.rb and attributes/default.rb files included with the project.

* `node['nut']['mode']` - The mode determines which parts of NUT is to be started.
* `node['nut']['devices']` - Serial devices to change to the NUT group
* `node['nut']['ups']` - The driver settings for your UPS
* `node['nut']['users']` - Determines user access control, authentication and roles
* `node['nut']['monitors']` - The monitor configuration
* `node['nut']['listen']` - Array of ip addresses to listen to. Default only 127.0.0.1

# Usage

Add the nut recipe to your run list.

##### Using knife to add nut to the run list
	knife node run_list add [NODE] 'recipe[nut]'
	
##### Basic Node Structure
	{
		"nut": {
			"mode": "standalone",
	
			"devices": ["ttyS0"],

			"ups": {
				'cyberpower': {
					"driver": "powerpanel",
					"port": "/dev/ttyS0",
					"desc": "Cyberpower CP1500AVR"
				}
			},

			"users": {
				"vagrant": {
					"password": "vagrant",
					"upsmon master": true
				}
			},

			"monitors": {
				"cyberpower": {
					"system": "cyberpower@localhost",
					"power_value": 1,
					"username": "vagrant",
					"password": "vagrant",
					"role": "master"
				}
			}

		}
	}

#### Note about listen ip addresses.
By default upsd listen on 127.0.0.1.

If you want upsd to listen to on other than 127.0.0.1 interface or more that only 127.0.0.1.
Override this attribute in a role or environment.

		"nut": {
                    ...
                            "listen": [
                                    "127.0.0.1",
                                    "192.168.1.1"
                                  ],
                    ...
                        }



#### Note about users. 

If upsmon process must run in SLAVE mode, then set node attribute follows:
"users": {
				"vagrant": {
					"password": "vagrant",
					"upsmon": "slave"
				}
			},

"upsmon master": false IS INVALID attribute in attributes setup.

### Running inside vagrant

First you'll need to install [Virtual Box](https://www.virtualbox.org/), [Vagrant](http://vagrantup.com/) and a UPS

##### Create the cookbook directory
	 [ -s cookbooks/nut ] || (mkdir cookbooks && ln -s /vagrant cookbooks/nut)
	
##### Start Vagrant
	vagrant up
	
	