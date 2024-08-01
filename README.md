# Arch Linux Ansible

### VM Setup in Virt-Manager

Get an ISO. Before installation, change the firmware to UEFI.
This affects how you install the [GRUB bootloader](https://wiki.archlinux.org/title/GRUB).
Everything else can pretty much be default.

```bash
curl -OL https://mirrors.mit.edu/archlinux/iso/2024.07.01/archlinux-2024.07.01-x86_64.iso
```

Get your IP, create a password for root, and SSH into the root user using the password and IP address you find.

```bash
ip a | grep inet # 192.168.122.36
echo super-secret-password | passwd --stdin root
ssh root@192.168.122.36
touch ~/.ssh/authorized_keys # put your public SSH key here
```

Now you can SSH into the installation environment without a password.

### Ansible Setup

```bash
python3 --version # Python 3.12.4
python3 -m venv .venv
source ./.venv/bin/activate
pip install --upgrade ansible=9.8.0 # for mitogen compatibility
pip install --upgrade mitogen
```

Change the IP address in `./inventory.ini`. Verify Ansible works with `ansible all -m ping`.

```ini
# inventory.ini
[vms]
192.168.122.179
```

### Ansible Vault

Create a vault password in `./.vault_password`.
Create encrypted variables as shown below with `ansible_vault`.

```bash
ansible-vault encrypt_string $PASSWORD_FOR_ROOT_LOGIN --name root_password
ansible-vault encrypt_string $PASSWORD_FOR_USER_LOGIN --name user_password
```

Create the file `./group_vars/all.yml` to store variables.

```yml
# ./group_vars/all.yml
root_password: !vault |
  $ANSIBLE_VAULT;1.1;AES256
  35643232373439646462326533393738346138616239613937353831623530396432343434633433
  3237366232646136313466613362643338393730653437650a623464643632383865633366376332
  39623938366135313664333862393530643539326264356638343330366132656638363834613161
  3634646533323932610a626130653730613032363664316532333230663838313033316238303539
  6161
user_password: !vault |
  $ANSIBLE_VAULT;1.1;AES256
  31663131613164396438613032626239633466356566313033396462656330393161353634656462
  3539633231666664663137333034373930316332633334300a653035623537363763313861396264
  65333066623139363763333433383234333336643561633836616665313537663237646537313335
  3637636639386630320a383466366536613836373836383331633633646662613839343662343834
  3135
```

### Run It

Finally, run the playbook.

```bash
ansible-playbook.yml
```
