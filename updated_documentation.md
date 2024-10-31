
### Mismatch in Commands and Documentation

> **Comment:**  
> - Go version update: `1.20` → `1.21.6`  
> - Node version update: `16` → `20`

---

# Install Git
```bash
sudo apt install -y git curl make jq
```
> **Note:**  
> `wget` is also needed. 

---

# Install Go
> **Comment:**  
> Recommended update for Go installation instructions to Go version `1.21.6`.  

- **Download and install Go 1.21.6**  
    ```bash
    wget https://go.dev/dl/go1.21.6.linux-amd64.tar.gz
    ```
- **Extract the downloaded tarball**  
    ```bash
    tar xvzf go1.21.6.linux-amd64.tar.gz
    ```
- **Move Go to the correct directory**  
    ```bash
    sudo mv go /usr/local
    ```
- **Set the GOROOT and update the PATH in .bashrc**  
    ```bash
    echo 'export GOROOT=/usr/local/go' >> ~/.bashrc
    echo 'export GOPATH=$HOME/go' >> ~/.bashrc
    echo 'export PATH=$GOROOT/bin:$GOPATH/bin:$PATH' >> ~/.bashrc
    ```
- **Reload .bashrc to apply changes**  
    ```bash
    source ~/.bashrc
    ```
- **Verify Go installation**  
    ```bash
    go version
    ```

---

# Install Node.js
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

### Avail DA Server Setup
> **Note:**  
> It’s essential to keep the Avail DA server running for subsequent steps.

1. **Clone and Navigate to Avail DA Server**  
    ```bash
    git clone https://github.com/availproject/avail-alt-da-server.git
    cd avail-alt-da-server
    ```
2. **Build Avail DA Server**  
    ```bash
    make da-server
    ```
3. **Run DA Sidecar**  
    ```bash
    ./bin/avail-da-server ./cmd/avail --addr=localhost --port=8000 --avail.rpc=<WS url to an avail node> --avail.seed=<> --avail.appid=<>
    ```
    > **Tip:**  
    > WS URLs can be found [here](https://docs.availproject.org/docs/networks) or run your own DA node (more complex setup).

---

### Running via Docker
1. Copy `.env.example` to `.env` and fill in necessary values.
2. Run the following commands:
    ```bash
    docker-compose build
    docker-compose up
    ```

---

### Build the Adapter Source
1. **Clone and Navigate to Avail Adapter**  
    ```bash
    git clone https://github.com/ethereum-optimism/optimism.git
    cd optimism
    git fetch --tag --all
    git checkout v1.9.1
    git submodule update --init --recursive
    ```
2. **Install Modules**  
    ```bash
    pnpm install
    ```
3. **Compile Necessary Packages**  
    ```bash
    make op-node op-batcher op-proposer
    pnpm build
    ```

---

### Build the Optimism Geth Source
1. **Clone and Navigate to op-geth**  
    ```bash
    git clone https://github.com/ethereum-optimism/op-geth.git
    cd op-geth
    git fetch --tag --all
    git checkout v1.101408.0
    ```
2. **Compile op-geth**  
    ```bash
    make geth
    ```

--- 
