# Spinning Up Stacks Using Heat
## TL;DR Remix
### Author: Pablo <paul.nelson@rackspace.com>

This repo is intended to provide a growing list of hands-on tutorials that each can be completed in 20 minutes or less, which start as simple as possible and continually build on the knowledge gained in previous tutorials.

The directory structure mimics college course numbering:

  * 100-level Tutorials are the basic building blocks of a Heat Stack: compute instances, load balancers, cloud databases, DNS
  * 200-level Tutorials start building real infrastructure by "wiring up" the basic building blocks introduced in the 100-level Tutorials
  * 300-level Tutorials introduce concepts like Software Configuration Management (SCM) and Auto-Scaling

Feel free to start with the tutorial that scratches your itch. If you find you don't understand something, look for an earlier tutorial to fill in any knowledge gaps.

Most of all, have fun!

</br>
## Prep Work

There are some things you will need to have sorted before you dive in. Making sure you have everything listed below in working order should allow you to move smoothly through the tutorials.

</br>
### 1. Prerequisites

  * [Python 2.7](http://www.python.org/download/releases/2.7/)
  * [pip](http://www.pip-installer.org/en/latest/installing.html/)
  * (optional) a [Pyhon Virtual Environment](http://docs.python-guide.org/en/latest/dev/virtualenvs/)

_If you're using a Mac, [homebrew](http://brew.sh/) is your friend._
</br>
</br>
### 2. This guide uses python-heatclient, so...

```shell
pip install python-heatclient
```

_If you're **not** using a virtual environment, you may need to start the above incantation with [sudo](http://xkcd.com/149/)._

More about python-heat-client can be found in [OpenStack's documentation](http://docs.openstack.org/developer/python-heatclient/).
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
OS_USERNAME="<your-rackspace-cloud-account-username>"
OS_PASSWORD="<your-rackspace-cloud-account-password>"
OS_TENANT_ID="<your-rackspace-cloud-account-tenant-id>"
OS_AUTH_URL="https://identity.api.rackspacecloud.com/v2.0/"
HEAT_URL="https://iad.orchestration.api.rackspacecloud.com/v1/${OS_TENANT_ID}"
```

_Refer to your particular OS for details on how to setup environment variables. These can also be passed as a **very** long list of command line parameters if you're the type that enjoys things like pokes in the eye with sharp sticks._
</br>
</br>
### 5. >>Sanity Check<< Local Env

```shell
heat -k stack-list
```

This should return a list of all of your stacks. If this is your first time using Heat, it should be an empty list.
</br>
</br>
### 6. CONGRATULATIONS! You're Done!

If you're not sure where to go next, try [the first tutorial](/101.Hello-Compute).
