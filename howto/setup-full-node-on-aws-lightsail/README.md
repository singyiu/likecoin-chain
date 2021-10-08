# How to setup full node on AWS lightsail (3 months free tier)

## Goal: Setup one or a small number of full nodes / validators with a cost effective cloud solution.

### [AWS lightsail](https://aws.amazon.com/lightsail/)

- Select a platform
  - Linux/Unix
- Select a blueprint
  - OS Only
    - Ununtu 20.04 LTS
- Generate and save a SSH key pair if needed
- Enable Automatic Snapshots
  - i.e. if you plan to setup multiple nodes such that you create new node with disk image snapshot where the likecoin-chain is synchronized.
- Instance plan
  - $10 USD per/month
  - 2 GB RAM, 1 vCPU, 60 GB SSD, 3TB transfer
  - **N.B. as of the writing of this doc, a full node take ~50 GB of diskspace. Please choose the next tier instance for longer terms.**
  - 3 months free tier for new user
- Identify your instance
  - Please give your instance a name
- Click the "Create instance" button after reviewing the settings
- Once the new instance is ready in a few minutes. Click into its detail config.
  - Networking
    - Attach a public static IP (it is free with your lightsail instance ^\_^)
    - IPv4 Firewall
      - Add rule
        - Custom TCP 26656-26657
        - Save settings
  - Connect
    - Connect using SSH
    - It will open a browser based SSH terminal shell on the instance

### [Setup a node](https://docs.like.co/validator/likecoin-chain-node/setup-a-node)

- Install docker and tools
  ```
  sudo apt-get update
  sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
  sudo apt-get install docker-ce docker-ce-cli containerd.io
  ```
- Install likecoin-chain
  - Simply follow the steps from this [doc](https://docs.like.co/validator/likecoin-chain-node/setup-a-node)
  - And prefix all commands with "sudo" i.e.
    ```
    sudo ./build.sh
    ...
    sudo docker-compose run --rm init
    ...
    sudo docker-compose up -d
    ```
  - Give it some time for the node to fully start up. And use the following command to check its status
    ```
    curl http://127.0.0.1:26657/status
    ```
  - If you are creating a new node from a snapshot where the likecoin-chain is already synchronized, simply stop the node, keep the .liked directory and re-run the configure commands, i.e.
    ```
    sudo docker-compose stop
    sudo docker-compose run --rm init to create
    sudo docker-compose run --rm liked-command keys add validator
    sudo docker-compose up -d
    ```
