## Steps to start a Quarkid Identity node (DID Method: did:quarkid)

As a prerequisite, the node has components that must be started together: [Api-zkSync](https://github.com/ssi-quarkid/api-zkSync), [Api-Proxy](https://github.com/ssi-quarkid/api-proxy), MongoDB, and IPFS.

## What is the Quarkid identity network?
The Quarkid identity network is based on *Sidetree*, an innovative second-layer protocol scalability layer that improves the efficiency and versatility of Decentralized Identifiers (DIDs). Sidetree allows us to perform operations on DIDs, such as creation, updating, and revocation, with efficiency that far exceeds the limits of first-layer implementations.
The QuarkID network is anchored in [*zkSync*](https://zksync.io/), a secure and scalable second-layer protocol for Ethereum. We leverage zkSync's capabilities to enable low-cost and high-speed transactions, thus providing an efficient utility infrastructure for our network and its adoption.

## Description of the anchoring network
The Quarkid network is built on the foundation of the zkSync blockchain, with the aim of providing stability and efficiency. It also supports other types of networks, such as RSK and any ETH-compatible network.
This network architecture is designed to support a high volume of DID operations with fast transaction confirmation times and minimal transaction costs.
To enable interaction on the network, we have deployed two smart contracts on the zkSync blockchain, one for the main network (Mainnet) and another for the test network (Testnet). These smart contracts facilitate the creation, updating, and revocation of DIDs on the network, in addition to providing the necessary functionality to anchor and resolve DIDs using Sidetree.

### Mainnet Network
For the mainnet network, the smart contract can be found at the following address:
- ZKSync: 0xe0055B74422Bec15cB1625792C4aA0beDcC61AA7
- Block: 2424399
- DID Method: did:quarkid
- Main network information:
- Network name: zkSync Era Mainnet
- RPC URL: https://mainnet.era.zksync.io
- Chain ID: 324
- Currency symbol: ETH
- Block explorer URL: https://explorer.zksync.io/

### Testnet Network
For the testnet network, the smart contract can be found at the following address:
ZKSync: 0x2535412fA22D9ad83384D7Ab7b636DDA37eFA872
Block: 505069
DID Method: did:quarkid:zksync
This data ensures synchronization with the active Quarkid testnet network.
Test network information:
- Network name: zkSync Era Testnet
- RPC URL: https://sepolia.era.zksync.dev
- Chain ID: 280
- Currency symbol: ETH
- Block explorer URL: https://sepolia.explorer.zksync.io/

To obtain funds on the zkSync network, it is necessary to use a bridge available to transfer Ether from the main network (Mainnet) or a test network (Testnet) such as Sepolia, to the corresponding (ZkSync) network.

## Relationship with the API Proxy component
API proxy, also known as reverse proxy, is a component designed to function as an entry point to multiple did methods with the aim of facilitating their handling and resolution since each implementation has its own specifications and dynamics.
The component works with *three endpoints*:

### First endpoint
```
GET /mappings
```

Its response provides us with a list of available nodes to operate with Api Proxy.
Example:
```
{
    "list": [
         {
            "url":"https://URL_TO_api-zksync",
            "pattern": "did:quarkid",
            "behavior": 1
          }
    ]
}
```

### Second endpoint
```
GET /resolve/:did
```

It receives a DID as a parameter and gives us the power to resolve a did-document. In this case, the proxy will return the document varying from the consumed method. For example, Api-zksync defines how the response will be, noting that this component only has the proxy function.

### Third endpoint
```
POST /create
```
This endpoint receives the did-method to use and the request as parameters. Like the previous one, its response varies depending on the did-registry.

## Relationship with the API zkSync component
It is a component that follows SideTree specifications to interact with a blockchain and efficiently manage decentralized records. It uses:

### 1. MongoDB
NoSQL database oriented to documents, which stores data in flexible JSON-like structures called BSON (Binary JSON).

### 2. IPFS
IPFS, or the InterPlanetary File System, is a protocol and network designed to create a decentralized and distributed method of file storage and access that seeks to connect all computing devices with the same file system.

## zkSync Network
As clarified earlier, this Ethereum scalability solution uses Zero-Knowledge Proofs (zkProofs) to enable fast and secure transactions with significantly lower fees than the main network. zkSync's "rollup" technology groups hundreds of off-chain Ethereum transactions into a single batch, and then proves their validity on the main blockchain (on-chain) with a single zero-knowledge proof.

## Obtaining the components
### Prerequisites
1. Have Docker installed on the system that will be used for installation.
2. Have Internet access and necessary permissions to download images from the official [Docker](https://hub.docker.com/u/quarkid) container registry.

### Steps
Step 1: Open Docker. Once authenticated, you can proceed to download the necessary images with the *docker pull* command:
```
 # docker pull quarkid/api-proxy
 # docker pull quarkid/api-zksync
 # docker pull ipfs/kubo:latest
 # docker pull mongo:latest
```

Step 2: Verify the downloaded images with *docker image ls*:
```
 REPOSITORY TAG IMAGE ID SIZE
 quarkid/api-proxy latest cb8941a6fbe4 16 hours ago 1.26GB
 quarkid/api-zksync latest 13e12acfd1c0 5 months ago 3.56GB
 ipfs/kubo latest 71f8fff78bb2 3 days ago 94.6MB
 mongo
```
### Configuration of component variables

### API Proxy
1. Variable: NODE_1_URL
   - Value: IP:PORT
   - Description: IP and Port of the API Proxy component
2. Variable: NODE_1_PATTERN
   - Value: did:quarkid
   - Description: Did method that NODE_1_URL resolves
3. Variable: NODE_1_BEHAVIOR
   - Value: 1
   - Description: Active

### API zkSync
The following variables must be configured for this component:
1. Variable: DID_METHOD_NAME
   - Value: quarkid
   - Description: Did method
2. Variable: CONTENT_ADDRESSABLE_STORE_SERVICE_URI
   - Value: IP:PORT
   - Description: IP and Port of the IPFS component
3. Variable: DATABASE_NAME
   - Value: -zksync-mainnet-v1
   - Description: Database name
4. Variable: RPC_URL
   - Value: https://mainnet.era.zksync.io
   - Description: URL of the network where the DID will be anchored
5. Variable: MONGO_DB_CONNECTION_STRING
   - Value: mongodb://10.1.0.2:27017
   - Description: Connection string to the MongoDB component
6. Variable: MAX_CONCURRENT_DOWNLOADS
   - Value: 20
   - Description: Number of operations to download per interval
7. Variable: OBSERVING_INTERVAL_IN_SECONDS
   - Value: 30
   - Description: Blockchain reading time
8. Variable: BATCHING_INTERVAL_IN_SECONDS
   - Value: 21600
   - Description: Blockchain writing time
9. Variable: BLOCKCHAIN_VERSION
   - Value: latest 
   - Description: Blockchain Version
10. Variable: MODENA_ANCHO_CONTRACT
    - Value: 0xe0055B74422Bec15cB1625792C4aA0beDcC61AA7
    - Description: Address of the Quarkid smart contract
11. WALLET_PRIVATE_KEY:
    - Value: XXX
    - Description: Private key of address with zkSync network balance
12. ACCOUNT_ADDRESS:
    - Value: 0x9CAA73a4865fa9dbb125b6C7B2f03b6621234
    - Description: Address of the previous private key
13. LEDGER_TYPE:
    - Value: zksync | rsk | eth
    - Description: Blockchain network to use

### MongoDB
This component has no additional configurations.

### IPFS
This component has no additional configurations.

----------------------------------------------------------------------------------------------------------------------------
## Deployment Example

To simplify, we will give an example of the process of deploying and managing our container infrastructure using Docker Compose.

The components have been successfully tested on the following orchestrators:
1. Azure - AKS
2. Google - GKS
3. AWS - EKS
4. Docker-compose
5. Any orchestrator that uses Docker as its containerization technology.

To perform these tests, you must have an address configured in Testnet and you must transfer balance to be able to execute the transactions.
```
ACCOUNT_ADDRESS==****
WALLET_PRIVATE_KEYS=***
```

An example of a docker-compose.yml file:
```yaml
version: '3.7'
services:
  api-sksync:
    image: quarkid/api-zksync:latest
    container_name: api-zksync-cont
    ports:
      - 8000:3000
    restart: always
    environment:
      - DID_METHOD_NAME=quarkid:zksync
      - CONTENT_ADDRESSABLE_STORE_SERVICE_URI=http://ipfs-quarkid:5001/
      - DATABASE_NAME=zksync-testnet-v1
      - RPC_URL=https://sepolia.era.zksync.dev
      - MONGO_DB_CONNECTION_STRING=mongodb://mongo-quarkid:27017
      - MAX_CONCURRENT_DOWNLOADS=20
      - OBSERVING_INTERVAL_IN_SECONDS=30
      - BATCHING_INTERVAL_IN_SECONDS=60
      - STARTING_BLOCKCHAIN_TIME=505069
      - BLOCKCHAIN_VERSION=latest
      - MODENA_ANCHOR_CONTRACT=0x2535412fA22D9ad83384D7Ab7b636DDA37eFA872
      - WALLET_PRIVATE_KEY=***
      - ACCOUNT_ADDRESS=***
      - LEDGER_TYPE=zksync

  api-proxy:
    image: quarkid/api-proxy:latest
    container_name: api-proxy-cont
    ports:
      - 8080:8080 #MODENA API port
    environment:
      - NODE_1_URL=api-sksync:8000
      - NODE_1_PATTERN=did:quarkid:zksync
      - NODE_1_BEHAVIOR=1

  mongo-quarkid:
    image: mongo
    restart: always
    expose:
      - 27017
    ports:
      - 27017:27017

  ipfs:
    restart: always
    container_name: ipfs-quarkid
    image: ipfs/kubo
    environment:
      - IPFS_PATH=/data/ipfs
    volumes:
      - ./ipfs:/data/ipfs
    ports:
      - 4001:4001
      - 5001:5001
```
When reviewing the api-zksync logs, you will notice that the first few minutes show synchronization with other nodes. In some cases, messages of the type {"code":"content_not_found"} can be generated, which means that another node recorded a transaction on the network, but IPFS was not opened to synchronize the information. It does not affect the node, only that those DIDs will not be able to be resolved. That's why it's important that IPFS is correctly synchronized.
