# Exercise : Create your first VPS (Alpine)

The goal of this exercise is to provision a VPS running **Alpine Linux** and discover how system updates work on a distribution that does not use `apt`.

If you already completed the Ubuntu update exercise, keep that in mind — Alpine works differently.

## 1. Create the VPS

Create a new online VPS with **Alpine Linux** at the provider of your choice.

- Connect to it as `root`.
- Write down the public **IPv4** address.

## 2. Update and upgrade the system

On your VPS, try to refresh the package lists and upgrade the installed packages.

- Which commands do you use on Alpine ? (If you try the same commands as on Ubuntu, what happens ?)
- Is there a separate step to **update** the index and another to **upgrade** installed packages, or is it handled differently ?
- In which order should you run them, and why ?

Run the correct commands on your VPS until the system is up to date.

Answer in your own words :

- What does the **update** step do on Alpine ?
- What does the **upgrade** step do ?
- How is this similar or different from what you did on Ubuntu with `apt` ?

## 3. What is `apk` ?

While working on the previous section, you used (or should have used) the **`apk`** tool.

Research and explain :

- What does `apk` stand for ?
- What is its role on Alpine Linux ?
- What is the equivalent tool on Ubuntu / Debian ?
- Why does Alpine use `apk` instead of `apt` ?

## Recap

At the end of this exercise you should be able to :

- Create and access an Alpine Linux VPS.
- Update and upgrade an Alpine system using the correct `apk` commands and order.
- Explain what `apk` is and how it relates to package management on other distributions.
