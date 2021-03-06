# Virtual development machine for the metacpan project

- You will need:

    - [Vagrant](http://www.vagrantup.com/downloads.html) (1.2.2 or later)
    - [VirtualBox](https://www.virtualbox.org/), we recommend [4.3.10](https://www.virtualbox.org/wiki/Download_Old_Builds), see [guest additions](http://stackoverflow.com/questions/22717428/vagrant-error-failed-to-mount-folders-in-linux-guest) if you get mounting issues
    - [A git client](http://git-scm.com/downloads)
    - An ssh client if not build in, [Windows users see
      this](http://docs-v1.vagrantup.com/v1/docs/getting-started/ssh.html).
    - To be able to download about 900MB of data on the first run

-  Check out this repo.

    ```bash
    git clone git://github.com/CPAN-API/metacpan-developer.git
    ```

-  Set Up repositories
   ````bash
    cd metacpan-developer
    sh bin/init.sh
   ```

- Start the virtual machine (first run will download our .box disk image
  ~900MB)

    ```bash
    vagrant up
    ```

- Run all the extra bits that were added after we created the .box file (you
might need to run this each time you start up the machine or if we have made
further changes)

    ```bash
    vagrant provision
    ```

If you get this error when provisioning "err: Could not request certificate: Connection refused - connect(2)"
then add the following to your /etc/resolv.conf as the first nameserver:

    ```bash
    nameserver 8.8.8.8
    ```

So, your /etc/resolv.conf should look something like

    ```bash
    domain home
    search home
    nameserver 8.8.8.8
    nameserver 10.0.2.3
    ```

- Connect to the vm

    ```bash
    vagrant ssh
    sudo su -     # to become root if you need it
    ```

- To edit and test

    Your workflow from this point will be to edit the MetaCPAN repositories
    which you have just checkout out to your local machine.  After you have
    made your changes, you can ssh to your box and restart the relevant
    services.  You will not develop on the VM directly.

    To install any missing (newly required) perl modules, as root run

    ```bash
    cd <to the mount as listed below>
    /home/metacpan/bin/install_modules --installdeps .
    ```

    - metacpan-web is the web front end
        - mounted as /home/metacpan/metacpan.org
        - service metacpan-www restart
    - cpan-api is the backend that talks to the elasticsearch
        - mounted as /home/metacpan/api.metacpan.org
        - service metacpan-api restart
    - metacpan-puppet is the sysadmin/server setup
        - mounted as /etc/puppet
        - /etc/puppet/run.sh

    Make changes in your checked out 'metacpan' repos and restart the service

- To connect to services on the VM

    WEB: [http://localhost:5001/](http://localhost:5001/)

    API: [http://localhost:5000/](http://localhost:5000/)

    SSH: `vagrant ssh`

- Running the metacpan-web test suite

    You'll want to run the suite at least once before getting started to make sure the VM has a clean bill of health.

    ```bash
    vagrant ssh

    cd /home/metacpan/metacpan.org
    ./bin/prove t
    ```

    If you're not planning to work on the API itself, congratulations!
    You're ready to start hacking.

### More documentation

 * [SETTING UP / TESTING THE API](README_API.md) page
 * [HELP](HELP.md) page (including VM tuning notes)

### Problems?
 * If you have trouble mounting the folders, check this fix for [guest additions](http://stackoverflow.com/questions/22717428/vagrant-error-failed-to-mount-folders-in-linux-guest)
 * Ask on #metacpan (irc.perl.org), or open an [issue](https://github.com/CPAN-API/metacpan-developer/issues)


