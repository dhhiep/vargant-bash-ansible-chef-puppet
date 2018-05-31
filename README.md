# Vargant with Bash Ansible Chef Puppet

#### Step by step for manual setup
  1. Create index.html (run at host machine)
  ```bash
  echo "<h1>Hello Word - Manual</h1>" > index.html
  ```

  2. Start server
  ```bash
  vargant up bash
  ```

  3. Run command in guess machine
  ```bash
  sudo apt-get update
  sudo apt-get install -y apache2
  sudo rm -rf /var/www
  sudo ln -fs /vargant /var/www
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
  ln -fs /vargant /var/www

  ```

  3. Run command in guess machine
  ```bash
  vargant up bash
  ```

  4. Test web server. Open web browser
  ```
  http://localhost:8081
  ```
