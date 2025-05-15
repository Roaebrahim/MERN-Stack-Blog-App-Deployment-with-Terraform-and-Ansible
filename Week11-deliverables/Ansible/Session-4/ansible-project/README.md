# Project-203 : Phonebook Application (Python Flask) Deployed with Ansible on Ubuntu 22.04 with MySQL

## Description

The Phonebook Application aims to create a phonebook application in Python and deploy it as a web application with Flask on Ubuntu 22.04 servers using Ansible. The application will be hosted on one server, and the MySQL database will reside on a separate server.

## Problem Statement

- Your company has recently started a project that aims to serve as a phonebook web application. Your teammates have developed the UI part of the project as shown in the template folder, and they need your help to deploy the app in the development environment.

- As a first step, you need to write a program that creates a phonebook, adds requested contacts to the phonebook, finds, and removes contacts from the phonebook.

- Application should allow users to search, add, update, and delete the phonebook records, and the phonebook records should be kept in a separate MySQL database on a remote Ubuntu 22.04 server.

- The Flask application and the MySQL database server should be configured using Ansible playbooks.

## Project Topology

```
[ Ansible Control Machine ]
            |
            |--- [ Ubuntu 22.04 App Server ] -> Runs phonebook-app.py
            |
            |--- [ Ubuntu 22.04 DB Server ] -> MySQL Database
```

## Application Requirements

- id: Unique identifier for the phone record, type is numeric.
- person: Full name of person for the phone record, type is string.
- number: Phone number of the person, type is numeric.

**User Input Validation:**
- Name must be a string. If the user inputs a number in the name field, the app should return a warning.
- Phone number must be numeric. If the user inputs a string in the phone number field, the app should return a warning.

| Input in name field | Format to Convert |
|---------------------|-------------------|
| ""                  | Warning -> 'Invalid input: Name cannot be empty' |
| callahan            | Callahan |
| joHn doE            | John Doe |
| 62267               | Warning -> 'Invalid input: Name of person should be text' |

| Input in number field | Format to Convert |
|-----------------------|-------------------|
| ""                    | Warning -> 'Invalid input: Phone number cannot be empty' |
| 1234567890             | 1234567890 |
| 546347                 | 546347 |
| thousand               | Warning -> 'Invalid input: Phone number should be numeric' |

## Project Structure

```bash
002-ansible-phonebook-web-application (folder)
|
|----readme.md               # Project definition
|----ansible
|      |---- playbook-app.yml    # Ansible Playbook for App setup (To be delivered by students)
|      |---- playbook-mysql.yml  # Ansible Playbook for DB setup (To be delivered by students)
|      |---- inventory           # Inventory file with App and DB server information
|
|----phonebook-app.py        # Python Flask Web Application
|----requirements.txt        # Python requirements libraries
|----init.sql                # sql script for creating db table
|
|----templates
|      |----index.html       # HTML template for search
|      |----add-update.html  # HTML template for adding/updating
|      |----delete.html      # HTML template for deleting
```

## Ansible Playbook Tasks

1. **App Server Setup:**
   - Install Python, Flask, and required packages.
   - Copy the `phonebook-app.py`, `init.sql`, `requirements.txt` and `templates` to the server.
   - Configure the Flask application to connect to the remote MySQL database.
   - Start the Flask application.

2. **Database Server Setup:**
   - Install MySQL.
   - Create a database and a table for the phonebook application.
   - Create a MySQL user with appropriate permissions.
   - Ensure MySQL is accessible from the App Server.

## Expected Outcome

- The web application should be accessible via web browser from the App Server's IP address.
- Users should be able to search, add, update, and delete phonebook records.
- The phonebook records should be persisted in the MySQL database.

## Steps to Solution

1. **Create the project folder and copy application files**

2. **Create an Ansible Inventory File**
   ```ini
   [app]
   app_server ansible_host=<APP_SERVER_IP> ansible_user=ubuntu

   [db]
   db_server ansible_host=<DB_SERVER_IP> ansible_user=ubuntu
   ```

3. **Write Ansible Playbooks:**
   - Install required packages on both servers.
   - Configure MySQL and Flask application.

4. **Run the Ansible Playbook:**
   ```bash
   ansible-playbook ansible/playbook-app.yml
   ansible-playbook inventory ansible/playbook-mysql.yml
   ```

5. **Access the Application:**
   - Open a web browser and go to `http://<APP_SERVER_IP>:80`

6. **Destroy Resources:**
   - Terminate cloud resources (if using) to avoid unnecessary costs.

## Notes

- Customize the application by hard-coding your name for the `developer_name` variable within HTML templates.

## Resources

- [Python Flask Framework](https://flask.palletsprojects.com/en/2.0.x/)
- [Ansible Documentation](https://docs.ansible.com/ansible/latest/user_guide/index.html)
- [MySQL Documentation](https://dev.mysql.com/doc/)

---


