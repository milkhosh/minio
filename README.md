# minio
Ansible Playbook for MinIO Deployment 🚀

This repository contains an Ansible playbook to automate the deployment and configuration of a MinIO server using Docker.

## Prerequisites

Before you begin, ensure you have the following installed and configured:

    Ansible: Installed on the machine you'll run the playbook from (the control node).

    Docker & Docker Compose: Installed on the target server.

    SSH Access: You need SSH access from your control node to the target server (either password or key-based).


## Project Structure

The project uses a standard Ansible role-based structure to keep things organized.

.
├── ansible.cfg              # Ansible configuration file
├── inventory/
│   └── hosts.yml            # Defines the servers to manage
├── playbooks/
│   └── deploy.yml           # Main playbook to run the deployment
└── roles/
    └── minio/               # Role containing all the MinIO logic
        ├── files/
        │   └── docker-compose.yml # Docker Compose template for MinIO
        ├── tasks/             # All automation tasks for MinIO
        └── vars/
            └── main.yml       # Default variables for the MinIO role


## Configuration

You'll need to configure your inventory and role variables before running the playbook.

### 1. Inventory (inventory/hosts.yml)

This file tells Ansible where to find your server. You must replace the placeholder values with your server's actual details.

Key values to update:

    ansible_host: The IP address of your target server.

    ansible_port: The SSH port of your target server.

    ansible_user: The username to connect with.

    ansible_password: The user's password (if not using SSH keys).

    ansible_become_password: The user's su or sudo password.

    ansible_ssh_private_key_file: (Optional) Uncomment and set the path to your private SSH key for key-based authentication.


### 2. Role Variables (roles/minio/vars/main.yml)

This file contains all the settings for your MinIO instance. You must change the default credentials for security!

Key variables to customize:

    minio_root_user: The root username for the MinIO console.

    minio_root_password: The root password for the MinIO console.

    minio_access_key & minio_secret_key: Credentials used by the MinIO client (mc).

    minio_private_buckets: A list of buckets to create with private access.

    minio_public_buckets: A list of buckets to create with public read-only access.



## How to Run

    Configure: Make sure you've updated inventory/hosts.yml and roles/minio/vars/main.yml with your settings.

    Execute Playbook: Run the following command from the root of the project directory.

Bash

ansible all -i inventory/hosts.yml -m ping

ansible-playbook -i inventory/hosts.yml playbooks/deploy.yml

This command will:

    Connect to your host.

    Deploy MinIO using the docker-compose.yml file.

    Create the buckets you defined.

    Set the correct access policies on the public buckets.

    Create a new user access key.

Once finished, you can access the MinIO console at http://<your_server_ip>:9001.
