### Learning Ansible

See [the docs](https://docs.ansible.com/ansible/latest/getting_started/get_started_ansible.html).

### Getting Arch Chroot to Work

Getting arch chroot to works
Basically, remote chroot is [not possible](https://github.com/ansible/ansible/issues/6440) with ansible.
There is the [chroot plugin](https://www.reddit.com/r/ansible/comments/8kc59a/how_to_use_the_chroot_connection_plugin/),
but that only allows you to chroot on the server you're running ansible from.
See [here](https://blog.bofh.it/debian/id_462) for how a debian maintainer does it.
They use `mitogen_linear` which makes ansible faster.

### Todo

- Should probably randomize the mirrorlist so that I don't get rate limited. For now, I turned on a VPN.
