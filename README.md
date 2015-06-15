# Gitblit-Federation-Configuration
You can configure federation on Gitblit instance to keep then sync.One Gitblit instance keep in sync with other instance.

## Table of Contents

* [Requirements](#requirements)
* [About Gitblit Federation](#about-gitblit-federation)
* [Origin Gitblit Instance Requirements](#origin-gitblit-instance-requirements)
* [Configuring federation.passphrase](#configuring-federation-passphrase)
* [Pulling Gitblit Instance Requirement](#pulling-gitblit-instance-requirement)
* [Controlling What Gets Pulled From Origin Instance](#controlling-what-gets-pulled-from-origin-instance)
* [Controlling What Gets Pulled At Pulling Instance](#controlling-what-gets-pulled-at-pulling-instance)
* [Federation Pull Registration Kyes](#federation-pull-registration-kyes)
* [Last Step To Sync Repository](#last-step-to-sync-repository)
* [Copy Repository Using Cron Job](#copy-repository-using-cron-job)
* [GitBlit Groovy Push Script](#gitBlit-groovy-push-script)

## Requirements
•	Two standalone Gitblit server

## About Gitblit Federation
A Gitblit federation is a mechanism to clone repositories and keep them in sync from one Gitblit instance to another. If your Gitblit instance allows federation and it is properly registered with another Gitblit instance, each of the non-excluded repositories of your Gitblit instance can be mirrored, in their entirety, to the pulling Gitblit instance. You may optionally allow pulling of user accounts and backup of server settings.


## Origin Gitblit Instance Requirements
•	`git.enableGitServlet` must be `true`, all Git clone and pull requests are handled through Gitblit's JGit servlet

•	`federation.passphrase` must be non-empty

•	The Gitblit origin instance must be `http/https` accessible by the pulling Gitblit instance. That may require configuring port-forwarding on your router and/or opening ports on your firewall.


## Configuring federation passphrase
The passphrase is used to generate permission tokens that can be shared with other Gitblit instances.

The passphrase value never needs to be shared, although if you give another Gitblit instance the ALL federation token then your passphrase will be transferred/backed-up along with all other server settings.

This value can be anything you want: an integer, a sentence, an haiku, etc. You should probably keep the passphrase simple and use standard Latin characters to prevent Java properties file encoding errors. The tokens generated from this value are affected by case, so consider this value CASE-SENSITIVE.
The federation feature is completely disabled if your passphrase value is empty.

**NOTE:**
Changing your `federation.passphrase` will break any registrations you have established with other Gitblit instances.

## Pulling Gitblit Instance Requirement
•	consider setting `federation.allowProposals=true` to facilitate the registration process from origin Gitblit instances

## Controlling What Gets Pulled From Origin Instance
•	After restarting your origin Gitblit instance, access federation page from user dropdown

•	You can find different federation tokens on federation page, Note down any token based on what you want to allow to pull from this Instance

•	Note down your http gitblit instance URL which you can access from your pulling gitblit instance, eg: http://111.111.111.111/gitblit


## Controlling What Gets Pulled At Pulling Instance
•	On your pulling Gitblit instance you can find `# federation.example1.url`  in `gitblit.properties` , Modify it based on your requirement
Eg:

```python
federation.TestRepository.url = http://170.22.222.222/gitblit/
federation.TestRepository.token = 7620b6101eaa884cb89dce0
federation.TestRepository.frequency = 5 mins
federation.TestRepository.folder = TestRepository
federation.TestRepository.bare = true
federation.TestRepository.mirror = true
federation.TestRepository.exclude = *
federation.TestRepository.include = TestRepository.git TestRepo.git
```

## Federation Pull Registration Kyes
 Find the each registration keys [here]  (http://gitblit.com/federation.html#H21)
 
 
## Last Step To Sync Repository
•	Restart your pulling instance. Wait for frequency time you have configured, you can find repository has been pulled from origin instance based on your configuration.

•	On repository page you can see pulled repository under configured folder


## Copy Repository Using Cron Job

•	Create sell script with command to copy whole folder structure. like below

```python
#!/bin/bash
scp –r /source/folder/path /target/Folder/path
```

•	Add this script in to crone table with configurable time

```python
crontab –e 
(Add below line and save it)
0 18 * * * /root/scripts/scp.sh
 ```

## GitBlit Groovy Push Script

