# ansible


## Task

The scope includes:

1. setting up a new Ansible instance on a Linux server - it could be a step-by-step guide rather than fully automated.
1. keeping local copy of ACME client for installation and management of "version inventory" on targets;
1. ACME client upgrade management - staged upgrade, where new version would be deployed on a small number of targets first, followed by upgrade on other nodes
1. logging - customise logging so it is formatted as JSON and if possible, push logs to an external server, including a customizable API key
1. playbooks to cover a) installation of the ACME client - with backout mechanism, i.e., quick switch to previous version b) regular checks of certificates - PEM files in pre-defined location(s) and a simple text file with hostnames (several locations can be defined to cater for specific Windows configurations); c) certificate renewals - invoking installed ACME clients and running follow-up certificate processing - e.g., certutil to create PFX files and import them to Windows crypto store
1. playbooks must be resilient against timeouts / stalling on targets, e.g., flagging such targets so they wouldn't impact other targets
1. testing of agents integration using a separate ACME proxy
1. delivery of all the code and scripts into GitLab repo(s)


## Initial Milestones

1. create a copy of the agent in a repo, initial script to install ansible and an "initial/empty" playbook and with a scheduler - basically to automate creating an ansible instance
1. create a playbook to install the agent: checks if agent already installed, and its version, if a new version is in the tower/repo, remove old copy, install a new one - I will provide some of the parameters required to set up agent's working folders
1. create 2nd playbook - renewal of certificates
1. improvements based on my feedback


## Details for testing

Linux server:
root@157.245.60.92 - password: ansible-kc

