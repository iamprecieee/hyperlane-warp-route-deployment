## Create a new directory e.g.
```bash
mkdir ./hyperlane-bridge
cd ./hyperlane-bridge
```

## Install nvm
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.4/install.sh | bash
```

## Set the nvm location as an environmental variable and load nvm into current shell session
```bash
export NVM_DIR="$HOME/.nvm"
source "$NVM_DIR/nvm.sh"
```

## Install node.js
```bash
nvm install node
```

## Install the hyperlane CLI globally
```bash
npm install -g @hyperlane-xyz/cli
```

## Set your private key as environmental variables (careful with this⚠️)
```bash
export HYP_KEY="your_private_key"
```

## Create a `.configs` directory and add a `warp-route-deployment.yaml` file
```bash
mkdir ./configs
nano ./configs/warp-route-deployment.yaml
```

## Add this code to the file and edit in chains and your wallet address:
```ini
"insert_chain_1_here":
    interchainSecurityModule:
        modules:
            - relayer: "your_wallet_address"
              type: trustedRelayerIsm
            - domains: {}
              owner: "your_wallet_address"
              type: defaultFallbackRoutingIsm
        threshold: 1
        type: staticAggregationIsm
    isNft: false
    mailbox: "chain_1_mailbox_contract address" # Check https://docs.hyperlane.xyz/docs/reference/contract-address
    owner: "your_wallet_address"
    token: "chain_1_token_contract_address"
    type: collateral
"insert_2nd_chain_here":
    interchainSecurityModule:
        modules:
            - relayer: "your_wallet_address"
              type: trustedRelayerIsm
            - domains: {}
              owner: "your_wallet_address"
              type: defaultFallbackRoutingIsm
        threshold: 1
        type: staticAggregationIsm
    isNft: false
    mailbox: "chain_2_mailbox_contract address"
    owner: "your_wallet_address"
    type: synthetic
```

### Example (For USDC between base mainnet and scroll mainnet)
```ini
base:
    interchainSecurityModule:
        modules:
            - relayer: "0x4..."
              type: trustedRelayerIsm
            - domains: {}
              owner: "0x4..."
              type: defaultFallbackRoutingIsm
        threshold: 1
        type: staticAggregationIsm
    isNft: false
    mailbox: "0xeA87ae93Fa0019a82A727bfd3eBd1cFCa8f64f1D"
    owner: "0x4..."
    token: "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913"
    type: collateral
scroll:
    interchainSecurityModule:
        modules:
            - relayer: "0x4..."
              type: trustedRelayerIsm
            - domains: {}
              owner: "0x4..."
              type: defaultFallbackRoutingIsm
        threshold: 1
        type: staticAggregationIsm
    isNft: false
    mailbox: "0x2f2aFaE1139Ce54feFC03593FeE8AB2aDF4a85A7"
    owner: "0x4..."
    type: synthetic
```

- Press "ctrl + x" to save.
- Press "y" to confirm.
- Press "enter" to exit.

## Run the warp command
```bash
hyperlane warp deploy --config ./configs/warp-route-deployment.yaml
```

## Reply the following runtime questions
Do you want to use an API key to verify on this (base) chain's block explorer (y/N)? 
- Select "N"

Do you want to use an API key to verify on this (scroll) chain's block explorer (y/N)? 
- Select "N"

Is this deployment plan correct? (Y/n)? 
- Select "Y"

[Optional]
Deployment may fail due to insufficient balance(s). Continue? (Y/n)? 
- Select "Y"  

(Ensure you have at least $5 worth of ETH on each chain for gas. Only a tiny fraction of this will be used though.)


### Example Result
```ini
    tokens:
      - chainName: base
        standard: EvmHypCollateral
        decimals: 6
        symbol: USDC
        name: USD Coin
        addressOrDenom: "0x9...6F"
        collateralAddressOrDenom: "0x8...13"
        connections:
          - token: ethereum|scroll|0xA...0d
      - chainName: scroll
        standard: EvmHypSynthetic
        decimals: 6
        symbol: USDC
        name: USD Coin
        addressOrDenom: "0xA...0d"
        connections:
          - token: ethereum|base|0x9...6F
```

⛽️ Gas Usage Statistics

- Gas required for warp deploy on base: 0.000041281779179263 ETH
- Gas required for warp deploy on scroll: 0.000307075270094679 ETH

### Copy the result from the output on your screen, or alternatively, run:
```bash
nano $HOME/.hyperlane/deployments/warp_routes/<enter_your_bridged_token_here>/<your_collateral_chain>-<your_synthetic_chain>-config.yaml # e.g. nano $HOME/.hyperlane/deployments/warp_routes/USDC/base-scroll-config.yaml
```

- Copy the entire code there, starting from `tokens...`.
- In your browser, open [hyperlane superbridge](https://hyperlane.superbridge.app/).
- Click on the settings button and select "customize" at the bottom
- Paste in your copied warp-route yaml code and click "save".
- Connect your wallet, select your desired chains and token.
- Bridge back and forth. The End.
