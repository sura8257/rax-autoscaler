Quick Start
***********

Installation
============
::

  pip install RAX-AutoScaler

Configuration
=============

Edit config.json adding the following:
* API username
* API key
* Region name
* Autoscaling group section should contain:

  * AutoScale Group UUID
  * Scale Up Policy UUID
  * Scale Down Policy UUID
  * Check Type (agent.cpu, agent.load_average...)
  * Metric Name (depends on the check type)
  * Scale Up Threshold
  * Scale Down Threshold
  * Webhooks Url (Pre & Post commit url(s) for scale up/down)

Usage
=====

Once configured you can invoke the autoscaler.py script.

--cluster option should be used when this script actually runs on auto-scale group members. Otherwise if it is running on a dedicated management instance you do not require this option.

--as-group option should be used when you have multiple groups listed in the config.json file.

--config-file option should be used if config.json file does not exists in current directory or in '/etc/rax-autoscaler' path. 

Once tested you should configure this script to run as a cron job either on a management instance or on all cluster members

Cloud Init
==========

You can use the cloud-config file to auto-install RAX-Autoscaler on new servers.  For example to do so on Rackspace cloud using supernova

  *supernova <region> boot --user-data ./cloud-config --image <image id/name> --flavor <flavor id/name> --config-drive=true <server name>*

To use this with autoscale you would want to set the userdata of your launch configuration to the base64 encoded string of the file:

.. code-block:: json

  "launchConfiguration": {
        "args": {
            "server": {
                "config_drive" : true,
                "flavorRef": "general1-1",
                "imageRef": "CentOS 6.5 (PVHVM)",
                "key_name" : "MY_SSH_KEY",
                "user_data" : "I2Nsb3VkLWNvbmZpZwoKcGFja2FnZXM6CiAgLSBweXRob24tcGlwCgpydW5jbWQ6CiAgLSBbIHBpcCwgaW5zdGFsbCwgUkFYLUF1dG9TY2FsZXIgXQo=",
                "name": "test-autoscale"
            }
        },
        "type": "launch_server"
    }


This has been tested on these images:

- Ubuntu 14.04 LTS (PVHVM)
- CentOS 6.5 (PVHVM)

In the example the value of user_data contains the base64 encoded version of the following script:
::

  echo -n "I2Nsb3VkLWNvbmZpZwoKcGFja2FnZXM6CiAgLSBweXRob24tcGlwCgpydW5jbWQ6CiAgLSBbIHBpcCwgaW5zdGFsbCwgUkFYLUF1dG9TY2FsZXIgXQo=" | base64 -D


To base64 encode a script
::
cat /path/to/USER_DATA | base64

Size of USER_DATA can be reduced with gzip:
::
cat /path/to/USER_DATA | gzip | base64

Note
====
  RAX-AutoScaler depends on Rackspace Monitoring Agent to get the data from nodes in scaling group.
  
  If the agent is not installed please read: Install the Cloud Monitoring Agent: http://www.rackspace.com/knowledge_center/article/install-the-cloud-monitoring-agent


Contributing
============

- Fork it
- Create your feature branch (git checkout -b my-new-feature)
- Commit your changes (git commit -am 'Add some feature')
- Push to the branch (git push origin my-new-feature)
- Create new Pull Request

