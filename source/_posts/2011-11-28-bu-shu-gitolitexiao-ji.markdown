---
layout: post
title: "部署Gitolite小记"
date: 2011-11-28 19:54
comments: true
categories: [git] [gitolite]
---
####看到更新后的Gitolite失去了gl-easy-install脚本自动化安装，有些失望。由于着急用，就临时用老方法安装了一次，这里记录一下。

<pre>
{% codeblock lang:bash %}
code snippet
# groupadd gitolite
# useradd -g gitolite gitolite
# passwd gitolite
Changing password for user gitolite.
New UNIX password: 
BAD PASSWORD: it is based on a dictionary word
Retype new UNIX password: 
passwd: all authentication tokens updated successfully.

# su - gitolite
$ ssh-keygen 
Generating public/private rsa key pair.
Enter file in which to save the key (/home/gitolite/.ssh/id_rsa): 
Created directory '/home/gitolite/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/gitolite/.ssh/id_rsa.
Your public key has been saved in /home/gitolite/.ssh/id_rsa.pub.
The key fingerprint is:
cd:84:5d:f7:01:b6:a6:d9:6e:08:94:06:30:c2:36:7b gitolite@Labs03

$ scp ~/.ssh/id_rsa.pub gitolite@Labs03:git.pub
The authenticity of host 'labs03 (127.0.0.1)' can't be established.
RSA key fingerprint is e8:bc:de:7a:79:d9:08:c9:a0:67:b3:71:0f:98:24:3a.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'labs03' (RSA) to the list of known hosts.
gitolite@labs03's password: 
id_rsa.pub                                                                                                                                                  100%  397     0.4KB/s   00:00   
 
$ git clone git://github.com/sitaramc/gitolite.git
Cloning into gitolite...
remote: Counting objects: 5580, done.
remote: Compressing objects: 100% (1912/1912), done.
remote: Total 5580 (delta 3943), reused 5210 (delta 3615)
Receiving objects: 100% (5580/5580), 2.05 MiB | 213 KiB/s, done.
Resolving deltas: 100% (3943/3943), done.

$ cd gitolite/
$ git bundle create /tmp/gitolite.bdl --all
Counting objects: 5580, done.
Delta compression using up to 2 threads.
$REPO_UMASK = 0077;
Compressing objects: 100% (1584/1584), done.
Writing objects: 100% (5580/5580), 2.05 MiB, done.
Total 5580 (delta 3943), reused 5580 (delta 3943)

$ git bundle create /tmp/gitolite.bdl --all
Counting objects: 5580, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (1584/1584), done.
Writing objects: 100% (5580/5580), 2.05 MiB, done.
Total 5580 (delta 3943), reused 5580 (delta 3943)

$ git clone /tmp/gitolite.bdl gitolite
Cloning into gitolite...

$ cd gitolite/
$ src/gl-system-install 
using default values for EUID=500:
/home/gitolite/bin /home/gitolite/share/gitolite/conf /home/gitolite/share/gitolite/hooks

$ cd ~
$ bin/gl-setup git.pub 
The default settings in the rc file (/home/gitolite/.gitolite.rc) are fine for most
people but if you wish to make any changes, you can do so now.

hit enter...
creating gitolite-admin...
Initialized empty Git repository in /home/gitolite/repositories/gitolite-admin.git/
creating testing...
Initialized empty Git repository in /home/gitolite/repositories/testing.git/
[master (root-commit) bb78d7c] start
 2 files changed, 6 insertions(+), 0 deletions(-)
 create mode 100644 conf/gitolite.conf
 create mode 100644 keydir/git.pub

$ git clone gitolite@Labs03:gitolite-admin
Cloning into gitolite-admin...
remote: Counting objects: 6, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 6 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (6/6), done.
{% endcodeblock %}
</pre>
