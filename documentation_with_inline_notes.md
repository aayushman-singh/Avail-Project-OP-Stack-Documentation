
<!-- Mismatch in commands and docs -->

> **Note:**  
> Go version changed from `1.20` to `1.21.6`  
> Node version updated from `16` to `20`  

---

# Install Git
```bash
sudo apt install -y git curl make jq
```
> **Note:**  
> `wget` is also needed for this setup.

---

# Install Go

> **Note:**  
> Updated instructions for downloading and installing Go version `1.21.6`.

```bash
# Download and install Go 1.21.6
wget https://go.dev/dl/go1.21.6.linux-amd64.tar.gz

# Extract the downloaded tarball
tar xvzf go1.21.6.linux-amd64.tar.gz

# Move Go to the correct directory
sudo mv go /usr/local

# Set the GOROOT and update the PATH in .bashrc
echo 'export GOROOT=/usr/local/go' >> ~/.bashrc
echo 'export GOPATH=$HOME/go' >> ~/.bashrc
echo 'export PATH=$GOROOT/bin:$GOPATH/bin:$PATH' >> ~/.bashrc

# Reload .bashrc to apply changes
source ~/.bashrc

# Verify Go installation
go version
```

---

# Install Node.js
> **Note:**  
> Node version updated to `20`.

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs npm
```

---

# Install Pnpm
```bash
sudo npm install -g pnpm
```

---

# Install Make
```bash
sudo apt install -y make
```

---

# Install jq
```bash
sudo apt install -y jq
```

---

# Install direnv
```bash
sudo apt install -y direnv
```

---

