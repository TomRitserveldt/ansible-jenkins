#jenkins CI platform with ansible on CentOS 7

This ansible roles installs a jenkins platform on CentOS 7/RHEL 7. It provides a basic jenkins configuration, allows you to add new jobs with a git repository, and choose the plugins you need for jenkins on your server.
However it does not have support for creating jenkins users yet. you can add these yourself manually in the jenkins dashboard or fork this project and add it into the ansible code.

# Requirements
It's recommended to run Ansible 1.7.1 and vagrant 1.7.2 or higher to use this role.

# Role Variables
Following variables are used by this role:

##### Host_vars

```
# An example list of jenkins jobs that you need to have created,
# and their values to be added to the template file for the job.xml creation.
# you can replace these values with your own, and add variables to the host_vars file and the template file(located in /roles/ansible-jenkins/templates)

jenkins_jobs:
    # The job name
  - name: "Job_test1"
    # The name of the xml file to be generated on the server(used to create the job)
    xml_name: "Job_test1.xml"
    # The to be generated jenkins job description 
    project_description: "This is a jenkins job."
    # The git repository for the job
    git_repository: "https://github.com/TomRitserveldt/cheat_sheets.git"
    # The following variables are default values, change according to your needs.
    git_branchspec: "*/master"
    can_roam: "true"      
    disabled: "false"
    concurrent_build: "false"
    
  - name: "Job_test2"
    xml_name: "Job_test2.xml"
    project_description: "This is another jenkins job."
    git_repository: "https://github.com/TomRitserveldt/cheat_sheets.git"
    git_branchspec: "*/master"
    can_roam: "true"
    disabled: "false"
    concurrent_build: "false"
```
## Jenkins role vars

```
jenkins_dest: /opt/jenkins  # Jenkins CLI destination folder
jenkins_delay_s: 25  # Delay to wait on the service to start, change as needed.
# (if the delay is too low you'll get a HTTP ERROR 503: Service unavailable.)

plugins: # List of the plugins you want to install for jenkins.
  - github
jenkins:
  cli_dest: '{{ jenkins_dest }}/jenkins-cli.jar' # Jenkins CLI destination
  updates_dest: '{{ jenkins_dest }}/updates_jenkins.json' # Jenkins updates file

```

# How the role works

1. the jenkins dependencies are installed
1. jenkins is downloaded and installed
1. the service is started and firewall rules are added
2. jenkins cli is installed and managed
3. plugins for jenkins are installed or updated, for now only github is installed(you can add plugins you need in the jenkins role var file as shown above)
4. jenkins jobs are created
5. you're done! now you can access the jenkins dashboard interface on "your-ip-address:8080" .