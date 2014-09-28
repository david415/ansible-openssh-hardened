
===========================================================
 Justifiably paranoid Ansible deployment of OpenSSH-server
===========================================================

* Only use ed25519 keys with elliptic-curve Diffie–Hellman for key exchange

* Only use the new transport cipher! `chacha20-poly1305@openssh.com`

* Operators are advised to use bcrypt KDF to protect keys at rest

::

   ---
   - hosts: ssh-servers
     user: human
     connection: ssh
     roles:
       - { role: ansible-openssh-hardened,
           backports_url: "http://ftp.de.debian.org/debian/",
           backports_distribution_release: "wheezy-backports",
           sudo: yes
         }


