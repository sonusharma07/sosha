---
title: Fabric, the handy tool for automation
description: >-
 I have come across many scenarios where I had to run a command on remote terminal. Yes we can use ssh 
 but we can't use it for automation can we ?
date: "2018-07-27T15:22:29.625Z"
categories: ["DevOps", "Automation"]
keywords: ["DevOps","Automation", "python"]
published: true
---
# Fabric, the handy tool for automation
During automation many time we come across use cases such as 

  * SSH into a remote terminal and execute certain commands.
  * COPY files from local terminal to remote terminal.
  * Editing the files on the remote terminal.

__What is Fabric :grey_question:__

Fabric is a high level Python (2.7, 3.4+) library designed to execute shell commands remotely over SSH, yielding useful Python objects in return.

It is build on **[Invoke](https://www.pyinvoke.org/ "Invoke is a Python (2.7 and 3.4+) task execution tool & library.")** (Python library to execute tasks on console) And **[Paramiko](https://www.paramiko.org/ "Paramiko is a Python (2.7, 3.4+) implementation of the SSHv2 protocol")** (Implementation of the SSHv2 protocol).

****

lets install Fabric using __pip__
`pip install fabric`

__SSH into a remote terminal and execute certain commands.__

```
from fabric import Connection
host = 'servername.domain.net'
con = Connection(host,'root',22,connect_kwargs={'password': 'serverpassword'})
cmd = 'hostname'
result = con.run(cmd , warn = True)
# result = con.run(cmd , warn = True, hide = Ture) can be used suppress local console verbose
print(result.return_code) 
print(result)
```


result.return_code , can be 0, 1, 2 which are standard console return codes.

__COPY files from local terminal to remote terminal.__

```
from fabric import Connection, transfer
host = 'servername.domain.net'
con = Connection(host,'root',22,connect_kwargs={'password': 'serverpassword'})
transfer.Transfer(con)
localfile  = '/root/filename.txt'
remotepath = '/root'
con.put(localfile ,remote)
```
__Editing the files on the remote terminal.__

lets say we have to edit a file which is at remote server.

content of remotefile.txt is 
`
this is first line.
this is second line.
`

but we need both the lines on the same line , like
<pre>
this is first line. this is second line.
</pre>

Therefore we need to connenct to remote machine and edit this file.

```
from fabric import Connection
host = 'servername.domain.net'
con = Connection(host,'root',22,connect_kwargs={'password': 'serverpassword'})
file = con.run('cat /root/remotefile.txt', warn=True)
remotefile = file.stdout.strip().split('\n')
editedremotefile = ' '.join(remotefile)
con.run('echo "{}" > /root/remotefile.txt'.format(editedremotefile), warn=True)
```
Many othre interesting things can be done using Fabric library,  this is just the random use cases shown in this blog.

__Reference :__

[Fabric documentation](http://www.fabfile.org/)



