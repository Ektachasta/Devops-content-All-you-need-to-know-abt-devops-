# Ansible Hands-On Guide


This document provides a hands-on guide for working with Ansible, covering playbook creation, role creation, and role integration within playbooks.

## Table of Contents

1.  [Prerequisites](#prerequisites)
2.  [Creating Ansible Playbooks](#creating-ansible-playbooks)
3.  [Creating Ansible Roles](#creating-ansible-roles)
4.  [Using Ansible Roles in Playbooks](#using-ansible-roles-in-playbooks)
5.  [Keep Learning and Spreading!](#keep-learning-and-spreading)

## Prerequisites

Before you begin, ensure you have the following:

1.  **Ansible installed:** Ansible must be installed on your master control node.
2.  **SSH Configuration:** Passwordless SSH access must be configured between the master node and the target hosts.  Refer to the official Ansible installation documentation for detailed instructions.

## Creating Ansible Playbooks

This section covers creating a basic Ansible playbook.

*   **Objective:** Create a playbook with two plays:
    *   Play 1: Execute a command and a script on host1.
    *   Play 2: Execute a script and install Nginx on host2.

**Steps:**

1.  **Create the playbook file:**

    Use a text editor (e.g., `nano`, `vim`) to create a `.yml` file for your playbook.

    ```bash
    sudo nano first_playbook.yml
    ```

2.  **Add the playbook content:**

    Paste the following YAML code into the `first_playbook.yml` file:

    ```yaml
    - hosts: host1
      become: true
      name: Play 1
      tasks:
        - name: Execute command 'Date'
          command: date

        - name: Execute script on server
          script: test_script.sh

    - hosts: host2
      become: true
      name: Play 2
      tasks:
        - name: Execute script on server
          script: test_script.sh

        - name: Install nginx
          apt: name=nginx update_cache=yes state=latest
    ```

3.  **Create the script file:**

    The playbook references a script called `test_script.sh`. Create this file on the master node.

    ```bash
    sudo nano test_script.sh
    ```

4.  **Add script content:**

    Paste the following content into `test_script.sh`:

    ```bash
    #!/bin/sh
    # This is a comment!
    echo Hello World  # This is a comment, too!
    ```

5.  **Check playbook syntax:**

    Before running the playbook, verify its syntax to catch any errors.

    ```bash
    ansible-playbook first_playbook.yml --syntax-check
    ```

    A successful syntax check will display "playbook: first\_playbook.yml".

6.  **Run the playbook:**

    Execute the playbook with the following command:

    ```bash
    sudo ansible-playbook first_playbook.yml
    ```

    **Example Output:**

    ```
    PLAY [Play 1] **********

    TASK [Gathering Facts] ************
    ok: [host1]

    TASK [Execute command 'Date'] ******************
    changed: [host1]

    TASK [Execute script on server] ********************
    changed: [host1]

    PLAY [Play 2] ************************************

    TASK [Gathering Facts] **************************************************
    ok: [host2]

    TASK [Execute script on server] ********************
    changed: [host2]

    TASK [Install nginx] ****************************
    changed: [host2]
    ```

    Congratulations! You've run your first Ansible playbook.  Note that running the playbook again without changes might not show any changes because Ansible ensures idempotency.

## Creating Ansible Roles

This section guides you through creating and structuring Ansible roles.

1.  **Create the role:**

    Use the `ansible-galaxy` command to create a new role directory.

    ```bash
    sudo ansible-galaxy init apache
    ```

    This command creates a directory structure like this:

    ```
    apache
    ├── defaults
    │   └── main.yml
    ├── files
    ├── handlers
    │   └── main.yml
    ├── meta
    │   └── main.yml
    ├── tasks
    │   └── main.yml
    ├── templates
    ├── tests
    │   ├── inventory
    │   └── test.yml
    └── vars
        └── main.yml
    ```

2.  **Explore role structure:**

    To visualize the directory structure, install the `tree` utility (if not already installed) and use it to list the role's contents.

    ```bash
    sudo apt install tree
    tree apache
    ```

3.  **Define installation tasks:**

    Edit the `tasks/main.yml` file within the role.  Instead of directly adding tasks to `main.yml`, include other task files for better organization.

    ```bash
    sudo nano /etc/ansible/roles/apache/tasks/main.yml
    ```

    Add the following lines:

    ```yaml
    ---
    # tasks file for apache
    - include: install.yml
    - include: configure.yml
    - include: service.yml
    ```

4.  **Create `install.yml`:**

    Create `install.yml` under `tasks/` folder.

    ```bash
    sudo nano /etc/ansible/roles/apache/tasks/install.yml
    ```

    Add the following tasks:

    ```yaml
    ---
    - name: install apache2
      apt: name=apache2 update_cache=yes state=latest
      become: true
    ```

5.  **Create `configure.yml`:**

    Create `configure.yml` under `tasks/` folder.

    ```bash
    sudo nano /etc/ansible/roles/apache/tasks/configure.yml
    ```

    Add the following tasks:

    ```yaml
    ---
    # configure apache2.conf and send copy.html file
    - name: apache2.conf file
      copy: src=apache2.conf dest=/etc/apache2/
      become: true
      notify:
        - restart apache2 service

    - name: send copy.html file
      copy: src=copy.html dest=/home/ubuntu/
      become: true
    ```

6.  **Create `service.yml`:**

    Create `service.yml` under `tasks/` folder.

    ```bash
    sudo nano /etc/ansible/roles/apache/tasks/service.yml
    ```

    Add the following tasks:

    ```yaml
    ---
    - name: starting apache2 service
      service: name=apache2 state=started
      become: true
    ```

7.  **Store files:**

    Place files that need to be copied to remote hosts into the `files/` directory of the role.

    *   Copy `apache2.conf` from the system directory.

        ```bash
        cp /etc/apache2/apache2.conf /etc/ansible/roles/apache/files/
        ```

    *   Create a dummy HTML file.

        ```bash
        sudo nano /etc/ansible/roles/apache/files/copy.html
        ```

        Add the content:

        ```html
        <html>
        <title> Some File </title>
        <body> <h1> Copy This File </h1> </body>
        </html>
        ```

    *   Verify the files.

        ```bash
        ls /etc/ansible/roles/apache/files/
        ```

8.  **Create handler:**

    A handler is a task that is only executed when notified by another task.  Edit the `handlers/main.yml` file:

    ```bash
    sudo nano /etc/ansible/roles/apache/handlers/main.yml
    ```

    Add the following handler:

    ```yaml
    ---
    # handlers file for apache
    - name: restart apache2 service
      service: name=apache2 state=restarted
    ```

    Make sure the `notify` name in `configure.yml` matches the `name` in `handlers/main.yml`.

9.  **Add role meta data (optional):**

    Edit the `meta/main.yml` file to provide information about the role.

    ```bash
    sudo nano /etc/ansible/roles/apache/meta/main.yml
    ```

    ```yaml
    galaxy_info:
      author: Intellipaat
      description: Simple apache role
      company: Intellipaat

    # If the issue tracker for your role is not on github, uncomment the
    # next line and provide a value
    # issue tracker url: http://example.com/issue/tracker
    ```

10. **Create a site playbook:**

    Create a `site.yml` file to use the role.

    ```bash
    cd /etc/ansible/
    sudo nano site.yml
    ```

    Add the following:

    ```yaml
    ---
    - hosts: host1
      become: true
      roles:
        - apache
    ```

11. **Run the role:**

    Before running the playbook, verify its syntax.

    ```bash
    ansible-playbook site.yml --syntax-check
    ```

12. **Execute the site playbook:**

    ```bash
    sudo ansible-playbook site.yml
    ```

    **Example Output:**

    ```
    PLAY [servers] **************************************************************

    TASK [Gathering Facts] ****************************************************
    ok: [host1]

    TASK [apache : install apache2] ********************************************
    ok: [host1]

    TASK [apache : apache2.conf file] *****************************************
    ok: [host1]

    TASK [apache : send copy.html file] ****************************************
    ok: [host1]

    TASK [apache : starting apache2 service] ***********************************
    ok: [host1]

    PLAY RECAP ********************************************************************
    host1                  : ok=5    changed=0    unreachable=0    failed=0
    ```

    Congratulations! You've successfully created and executed an Ansible role.

## Using Ansible Roles in Playbooks

This section demonstrates how to use roles along with other tasks in a playbook.

1.  **Create a playbook:**

    Create a new playbook called `playbookrole.yml`.

    ```bash
    sudo nano /etc/ansible/playbookrole.yml
    ```

2.  **Add content:**

    ```yaml
    ---
    - hosts: host1
      become: true
      tasks:
        - debug:
            msg: "before we run our role"

        - import_role:
            name: apache

        - include_role:
            name: apache

        - debug:
            msg: "after we ran our role"
    ```

3.  **Check and execute:**

    Check syntax

    ```bash
    ansible-playbook playbookrole.yml --syntax-check
    ```

    Execute the playbook.

    ```bash
    sudo ansible-playbook playbookrole.yml
    ```

    **Example output:**

    ```
    PLAY [servers] **************************************************************

    TASK [Gathering Facts] ****************************************************
    ok: [host1]

    TASK [debug] ****************************************************************
    ok: [host1] => {
        "msg": "before we run our role"
    }

    TASK [apache : install apache2] ********************************************
    ok: [host1]

    TASK [apache : apache2.conf file] *****************************************
    ok: [host1]

    TASK [apache : send copy.html file] ****************************************
    ok: [host1]

    TASK [apache : starting apache2 service] ***********************************
    ok: [host1]

    TASK [debug] ****************************************************************
    ok: [host1] => {
        "msg": "after we ran our role"
    }
    ```

Congratulations! You have successfully integrated Ansible roles with Ansible playbooks.

## Keep Learning and Spreading!

Happy automating!

