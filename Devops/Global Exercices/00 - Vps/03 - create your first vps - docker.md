# Exercise : Create your first VPS (Docker)

The goal of this exercise is to provision an **Ubuntu** VPS and install **Docker Engine** following the official documentation.

Official reference : [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

## 1. Create the VPS

Create a new online VPS with **Ubuntu** (a version supported by Docker — check the prerequisites in the documentation).

Connect to it as `root` and make sure the system is up to date before installing anything.

## 2. Read the documentation before installing

Open the [Docker Ubuntu install guide](https://docs.docker.com/engine/install/ubuntu/) and read it entirely before running any command on your VPS.

Answer the following from what you read :

- Which Ubuntu versions are officially supported ?
- Why does Docker recommend uninstalling certain packages **before** installing Docker Engine ? Which packages are listed as conflicting ?
- What are the different **installation methods** proposed by Docker ? Which one is recommended for production, and which one is only recommended for testing / development ?

## 3. Install Docker Engine

Follow the **Install using the `apt` repository** method from the documentation (not the convenience script).

On your VPS, complete the full installation flow :

- Set up Docker's official `apt` repository (GPG key, sources, update the index).
- Install the Docker packages listed in the guide.
- Confirm that the Docker service is running.

Do not copy-paste commands blindly — understand each step you perform.

## 4. Verify the installation

The documentation proposes a way to confirm that Docker works correctly.

- Run the verification step described in the guide.
- What image does it pull ?
- What message do you get when the container exits successfully ?

## 5. Run Docker without `sudo` ?

After installation, try to run a simple Docker command **without** `sudo`.

- Does it work ? If not, why ?
- What does the documentation suggest to allow a non-root user to run Docker commands ? (Read the linked post-installation section mentioned in the guide.)

## Recap

At the end of this exercise you should be able to :

- Install Docker Engine on Ubuntu using the official `apt` repository method.
- Explain why old or unofficial Docker packages must be removed first.
- Name the main installation methods and know which one to avoid in production.
- Verify that Docker is installed and running.
- Explain why `sudo` is required by default and how non-root access can be configured.
