# Useful Bash Aliases and Functions

## Declare a Bash Alias

```bash
alias alias_name="command_to_run"
```

## Remove alias

```bash
unalias alias_name
```

```bash
alias ll="ls -lhA"

unalias ll
```

To temporarily bypass an alias (say we aliased ls to ls -a), we can type:

```bash
\ls
```

## Persisting aliases

nano

```bash
#  ~/.bashrc

# Aliases

alias ll="ls -lhA"
```

## Examples

```bash
# Colorize the ls output
alias ls='ls --color=auto'
 
# Use a long listing format
alias ll='ls -la'
 
# Show hidden files
alias l.='ls -d .* --color=auto'

###

# Colorize the grep command output for ease of use (good for log files)
alias grep='grep --color=auto'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'

###

# create parent directories on demand
alias mkdir='mkdir -pv'

###

alias path='echo -e ${PATH//:/\\n}'
alias now='date +"%T"'
alias nowtime=now
alias nowdate='date +"%d-%m-%Y"'

###

# set vim as default
alias vi=vim
alias svi='sudo vi'
alias vis='vim "+set si"'
alias edit='vim'

###

# Stop after sending count ECHO_REQUEST packets #
alias ping='ping -c 5'
# Do not wait interval 1 second, go fast #
alias fastping='ping -c 100 -s.2'

###

# show open ports
alias ports='netstat -tulanp'

###

# get web server headers #
alias header='curl -I'
 
# find out if remote server supports gzip / mod_deflate or not #
alias headerc='curl -I --compress'

###

# do not delete / or prompt if deleting more than 3 files at a time #
alias rm='rm -I --preserve-root'
 
# confirmation #
alias mv='mv -i'
alias cp='cp -i'
alias ln='ln -i'
 
# Parenting changing perms on / #
alias chown='chown --preserve-root'
alias chmod='chmod --preserve-root'
alias chgrp='chgrp --preserve-root'

###

# distro specific  - Debian / Ubuntu and friends #
# install with apt-get
alias apt-get="sudo apt-get"
alias updatey="sudo apt-get --yes"
 
# update on one command
alias update='sudo apt-get update && sudo apt-get upgrade'

###

# become root #
alias root='sudo -i'
alias su='sudo -i'

###

## pass options to free ##
alias meminfo='free -m -l -t'
 
## get top process eating memory
alias psmem='ps auxf | sort -nr -k 4'
alias psmem10='ps auxf | sort -nr -k 4 | head -10'
 
## get top process eating cpu ##
alias pscpu='ps auxf | sort -nr -k 3'
alias pscpu10='ps auxf | sort -nr -k 3 | head -10'
 
## Get server cpu info ##
alias cpuinfo='lscpu'
 
## older system use /proc/cpuinfo ##
##alias cpuinfo='less /proc/cpuinfo' ##
 
## get GPU ram on desktop / laptop##
alias gpumeminfo='grep -i --color memory /var/log/Xorg.0.log'

###

alias wget='wget -c'

###
```

Do not create ssh alias, instead use `~/.ssh/config` OpenSSH SSH client configuration files. It offers more option. An example:

```txt
Host server10
  Hostname 1.2.3.4
  IdentityFile ~/backups/.ssh/id_dsa
  user foobar
  Port 30000
  ForwardX11Trusted yes
  TCPKeepAlive yes
```

You can now connect to peer1 using the following syntax:

```bash
ssh server10
```
