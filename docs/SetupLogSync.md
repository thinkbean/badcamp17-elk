# SSH & Rsync Setup

## SSH Keys

You will need to create SSH keys on this system if you haven't already.

We'll use these keys to sync the logs from the remote server to this one.

```
ssh-keygen -t rsa -b 4096
```

Next, add the contents of `~/.ssh/id_rsa.pub` to your hosting provider.

Typically there will be an SSH Keys page in the account section of their interface.

## Create a directory to sync your logs to:

We'll use the following directory format for storing logs: ~/projects/<project-name>/access.log.

Tip: You may want to refactor this with environment subdirectories if you are planning to sync logs for more than
1 environment per project.

```
mkdir -p ~/projects/myproject`
```

For the various host examples in this repo, we will use the following directories:

```
mkdir -p ~/projects/acquia-proj
mkdir -p ~/projects/pantheon-proj
mkdir -p ~/platform-proj
```

## Create the rsync script

Next, let's create a file:

`~/sync-logs.sh`

Use the following format for each log you would like to sync. Replace `[user]`, `[host]`, `[logpath]`, and `[localpath]`
accordingly.

```
rsync -avz --no-perms --no-owner --no-group -e "ssh -o StrictHostKeyChecking=no" [user]@[host]:[logpath] [localpath]
```

`logpath` is the absolute path to the access log file on the remote host.

`logpath` is the local path of the log file. i.e. `/home/ubuntu/projects/myproject/access.log`


Note that Pantheon uses port `2222` for connections.

Below are examples of what the connection strings may look like for each host:

```
# Acquia project example
rsync -avz --no-perms --no-owner --no-group -e "ssh -o StrictHostKeyChecking=no" example.prod@srv-1234.devcloud.hosting.acquia.com:/mnt/log/sites/dqi.prod/logs/srv-1234/access.log /home/ubuntu/projects/acquia-proj/access.log

# Pantheon project example
rsync -avz --no-perms --no-owner --no-group -e "ssh -p 2222 -o StrictHostKeyChecking=no" live.5fc982de-ab1c-52ed-81e4-a55528151e32@appserver.live.5fc982de-ab1c-52ed-81e4-a55528151e32.drush.in:logs/nginx-access.log /home/ubuntu/projects/pantheon-proj/access.log

# Platform project example
rsync -avz --no-perms --no-owner --no-group -e "ssh -o StrictHostKeyChecking=no" 1p23e4pyx28q-master-5lpsesi@ssh.us.platform.sh:/var/log/access.log /home/ubuntu/projects/platform-proj/access.log
```

## Set permissions and test

```
chmod +x sync-logs.sh
./sync-logs.sh
```

If all goes well, you will find an access log in the local path.


Troubleshooting:

- Make sure your public key has been added to the remote host.
- SSH (or SFTP in Pantheon's case) to the remote host to make sure the remote path you defined is correct.
- Verify the directory of the local path you defined is correct.

## Setup cron to sync every minute

Use `crontab -e` to open the cron editor.

Add the following:

```
* * * * * /home/ubuntu/sync-logs.sh > /dev/null 2>&1
```
