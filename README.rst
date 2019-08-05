
===========================================================
 Justifiably paranoid Ansible deployment of OpenSSH-server
===========================================================

* Only use ed25519 keys with elliptic-curve Diffie–Hellman for key exchange

* Only use the new transport cipher! `chacha20-poly1305@openssh.com`
Here in this playbook I don't fuck around with any of the old ciphers for ssh.
This requires OpenSSH 6.5 and later for both the client AND server.
Don't carelessly mess around with this ansible role, lock yourself out of your servers
and then send me pissed off e-mails about how it's all my fault. =-p

* Operators are advised to use bcrypt KDF to protect keys at rest...
  Generate your new ed25519 ssh keys with bcrypt stretching.
  Here's uber-paranoid 1000 rounds of bcrypt for your entropic passphrase::
    ssh-keygen -t ed25519 -o -a 1000 -f ~/.ssh/id_ed25519_new2u


Here's a minimal playbook demonstrating how I upgrade a Debian wheezy system to
use wheezy-backports openssh-server and client

::

    ---
    - hosts: ssh-servers
      user: human
      connection: ssh
      roles:
        - { role: ansible-openssh-hardened,
            backports_url: "http://ftp.de.debian.org/debian/",
            backports_distribution_release: "wheezy-backports",
            ssh_admin_ed25519pubkey_path: "/home/amnesia/.ssh/id_ed25519.pub",
            sudo: yes
          }
