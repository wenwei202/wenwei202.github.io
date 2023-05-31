---
id: 1162
title: 'Linux Shell'
date: '2019-01-24T19:18:05+00:00'
author: weiwen.web
layout: revision
guid: 'http://www.pittnuts.com/2019/01/188-revision-v1/'
permalink: '/?p=1162'
---

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell"># tensorboard browsing of remote server2 via an intermediate server1
server2$ tensorboard --logdir cifar10/ --port 8888
localhost$ ssh -L 7777:server2:8888 username@server1
localhost browser: http://localhost:7777/


# get random lines of a file
shuf -n 4 filename

# sort by number
sort -V $list

# download with retry
wget --retry-connrefused --waitretry=1 --read-timeout=20 --timeout=15 -t 0  -c http://www.image-net.org/challenges/LSVRC/2012/nonpub/ILSVRC2012_img_train.tar

# check file sizes
du -h --max-depth=1 | sort -h

# search string with 'Net' in folder include
# -r recursively search, -n display line number
grep -nr  'Net' ./include/

# sum numbers in each line
awk '{s+=$1} END {print s}' yourfileORpipeline

# arch 
lscpu # check cpu core count
cat /proc/cpuinfo # print info for each cpu
cat /proc/meminfo # check memory info

# add user
export username=
sudo useradd -m -d /home/$username -s /bin/bash -U $username
sudo passwd $username # initialize password
sudo chage -d 0 $username # force to change password when login

usermod -a -G $groupname $username #add user to group

apt-get remove packagename #remove the binaries, but not the configuration or data files of the package
apt-get purge packagename # = apt-get --purge remove packagename. remove everything regarding the package but not the dependencies installed with it.
apt-get autoremove # removes orphaned packages, i.e. installed packages that used to be installed as an dependency, but aren't any longer.

# printf+awk to align
awk 'NR%2==1 {printf "%-30s %s\n",$5, $7}' yourfile # print the 5-th and 7-th columns in the odd lines, and align left with width of 30

# check the topology of PCI
lspci -t -v

## a script example to launch executable across servers
## First follow here to add ssh keys in all servers:
##    -  http://www.linuxproblem.org/art_9.html
################ Script begins #################
#!/bin/sh
# script to launch a server/client test with ssh
# must be launched from client
# example: runme 10.0.0.1 /home/perftest/rdma_lat -s 10
if [ $# -lt 1 ] ; then
 echo "Usage: runme <server> <test> <test options>"
 exit 3
fi
server=$1
shift # remove 'runme' from the arguments
echo $*
ssh $server $* & # log into the server and launch the executable 
#give server time to start
sleep 2
$* $server # launch executable in local with the remove server ip
status=$?
wait
exit $status
################# script ends ###################

# Switch your current process in the background
# 1. ctrl+z to stop (pause) the program and get back to the shell
bg # to run it in the background
disown -h # so that the process isn't killed when the terminal closes


#### slurm #####
sinfo # check node list
squeue # check jobs in the queue
scancel <job id> # cancel a job in the queue
srun -N 2 --gres=gpu:4 -p gpu python main_gbn.py # run you program in two (-N 2) nodes each of which must have 4 gpus.

```

<audio controls="controls" style="display: none;"></audio>

<audio controls="controls" style="display: none;"></audio>