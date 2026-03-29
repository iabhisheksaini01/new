![go](https://github.com/user-attachments/assets/302c1f38-bfec-4110-9d46-6b0f1007a125)
# Golang Installation and Version Management Script
## Author Information

| Created by      | Created on         | Version          | Last updated On   | pre Reviewer       | L0 Reviewer     | L1 Reviewer          |    L2 Reviewer    |
|-----------------|--------------------|------------------|-------------------|--------------------|-----------------|----------------------|-------------------|
| Abhishek saini  |  16-07-2025        | V 1.0            |     18-07-2025    |  Prashant          |  -      |     -   |   - |

---


---
## Table of Contents
- [Objective](#objective)
- [Prerequisites](#prerequisites)  
- [Features](#features)  
- [Supported Versions](#supported-versions)  
- [Installation Methods](#installation-methods)  
- [How It Works](#how-it-works)
- [Usage](#usage)
- [Bash Script](#bash-script)
- [conclusion](#conclusion)  

---

## Objective

To provide a simple script for installing and managing multiple Go (Golang) versions on Ubuntu. It supports APT and tarball methods, and allows easy switching between versions using `update-alternatives`.

---

## Prerequisites

- Ubuntu or Debian-based Linux distribution
- `sudo` privileges

---

## Features

- Install Go using APT package manager
- Install specific Go versions via official binary tarballs
- Version selection from supported list
- Easy to extend with more versions

---


## Supported Versions

Currently the script supports:

- Go 1.20.7 +

You can add more versions by updating the script.

---

## Installation Methods

The script supports three types of operations:

| Option | Description |
|--------|-------------|
| 1      | Install Go via system package manager (`apt`) |
| 2      | Install Go via official tarball from go.dev |


---

## How It Works

1. Prompts the user to select an operation (install)
2. Asks for the desired Go version
3. Installs Go using the selected method
4. Displays the installed Go version at the end

---

## Usage

Run the script with Bash:

```bash
bash go_installer.sh
```
---
## Bash Script
install_go.sh
```bash
#!/bin/bash

echo "Select Go Installation Method:"
echo "1) Package Manager (via apt)"
echo "2) Tarball (official binary)"
read -p "Enter method (1 or 2): " method

echo
echo "Select Go version to install:"
echo "1) 1.18"
echo "2) 1.19"
echo "3) 1.20"
echo "4) 1.21"
echo "5) 1.22"
read -p "Enter version choice (1-5): " choice

case $choice in
  1) goversion="1.18.10" ;;
  2) goversion="1.19.13" ;;
  3) goversion="1.20.14" ;;
  4) goversion="1.21.11" ;;
  5) goversion="1.22.3" ;;
  *) echo "Invalid version choice"; exit 1 ;;
esac

if [[ $method == 1 ]]; then
  echo
  echo "Installing Go $goversion using APT..."

  sudo apt update
  sudo apt install -y golang-go

  # Note: apt usually installs default Go version
  installed_path="/usr/bin/go"

elif [[ $method == 2 ]]; then
  echo
  echo "Installing Go $goversion from tarball..."

  cd /tmp
  wget https://go.dev/dl/go$goversion.linux-amd64.tar.gz
  sudo tar -C /usr/local -xzf go$goversion.linux-amd64.tar.gz

  # Move to unique versioned folder
  sudo mv /usr/local/go /usr/local/go$goversion

  installed_path="/usr/local/go$goversion/bin/go"
else
  echo "Invalid installation method selected."
  exit 1
fi

echo
echo "Registering all Go versions with update-alternatives..."

# Search and register all /usr/local/goX.Y/bin/go and /usr/bin/go
for bin in /usr/local/go*/bin/go /usr/bin/go; do
  [[ -x "$bin" ]] || continue
  ver=$($bin version | awk '{print $3}' | sed 's/go//')
  prio=$((100 + ${ver##*.}))
  echo " - Registering $bin with priority $prio"
  sudo update-alternatives --install /usr/local/bin/go go "$bin" "$prio"
done

echo
echo "Do you want to select the default go version now?"
read -p "Enter y/n: " change_default

if [[ $change_default == "y" || $change_default == "Y" ]]; then
  sudo update-alternatives --config go
fi

echo
echo "Installed Go versions available via alternatives:"
sudo update-alternatives --display go
echo
echo "Current default Go version: $(go version)"
```
---

## Conclusion

This script simplifies the process of installing and managing multiple Go (Golang) versions on Ubuntu systems. It provides users with the flexibility to choose installation methods—either through the system package manager or by downloading official tarballs. Additionally, it leverages `update-alternatives` to manage and switch between multiple Go versions efficiently. This approach ensures that developers can easily work with different Go versions depending on project needs without interfering with system-wide configurations.

---

## Contact Information

| **Name**           | **Email address**                         |
|--------------------|--------------------------------------------|
| Abhishek saini    | abhishek.saini.snaatak@mygurukulam.co |

---

## References

| **Link**                                                                 | **Description**                                   |
|--------------------------------------------------------------------------|---------------------------------------------------|
| [Python installation guide– ](https://phoenixnap.com/kb/how-to-install-python-3-ubuntu) | Document format followed from this link.          |





