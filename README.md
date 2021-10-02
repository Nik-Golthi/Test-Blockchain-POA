# How to Set Up a Multi-Node Proof-of-Authority (POA) Blockchain

## **Part 1: Create accounts for your network nodes with a separate datadir**

Create multiple nodes by running the following geth command in your terminal:

##### **./geth --datadir node1 account new**

For example, I created two nodes (zbank1 & zbank2) for my testnet blockchain with the following commands:

##### **./geth --datadir zbank1 account new**
##### **./geth --datadir zbank2 account new**  

#### Create Nodes
![Create Nodes with geth]()

This command will prompt you to create a password for each node. After setting a password for each node, a public key and secret key file location is generated for the node. For future reference, store each node's password, public key, and secret key file location in a separate file.

##### Store node credentials for future use:
![Node Credentials]()



## **Part 2: Create Network and Configure Genesis Block**

1)  Run ./puppeth in your terminal. This initiates the process of creating your network and configuring your genesis block.

2)  Next, enter a network name of your choice when prompted. For my testnet, I used a network name of "homework".

3) At the next prompt, select option 2, "Configure new genesis".

4) Select "Clique - proof-of-authority" (option 2) as your consensus engine.

5) At the next prompt, hit enter to accept the default option for blocktime.

6) At the next prompt, enter the public key of the nodes created during Part 1 above to seal the node addresses. The node's public key addresses should have been stored for later use. Make sure to omit the first two characters ("0x") of the public key when entering the address into your terminal.

7) Pre-fund both node accounts by repeating the above step at the next prompt.

8) Select "no" when asked with pre-compile node addresses with 1 wei.

9) The next step defines the chain/network ID. Enter a three-digit code. For example, I used a chain ID of "232" for my testnet. Store the chain ID in a separate file for future reference.

10) Setting the chain ID will conclude the process of configuring your genesis block, and send you back to the first prompt. Once there, select "Manage existing genesis" (option 2).

11) Select "Export genesis configurations" (option 2). This will create 2 files and fail to create 2 files. For the purposes of running our blockchain, we'll only need one of the two files created (networkname.json). 

Below is a look at the end-to-end process of creating your network and configuring your genesis block:

##### Creating Network & Configuring 
![network_genesis_block_config]()

## **Part 3: Initialize Nodes

Initialize nodes by running the following geth command in your terminal:

##### **./geth --datadir node1 init networkname.json**

For example, I initialized my nodes the following commands:

##### **./geth --datadir zbank1 init homework.json**
##### **./geth --datadir zbank2 init homework.json**  

##### Initialize First Node 
![initialize_zbank1]()

##### Initialize Second Node 
![initialize_zbank2]()

## **Part 4: Run Nodes and Start Mining Blocks

Run your first node with the following command: 

##### **./geth --datadir node1 --unlock "SEALER_ONE_ADDRESS" --mine --rpc --allow-insecure-unlock**

The sealer address refers to the node's public key address, which we saved in a previous step. Here's what I executed to run my first node:

##### **./geth --datadir zbank1 --unlock "0xfB8d70cb8AD379402cB4C3Da128e8Ad76866B109" --mine --rpc --allow-insecure-unlock**

After executing this command, you'll need to enter the node password that was defined in an earlier step to unlock the account. Once the account has been unlocked, you should see a message that says "Unlocked Account".

Additionally, if all steps were executed properly, the node should start mining blocks. You should see messages that say "Commit new mining work", "Successfully sealed new block", and "mined potential block" in your terminal window.

##### Run First Node 
![zbank1_run_unlock]()

Prior to running our second node, we need to save one more bit of information: the first nodes enode address. We should see the enode address in our terminal window after running the above geth command to run the first node. The enode address will be to right of a message that says "Started P2P networking". We'll need this enode address to sync our second node with the first node. Find the address and store it in a separate file.

##### Store Enode Address
![enode_address]()

In a new terminal window, run your second node with the followng command:

##### **./geth --datadir node2 --unlock "SEALER_TWO_ADDRESS" --mine --port 30304 --bootnodes "enode://SEALER_ONE_ENODE_ADDRESS@127.0.0.1:30303" --ipcdisable --allow-insecure-unlock**

Once again, the sealer address refers to the second node's public key address, which was saved earlier. The enode address is the address we just saved after running our first node. Below is the command I executed to run my second node:

##### **./geth --datadir zbank2 --unlock "0x37837C20EA04c25F7c77536BC72Cba2B3ef1BfF2" --mine --port 30304 --bootnodes "enode://163b20d1c8ac6e0d5bbfedb19d42185d36ee88af6e967233c2e411ebc70aded92100f35da31b3ddc9ca3157a25b08942846215d0307e1aecf0c0523cace5fe99@127.0.0.1:30303" --ipcdisable --allow-insecure-unlock**

As with the first node, we'll need to unlock the second node account with the node password defined earlier after running this command. If all steps were executed properly, the second node will sync with the first node and start mining blocks.

##### Run Second Node 
![zbank2_run_unlock]()

## **Part 5: Send a Test Transaction in MyCrypto App

At this point, both nodes should be running and mining new blocks. The final step is to send a test transaction. Our approach will be to send funds from the first node to the second node.

1) Open MyCrypto application

2) Set up a custom network by selecting "Change Network" in the bottom left corner of the MyCrypto window, and then selecting "Add Custom Node".

3) Set the parameters of your custom network as follows:

- Node Name: Set equal to the network name you defined in puppeth
- Network: Custom
- Network Name: Set equal to the network name you defined in puppeth
- Currency: Set to "ETH"
- Chain ID: Set equal to the chain ID you defined when configuring your genesis block
- URL: Set to http://127.0.0.1:8545

Below is my custom network setup for my testnet:

##### Custom Network Parameters
![custom_network1]()

![custom_network2]()

4) After connecting to your custom network, navigate to the "View & Send" tab to initialize your transaction

5) Select "Keystore File" and select your first node's keystore file. The location of this file should have been saved in a previous step. Use the node password to unlock the keystore file. After unlocking, you should see your first node's pre-funded balance:

![node1_wallet_balance]()

6) Initiate a transfer from your first node to the second node:

- Enter second node address in "To Address" field.
- Enter an amount of ETH in the "Amount" field.
- Select "Send Transaction".

7) Sending the transaction will prompt a dialog box with a "Check TX Status" box. Click on "Check TX Status" to see the status and full details of the transaction.

##### Check TX Status
![check_tx_status]()

If the transaction executed successfully, you should see a "Successful" message on the status page:

#### Successful Transaction
![tx_success]()