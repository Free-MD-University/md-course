# Exercise : Create your first VPS

The goal of this exercise is to provision your first online VPS on Ubuntu, connect to it with SSH, and play with several ways to manage SSH keys.

## Goal

- Create an Ubuntu VPS at any cloud provider.
- Connect to it as `root` over SSH.
- Manage multiple SSH keys (add, copy, remove) and understand how the `ssh` client picks a key.

## 1. Create the VPS

1. Pick a provider and create a new VPS with the following:
    - OS : **Ubuntu** (latest LTS).
    - A public **IPv4** address (write it down, you will need it).
    - An **SSH key** registered in the provider dashboard (this will be the *initial* key, the one that gives you the first access to the machine).

> Tip : if you don't have a key yet, generate one on your local machine before creating the VPS

## 2. First connection as root

Once the VPS is up, connect to it with the initial key :

```bash
ssh root@<VPS_IPV4>
```

## 3. Create a new SSH key (RSA 2048)

On your **local** machine (not on the VPS), generate a brand new key in **RSA 2048** :

check the ssh-keygen command. 

## 4. Install the new key on the server (2 ways)

### Way 1 : manually edit `~/.ssh/authorized_keys`


```bash
nano /root/.ssh/authorized_keys
```
check on internet how work this file

### Way 2 : with `ssh-copy-id`

From your **local** machine :


> What `ssh-copy-id` does : it logs in with an existing valid key, then appends the public key you gave it to `~/.ssh/authorized_keys` on the remote host (and fixes the permissions if needed).

Back on the VPS, re-check the file. If you copied the same key twice, you should see it duplicated remove one of them .

## 5. Move the new key out of `~/.ssh`

Still on your **local** machine, copy the new key pair to another directory than .ssh.

## 6. Connect using the key from the new location (`-i`)

By default, `ssh` only looks at the keys inside `~/.ssh/`. To use a key from another path, you need the `-i` option :

```bash
ssh -i {key_path} root@<VPS_IPV4>
```

You should be logged in without a password prompt.

> `-i <identity_file>` : tells `ssh` which **private** key file to use for authentication.

## 7. Remove the old key and check the behavior

Goal : prove that `ssh` will **not** automatically pick the key sitting in `~/keys/`.

1. On your **local** machine, delete (or move away) the original key.

2. Also remove the RSA key you may have copied into `~/.ssh/`.

3. Try to connect **without** `-i` :

    ```bash
    ssh root@<VPS_IPV4>
    ```

    → It should **fail** (Permission denied / no key offered), because `ssh` does not look in `~/keys/` by itself.

4. Now connect again **with** `-i` :

    ```bash
    ssh -i ~/keys/id_rsa_vps root@<VPS_IPV4>
    ```

    → It works again.

> Conclusion : `ssh` only auto-loads keys named `id_rsa`, `id_ed25519`, `id_ecdsa`, ... and only from `~/.ssh/`. Any other location must be given explicitly with `-i` (or configured in `~/.ssh/config`).

## 8. Discover `ssh-add`

Typing `-i ~/keys/id_rsa_vps` every time is annoying. This is what `ssh-agent` + `ssh-add` are made for.

Read the manual and answer these questions :

```bash
man ssh-add
ssh-add --help
```

- What is the role of `ssh-agent` ?
- What does `ssh-add <path-to-key>` do ?
- How do you list the keys currently loaded in the agent ? (`ssh-add -l`)
- How do you remove a key from the agent ? (`ssh-add -d <key>`, or `-D` for all)

## Recap

At the end of this exercise you should be able to :

- Create an Ubuntu VPS with an IPv4 and an SSH key.
- Generate an RSA 2048 key with `ssh-keygen`.
- Install a key on a server in two ways : editing `~/.ssh/authorized_keys` by hand, and with `ssh-copy-id`.
- Use a key located outside `~/.ssh/` thanks to the `-i` option.
- Explain why `ssh` does not pick a custom-located key automatically.
- Use `ssh-agent` / `ssh-add` to avoid passing `-i` every time.
