# Vargant with Bash Ansible Chef Puppet

#### Step by step for manual setup
  1. Create index.html (run at host machine)
  ```bash
  echo "<h1>Hello Word - Manual</h1>" > index.html
  ```

  2. Start server
  ```bash
  vagrant up bash
  ```

  3. Run command in guess machine
  ```bash
  sudo apt-get update
  sudo apt-get install -y apache2
  sudo rm -rf /var/www
  sudo ln -fs /vagrant /var/www
  ```

  4. Test web server. Open web browser
  ```
  http://localhost:8081
  ```

#### Step by step for server with provisioner is bash
  1. Create index.html (run at host machine)
  ```bash
  echo "<h1>Hello Word - Bash</h1>" > index.html
  ```

  2. Add new file provisioners/bash/install_apache2.sh
  ```bash
  !#/usr/bin/env bash

  echo 'Installing Apache2 ...'
  apt-get update > /dev/null 2>&1
  apt-get install -y apache2 > /dev/null 2>&1
  rm -rf /var/www
  ln -fs /vagrant /var/www

  ```

  3. Run command in guess machine
  ```bash
  vagrant up bash
  ```

  4. Test web server. Open web browser
  ```
  http://localhost:8081
  ```

#### Step by step for server with provisioner is ansible
  1. Preparing directories structure
  ```
  mkdir -p provisioners/ansible/{roles,group_vars}
  mkdir -p provisioners/ansible/roles/apache2/tasks
  ```

  2. Add playbook.yml to provisioners/ansible/playbook.yml
  ```yaml
  ---
  - hosts: all
    become: yes
    roles:
      - apache2
  ```

  3. Add task install apache2 (create file provisioners/ansible/roles/apache2/tasks/main.yml)
  ```yaml
  ---
  - name: Installing Apache and setting apache up... Please wait
    apt:
      name: apache2
      update_cache: yes

  - import_tasks: link_app.yml
  ```

  4. Add task sync my app directory to apache2 directory (create file provisioners/ansible/roles/apache2/tasks/link_app.yml)
  ```
  ---
  - name: Remove default apache2 app directory
    file:
      name: /var/www
      state: absent

  - name: Create softlink from /vagrant to apache2 app directory /var/www
    file:
      src: /vagrant
      dest: /var/www
      state: link
  ```

  5. Start guest machine
  ```bash
  vagrant up ansible
  ```

  6. Test web server. Open web browser
  ```
  http://localhost:8081
  ```
