## Simplified version of Build Your First Network Hypeledger Fabric

This project basically clone of the Hyperledger Fabric first-network sample
while to make it easy to use and work with clients SDK it provides
configuration without TLS.

There is no need to generate any crypto material or channel realted artifacts
this repository already contains demo samples including everything.


1. Start network

```
docker-compose -f docker-compose-cli.yaml up
```

2. Login into docker cli container

```
docker exec -it cli /bin/bash
```

3. Create a new channel

```
peer channel create -o orderer.example.com -c mychannel -f channel-artifacts/channel.tx
```

4. Join channel

```
peer channel join -o orderer.example.com -b mychannel.block
```

5. Install chaincode

```
peer chaincode install -n myChaincode -v 1.0 -p github.com/hyperledger/fabric/examples/go/chaincode_example02
```

6. Instantiate chaincode

```
peer chaincode instantiate -o orderer.example.com:7050 -n myChaincode -v 1.0 -C mychannel -c '{"Args": ["init", "a", "100", "b", "200"]}'
```


There are two organization in the given setup with two peers each, in order to
switch between peers and organization following environmental variables have to
be updated:

First organization and peer0 will look as following (configured by default in
cli container):

```
export CORE_PEER_LOCALMSPID="Org1MSP"
export CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganization/org1.example.com/users/Admin@org1.example.com/msp
export CORE_PEER_ADDRESS=peer0.org1.example.com:7051
```

Or to the second org and peer0

```
export CORE_PEER_LOCALMSPID="Org2MSP"
export CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganization/org2.example.com/users/Admin@org2.example.com/msp
export CORE_PEER_ADDRESS=peer0.org2.example.com:7051
```


The directions for using this are documented in the Hyperledger Fabric
["Build Your First Network"](http://hyperledger-fabric.readthedocs.io/en/latest/build_network.html) tutorial.
