# CI/CD Deployment using Ansible CM Tool

### Server Setup:

We will need 3 EC2 instances. 1 Ansible Master and 2 Ansible Slave nodes. Jenkins will be installed on Ansible Master Node.

1. Update Packages on All 3 Nodes.
   ` sudo apt-get update
    sudo apt-get upgrade `

2. Install Ansible on Ansible Master.
    `sudo apt-get install ansible`

3. Copy ssh-keygen from Ansible Master to Ansible slaves so passwordless ssh can be enabled.

### Setup Jenkins on Ansible Master

1. Install OpenJDK-17-jdk

  `sudo apt-get install openjdk-17-jdk`

2. Install Jenkins

  `sudo apt-get install jenkins`

3. Install Maven

  `sudo apt-get maven`

### Create Job in Jenkins UI

1. Install ansible plugin

2. Setup ansible, java and maven in jenkins.

3. Ansible-Playbook for execution.

 ` Ansible Playbook

  - name: "Execute war file on Tomcat Server"
    hosts: WEB
    become: true
    vars:
      gitdir: "/tmp/addressbook-project-1"
    tasks:
      - name: "install maven"
        apt:
          update-cache: "yes"
          name: "maven"
          state: "present"


      - name: "Install tomcat9"
        apt:
          update-cache: "yes"
          name: "tomcat9"
          state: "present"
          
      - name: "clone the repository"
        git:
          repo: "https://github.com/upshiftnow/addressbook"
          dest: "{{gitdir}}"
          version: "master"

      - name: "compile the addressbook project"
        command:
          chdir: "{{gitdir}}"
          cmd: "mvn package"

      - name: "Deploy WAR File"
        copy:
          src: "{{gitdir}}/target/addressbook-2.0.war"
          dest: "/var/lib/tomcat9/webapps/addressbook.war"
          remote_src: "yes"
   
      - name: "Stop tomcat9"
        service:
          name: "tomcat9"
          state: "stopped"


      - name: "Start tomcat9"
        service:
          name: "tomcat9"
          state: "started"
          enabled: "yes"
`

4. Inventory File

  `[WEB]
  172.31.12.189
  `