Win servers:
IP: 18.143.103.50 - default admin password: JQ(qL.Ly(Ai
poison.bastion.zone
b1.bastion.zone
b2.bastion.zone

IP: 54.254.168.182 - Ntp!9!Wwd?
poison2.bastion.zone
b3.bastion.zone
b4.bastion.zone

ports open on Win servers: 22, 80, 3389, 5985, 5986

## Agent to deploy on targets

page with details: https://certbot.eff.org/instructions?ws=other&os=windows

download of the agent: https://dl.eff.org/certbot-beta-installer-win_amd64.exe

basic parameters to run the agent: 
certbot certonly -d poison.bastion.zone --server https://acme.digicert.com/v2/acme/directory/ --standalone --work-dir <?> --logs-dir <?> --config-dir <?> -m support@keychest.net --agree-tos --eff-email -v

Folders: 
installation: default - C:\program files (x86)\certbot
data: c:\keychest
 - subfolders of the data folder:
     --work-dir .\work
     --config-dir .\config
     --logs-dir .\log 




## Getting started

To make it easy for you to get started with GitLab, here's a list of recommended next steps.

Already a pro? Just edit this README.md and make it your own. Want to make it easy? [Use the template at the bottom](#editing-this-readme)!

## Add your files

- [ ] [Create](https://docs.gitlab.com/ee/user/project/repository/web_editor.html#create-a-file) or [upload](https://docs.gitlab.com/ee/user/project/repository/web_editor.html#upload-a-file) files
- [ ] [Add files using the command line](https://docs.gitlab.com/ee/gitlab-basics/add-file.html#add-a-file-using-the-command-line) or push an existing Git repository with the following command:

```
cd existing_repo
git remote add origin https://gitlab.com/keychest/ansible.git
git branch -M main
git push -uf origin main
```

## Integrate with your tools

- [ ] [Set up project integrations](https://gitlab.com/keychest/ansible/-/settings/integrations)

## Collaborate with your team

- [ ] [Invite team members and collaborators](https://docs.gitlab.com/ee/user/project/members/)
- [ ] [Create a new merge request](https://docs.gitlab.com/ee/user/project/merge_requests/creating_merge_requests.html)
- [ ] [Automatically close issues from merge requests](https://docs.gitlab.com/ee/user/project/issues/managing_issues.html#closing-issues-automatically)
- [ ] [Enable merge request approvals](https://docs.gitlab.com/ee/user/project/merge_requests/approvals/)
- [ ] [Automatically merge when pipeline succeeds](https://docs.gitlab.com/ee/user/project/merge_requests/merge_when_pipeline_succeeds.html)

## Test and Deploy

Use the built-in continuous integration in GitLab.

- [ ] [Get started with GitLab CI/CD](https://docs.gitlab.com/ee/ci/quick_start/index.html)
- [ ] [Analyze your code for known vulnerabilities with Static Application Security Testing(SAST)](https://docs.gitlab.com/ee/user/application_security/sast/)
- [ ] [Deploy to Kubernetes, Amazon EC2, or Amazon ECS using Auto Deploy](https://docs.gitlab.com/ee/topics/autodevops/requirements.html)
- [ ] [Use pull-based deployments for improved Kubernetes management](https://docs.gitlab.com/ee/user/clusters/agent/)
- [ ] [Set up protected environments](https://docs.gitlab.com/ee/ci/environments/protected_environments.html)

***

# Editing this README

When you're ready to make this README your own, just edit this file and use the handy template below (or feel free to structure it however you want - this is just a starting point!). Thank you to [makeareadme.com](https://www.makeareadme.com/) for this template.

## Suggestions for a good README
Every project is different, so consider which of these sections apply to yours. The sections used in the template are suggestions for most open source projects. Also keep in mind that while a README can be too long and detailed, too long is better than too short. If you think your README is too long, consider utilizing another form of documentation rather than cutting out information.

## Name
Choose a self-explaining name for your project.

## Description
Let people know what your project can do specifically. Provide context and add a link to any reference visitors might be unfamiliar with. A list of Features or a Background subsection can also be added here. If there are alternatives to your project, this is a good place to list differentiating factors.

## Badges
On some READMEs, you may see small images that convey metadata, such as whether or not all the tests are passing for the project. You can use Shields to add some to your README. Many services also have instructions for adding a badge.

## Visuals
Depending on what you are making, it can be a good idea to include screenshots or even a video (you'll frequently see GIFs rather than actual videos). Tools like ttygif can help, but check out Asciinema for a more sophisticated method.

## Installation
Within a particular ecosystem, there may be a common way of installing things, such as using Yarn, NuGet, or Homebrew. However, consider the possibility that whoever is reading your README is a novice and would like more guidance. Listing specific steps helps remove ambiguity and gets people to using your project as quickly as possible. If it only runs in a specific context like a particular programming language version or operating system or has dependencies that have to be installed manually, also add a Requirements subsection.

## Usage
Use examples liberally, and show the expected output if you can. It's helpful to have inline the smallest example of usage that you can demonstrate, while providing links to more sophisticated examples if they are too long to reasonably include in the README.

## Support
Tell people where they can go to for help. It can be any combination of an issue tracker, a chat room, an email address, etc.

## Roadmap
If you have ideas for releases in the future, it is a good idea to list them in the README.

## Contributing
State if you are open to contributions and what your requirements are for accepting them.

For people who want to make changes to your project, it's helpful to have some documentation on how to get started. Perhaps there is a script that they should run or some environment variables that they need to set. Make these steps explicit. These instructions could also be useful to your future self.

You can also document commands to lint the code or run tests. These steps help to ensure high code quality and reduce the likelihood that the changes inadvertently break something. Having instructions for running tests is especially helpful if it requires external setup, such as starting a Selenium server for testing in a browser.

## Authors and acknowledgment
Show your appreciation to those who have contributed to the project.

## License
For open source projects, say how it is licensed.

## Project status
If you have run out of energy or time for your project, put a note at the top of the README saying that development has slowed down or stopped completely. Someone may choose to fork your project or volunteer to step in as a maintainer or owner, allowing your project to keep going. You can also make an explicit request for maintainers.
