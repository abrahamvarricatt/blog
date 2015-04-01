#######################################
Configuring multiple ssh keys in Ubuntu
#######################################

:date: 2015-04-01 11:12
:modified: 2015-04-01 11:12
:tags: ssh, ubuntu
:category: programming
:author: Abraham_V
:summary: How to configure ssh keys to multiple servers from an Ubuntu client

Before we begin, it is probably a good idea to see if there are any existing ssh keys present on the system. 

.. code-block :: bash

  $ ls -al ~/.ssh

Consider making a back-up of anything found here. We'll just assume that it's empty.

Now we need to create a new SSH key,

.. code-block :: bash

  $ ssh-keygen -t rsa -C "my_email@example.com"
  Generating public/private rsa key pair.
  Enter file in which to save the key (/home/USER/.ssh/id_rsa): /home/USER/.ssh/id_example_rsa
  Enter passphrase (empty for no passphrase): 
  Enter same passphrase again: 
  Your identification has been saved in /home/USER/.ssh/id_example_rsa.
  Your public key has been saved in /home/USER/.ssh/id_example_rsa.pub.
  The key fingerprint is:
  df:5e:4e:3a:cf:3a:a2:e7:98:93:e0:82:5d:38:09:56 my_email@example.com
  The key's randomart image is:
  +--[ RSA 2048]----+
  |                 |
  |    E            |
  |   .             |
  |  o              |
  | . . o  S        |
  |    + o  . .     |
  |   o + . .. . o  |
  |  . o . ooo.o*   |
  |     .  +=..+=+  |
  +-----------------+
  $ 

You need different SSH keys for each server you want to connect to. And it's a good idea to customize the filename where the key is stored (as shown above), to help distinguist between the keys. 

Next is to enable ssh-agent and add the key to it.

.. code-block :: bash

  $ eval "$(ssh-agent -s)"
  Agent pid 8550
  $ ssh-add ~/.ssh/id_example_rsa
  # You may be asked a password, if it was set earlier

Now we need to make a config file to tell ssh which keys we want to use with which servers/usernames,

.. code-block :: bash

  $ cat ~/.ssh/config
  Host myshortname realname.example.com
      Hostname realname.example.com
      IdentityFile ~/.ssh/id_example_rsa # private key for realname
      User remoteusername

And that's it! Now you can use ``myshortname`` when referring to that particular combination of server+username. 


**********
References
**********
 * `Generating SSH keys <https://help.github.com/articles/generating-ssh-keys/>`_
 * `Best way to use multiple SSH private keys on one client <http://stackoverflow.com/questions/2419566/best-way-to-use-multiple-ssh-private-keys-on-one-client>`_
 * `How to use multiple ssh keys with different accounts and hosts <http://askubuntu.com/questions/269140/how-to-use-multiple-ssh-keys-with-different-accounts-and-hosts>`_

