# Ruby on Rails Development Environment

### Stack versions
- chruby: 0.3.9
- imagemagick: latest from ubuntu repository
- nodejs version: latest from official nodejs repository
- phantomjs: 1.9.8
- postgresql : 9.4.x from official postgresql repository
- ruby: 2.2.1
- ruby-install: 0.5.0

### Set up the environment

1. Install [VirtualBox](https://www.virtualbox.org/)
2. Install [Ansible](http://www.ansible.com/)
3. Clone this repository
4. You must set your workspace directory in `Vagrantfile`. Change the ``"."`` in the following line, to the directory where your source code is:

  Change:
  ```ruby
  config.vm.synced_folder ".", "/vagrant", nfs: true
  ```

  to something like:
  ```ruby
  config.vm.synced_folder "/home/my-user/workspace/my-project-src", "/Workspace", nfs: true
  ```

5. This playbook already creates a database user for us, check the username and password [here](https://github.com/dbalexandre/rails-env/blob/master/recipes/postgresql/vars/main.yml). Feel free to change if needed.
6. Then run `vagrant up`

### FAQ

1. **Question**: What if my machine does not have enough RAM (4GB or less)?

    **Answer**: You can customize the amount of memory in the `Vagrant` file under the `vm.provider` block:

    ```ruby
    config.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      # ... other configs
    end
    ```


2. **Question**: Vagrant is failing with the message "Failed to mount folders in Linux guest."?

    **Answer**:  Run `vagrant plugin install vagrant-vbguest`. Then run `vagrant reload --provisioning` for
resuming the provisioning process.

3. **Question**: You are facing problems with permissions to mount your local filesystem on the VM environment, something like:

    ```shell
    mount -o 'vers=3,udp' 10.0.0.1:'/home/myuser/mysrc' /Workspace
    ...
    Stderr from the command:

    stdin: is not a tty
    mount.nfs: access denied by server while mounting 10.0.0.1:'/home/myuser/mysrc'
    ```

    or:
    ```
    exportfs: {folder} does not support NFS export
    ```

    **Answer**: Check if you are trying to mount a directory in an encrypted partition or folder. NFS can't mount this kind of directory. In this case, find a folder (maybe in another partition), give your user proper permissions and move your refer source code there.

      Then you change your Vagrantfile line:

      ```ruby
       config.vm.synced_folder "/home/workspace/my-codes/my-project-src", "/Workspace", nfs: true
      ```

      To:
      ```ruby
      config.vm.synced_folder "/home/another-dir-i-can-access/my-codes/my-project-src", "/Workspace", nfs: true
      ```
