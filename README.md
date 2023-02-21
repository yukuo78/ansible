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
`certbot certonly -d poison.bastion.zone --server https://acme.digicert.com/v2/acme/directory/ --standalone --work-dir <?> --logs-dir <?> --config-dir <?> -m support@keychest.net --agree-tos --eff-email -v`

Folders: 
installation: default - C:\program files (x86)\certbot
data: c:\keychest
 - subfolders of the data folder:
     --work-dir .\work
     --config-dir .\config
     --logs-dir .\log 


the data folder also contains a simple text file "hostname", which contains a hostname - one per line, these will be added to parameters with "-d"

e.g. - the file content:
`b2.bastion.zone`
`b1.bastion.zone`
`hostname testing.zone`

if the line "hostname" is present, the playbook will also rung the "hostname" command and adds it to the names for the agent: let's say it returns "win-unit1", the parameters for the agent will be:

`certbot certonly -d b2.bastion.zone -d b1.bastion.zone -d win-unit1.testing.zone --server https://acme.digicert.com/v2/acme/directory/ --standalone --work-dir <?> --logs-dir <?> --config-dir <?> -m support@keychest.net --agree-tos --eff-email -v`

Note: if the hostname command doesn't return FQDN (like above) and the line with 'hostname' has the second string there - "testing.zone", the playbook should add the "testing.zone" to the name to form an FQDN.

Note2: the default contents of the "hostname" file is:
`hostname <zone>`


where the <zone> is pulled from an Ansible configuration variable.



