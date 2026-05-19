# Exercise : Create your first VPS (update)

The goal of this exercise is to get comfortable with a fresh Ubuntu VPS: system updates, package management with `sudo`, and making a personal script available from anywhere via `PATH`.

## 1. Create the VPS

Create a new online VPS with **Ubuntu** at the provider of your choice. Connect to it as `root`.

## 2. Update the system

Before installing anything, you need to refresh the package lists and upgrade installed packages.

- What does `apt update` do ?
- What does `apt upgrade` do ?
- What is the difference between the two ?
- In which order must you run them, and why ?

Run them in the correct order on your VPS.

## 3. Install a package : with and without `sudo`

Choose a package that is not already installed on your VPS — for example **htop**.

Try to install it with `apt install` **without** `sudo`.

- What happens ?
- Why does it fail ?

Now try again **with** `sudo`.

- What is the role of `sudo` ?
- What is the difference between running a command as a normal user and running it with `sudo` ?

## 4. Create a script in the home directory

Create a file named `my_ls.sh` in the **`~` directory**.

Before you create the file, explain in your own words :

- What is the `~` directory ?
- Which user does it belong to when you are logged in as `root` ?

Put the following content inside `my_ls.sh` :

```
#!/bin/bash
ls -la
```

Make the script executable :

```bash
chmod +x ./my_ls.sh
```

run the script: 
```bash
./my_ls.sh
```

## 5. Run the script from another directory

Change your current directory to `/` (the root of the filesystem).

Try to run `my_ls.sh` from there.

- Does it work ? Why or why not ?
- What is the difference between giving a relative path and giving an absolute path to a script ?

## 6. Make the script available everywhere with `PATH`

Modify the `PATH` environment variable so that you can run `my_ls.sh` (or `my_ls`) from **any** directory without typing the full path to the file.

PATH={PATH_TO_my_ls.sh:$PATH}
Test it from at least two different directories.

## 7. Persist the `PATH` across sessions

Disconnect from the ssh VPS, then reconnect.

Check the current value of `PATH` :

```bash
echo $PATH
```

- Is your custom entry still there ?
- Why was it lost after reconnecting ?

The change you made earlier was only applied to the **current shell session**. To make it permanent, you need to save it in a file that is loaded every time you open a new shell.

Use the **`.bashrc`** file in your home directory (`~/.bashrc`).

- What is the purpose of `.bashrc` ?
- Where in the file should you add your `PATH` modification ?
- After editing `.bashrc`, how can you apply the change without disconnecting ?

Disconnect and reconnect one more time, then run `echo $PATH` again to confirm the change is saved.

## Recap

At the end of this exercise you should be able to :

- Explain the difference between `apt update` and `apt upgrade`, and run them in the correct order.
- Explain why `apt install` requires `sudo` and what `sudo` does.
- Explain what the `~` directory represents.
- Create an executable shell script and understand why it cannot be run by name alone from another directory.
- Extend `PATH` so a script is reachable from anywhere.
- Explain why environment changes are lost on disconnect, and how `.bashrc` makes them permanent.
