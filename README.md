# Spinning Up Stacks Using Heat
## TL;DR Remix
### Author: Pablo <paul.nelson@rackspace.com>

This repo is ___not___ intended to be comprehensive documentation, though we do our best to make sure references to comprehensive documentation are provided along the way. An excellent overall reference is the <a href="http://docs.openstack.org/developer/heat/template_guide/" target="_blank">OpenStack Developer Heat Template Guide</a>.

This repo ___is___ intended to provide a growing list of hands-on tutorials that each can be completed in 20 minutes or less, which start as simple as possible and continually build on the knowledge gained in previous tutorials.

The directory structure mimics college course numbering:

  * 100-level Tutorials are the basic building blocks of a Heat Stack: compute instances, load balancers, cloud databases, DNS
  * 200-level Tutorials start building real infrastructure by "wiring up" the basic building blocks introduced in the 100-level Tutorials
  * 300-level Tutorials introduce concepts like Software Configuration Management (SCM) and Auto-Scaling

Feel free to start with the tutorial that scratches your itch. If you find you don't understand something, look for an earlier tutorial to fill in any knowledge gaps. If you don't find something you're looking for feel free to submit your ideas via email or, if you're so inclined, fork the repo and submit a Pull Request!

Most of all, have fun!

</br>
## Prep Work

There are some things you will need to have sorted before you dive in. Making sure you have everything listed below in working order should allow you to move smoothly through the tutorials.

</br>
### 1. Prerequisites

  * <a href="http://www.python.org/download/releases/2.7/" target="_blank">Python 2.7</a>
  * <a href="http://www.pip-installer.org/en/latest/installing.html/" target="_blank">pip</a>
  * (optional) a <a href="http://docs.python-guide.org/en/latest/dev/virtualenvs/" target="_blank">Python Virtual Environment</a>

*If you're using a Mac, <a href="http://brew.sh/" target="_blank">homebrew</a> is your friend.*
</br>
</br>
### 2. This guide uses python-heatclient, so...

```shell
pip install python-heatclient
```

*If you're __not__ using a virtual environment, you may need to start the above incantation with <a href="http://xkcd.com/149/" target="_blank">sudo</a>.*

More about python-heat-client can be found in <a href="http://docs.openstack.org/developer/python-heatclient/" target="_blank">OpenStack's documentation</a>.
</br>
</br>
### 3. >>Sanity Check<< Command Line Works

```shell
heat help
```

This should give you a long list of all the options available from the heat command
</br>
</br>
### 4. Environment Variables Make Things Easier

```shell
export OS_USERNAME="<your-rackspace-cloud-account-username>"
export OS_PASSWORD="<your-rackspace-cloud-account-password>"
export OS_TENANT_ID="<your-rackspace-cloud-account-tenant-id>"
export OS_AUTH_URL="https://identity.api.rackspacecloud.com/v2.0/"
export HEAT_URL="https://iad.orchestration.api.rackspacecloud.com/v1/${OS_TENANT_ID}"
```

_Refer to your particular OS for details on how to setup environment variables. These can also be passed as a **very** long list of command line parameters if you're the type that enjoys things like pokes in the eye with sharp sticks._
</br>
</br>
### 5. >>Sanity Check<< Local Env

```shell
heat stack-list
```

This should return a list of all of your stacks. If this is your first time using Heat, it should be an empty list.
</br>
</br>
### 6. CONGRATULATIONS! You're Done!

If you're not sure where to go next, try [the first tutorial](/101.Hello-Compute).
