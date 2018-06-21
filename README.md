## Go TTC

Golang implementation of the TTC protocol.

## Running test on a private network

#### Create Directory

```
$ mkdir devnet
$ cd devnet
$ mkdir node1 node2
```

#### Create user account
  
```
$ geth --datadir node1/ account new
$ geth --datadir node2/ account new
```

#### Write account info into files for run network

```
$ echo 'node1_address' >> account.txt
$ echo 'node2_address' >> account.txt
$ echo 'password1' >> node1/password.txt
$ echo 'password2' >> node2/password.txt
```

#### Build genesis.json file by puppeth

```
$ puppeth
```

```
+-----------------------------------------------------------+
| Welcome to puppeth, your Ethereum private network manager |
|                                                           |
| This tool lets you create a new Ethereum network down to  |
| the genesis block, bootnodes, miners and ethstats servers |
| without the hassle that it would normally entail.         |
|                                                           |
| Puppeth uses SSH to dial in to remote servers, and builds |
| its network components out of Docker containers using the |
| docker-compose toolset.                                   |
+-----------------------------------------------------------+

Please specify a network name to administer (no spaces or hyphens, please)
> devnet

Sweet, you can set this via --network=devnet next time!

INFO [06-04|12:33:34] Administering Ethereum network           name=devnet
WARN [06-04|12:33:34] No previous configurations found         path=/Users/tataufo/.puppeth/devnet

What would you like to do? (default = stats)
 1. Show network stats
 2. Configure new genesis
 3. Track new remote server
 4. Deploy network components
> 2

Which consensus engine to use? (default = alien)
 1. Ethash - proof-of-work
 2. Clique - proof-of-authority
 3. Alien  - delegated-proof-of-stake
> 3

How many seconds should blocks take? (default = 15)
> 4

How many blocks create for one epoch? (default = 30000)
> 30

What is the max number of signers? (default = 21)
> 3

Which accounts are vote by themselves to seal the block?(least one, those accounts will be auto pre-funded)
> 0xfa846876ef5ed3826e483303f42d987a66af8e15
> 0x62739566c666df9a057d7e7c92898511d4e64c07
> 0x

Which accounts should be pre-funded? (advisable at least one)
> 0x

Specify your chain/network ID if you want an explicit one (default = random)
> 
INFO [06-04|12:35:27] Configured new genesis block 

What would you like to do? (default = stats)
 1. Show network stats
 2. Manage existing genesis
 3. Track new remote server
 4. Deploy network components
> 2

 1. Modify existing fork rules
 2. Export genesis configuration
 3. Remove genesis configuration
> 2

Which file to save the genesis into? (default = devnet.json)
> genesis.json
INFO [06-04|12:35:45] Exported existing genesis block 

What would you like to do? (default = stats)
 1. Show network stats
 2. Manage existing genesis
 3. Track new remote server
 4. Deploy network components
> ^C

```
#### Initialize the node

```
$ geth --datadir node1/ init genesis.json
$ geth --datadir node2/ init genesis.json
```

#### Create bootnode

```
$ bootnode -genkey boot.key
```

#### Run bootnode

```
$ bootnode -nodekey boot.key -verbosity 9 -addr :30310
```

#### Run node1 and node2

```
$ geth --datadir node1/ --syncmode 'full' --port 30311 --rpc --rpcaddr 'localhost' --rpcport 8501 --rpcapi 'personal,db,eth,net,web3,txpool,miner,net' --bootnodes 'enode://20940eac58b9e615706ea3c357c409aecbb44998d1388db49a8df61e727f92029019708b2ad69467f94eef9a49b5d4ffb2cc1e71bb06addeb134fe8bdbc62153@127.0.0.1:30310' --networkid 1014 --gasprice '1' -unlock 'fa846876ef5ed3826e483303f42d987a66af8e15' --password node1/password.txt --mine
```
```
$ geth --datadir node2/ --syncmode 'full' --port 30312 --rpc --rpcaddr 'localhost' --rpcport 8502 --rpcapi 'personal,db,eth,net,web3,txpool,miner,net' --bootnodes 'enode://20940eac58b9e615706ea3c357c409aecbb44998d1388db49a8df61e727f92029019708b2ad69467f94eef9a49b5d4ffb2cc1e71bb06addeb134fe8bdbc62153@127.0.0.1:30310' --networkid 1014 --gasprice '1' -unlock '62739566c666df9a057d7e7c92898511d4e64c07' --password node2/password.txt --mine
```

#### Message show in Terminator

```
INFO [06-01|17:14:00] Maximum peer count                       ETH=25 LES=0 total=25
INFO [06-01|17:14:00] Starting peer-to-peer node               instance=Geth/v0.0.1-unstable/darwin-amd64/go1.10.1
INFO [06-01|17:14:00] Allocated cache and file handles         database=/Users/tataufo/myeth/node2/geth/chaindata cache=768 handles=128
INFO [06-01|17:14:00] Initialised chain configuration          config="{ChainID: 1014 Homestead: 1 EIP150: 2 EIP155: 3 EIP158: 3 Byzantium: 4 Constantinople: <nil> Engine: alien}"
INFO [06-01|17:14:00] Initialising Ethereum protocol           versions="[63 62]" network=1014
INFO [06-01|17:14:00] Loaded most recent local header          number=0 hash=ba34cb…19036c td=1
INFO [06-01|17:14:00] Loaded most recent local full block      number=0 hash=ba34cb…19036c td=1
INFO [06-01|17:14:00] Loaded most recent local fast block      number=0 hash=ba34cb…19036c td=1
INFO [06-01|17:14:00] Regenerated local transaction journal    transactions=0 accounts=0
INFO [06-01|17:14:00] Starting P2P networking 
INFO [06-01|17:14:02] UDP listener up                          self=enode://c965ca2abf97c6f5aa02a72cdcdbd5b95d2a5321f80a86f66bfbc41586c3d7f4b21f01f9b34ad9ffb5798c697b28f0803ae640ef35f0667d4deb3de363319348@[::]:30312
INFO [06-01|17:14:02] RLPx listener up                         self=enode://c965ca2abf97c6f5aa02a72cdcdbd5b95d2a5321f80a86f66bfbc41586c3d7f4b21f01f9b34ad9ffb5798c697b28f0803ae640ef35f0667d4deb3de363319348@[::]:30312
INFO [06-01|17:14:02] IPC endpoint opened                      url=/Users/tataufo/myeth/node2/geth.ipc
INFO [06-01|17:14:02] HTTP endpoint opened                     url=http://localhost:8502               cors= vhosts=localhost
INFO [06-01|17:14:03] Unlocked account                         address=0x62739566c666Df9a057D7E7c92898511D4e64c07
INFO [06-01|17:14:03] Transaction pool price threshold updated price=1
INFO [06-01|17:14:03] Etherbase automatically configured       address=0x62739566c666Df9a057D7E7c92898511D4e64c07
INFO [06-01|17:14:03] Starting mining operation 
INFO [06-01|17:14:03] Commit new mining work                   number=1 txs=0 uncles=0 elapsed=107.751µs
INFO [06-01|17:14:03] Successfully sealed new block            number=1 hash=dde724…14a56c
INFO [06-01|17:14:03] 🔨 mined potential block                  number=1 hash=dde724…14a56c
INFO [06-01|17:14:03] Commit new mining work                   number=2 txs=0 uncles=0 elapsed=388.179µs
INFO [06-01|17:14:03] Signed recently, must wait for others 
INFO [06-01|17:14:12] Block synchronisation started 
INFO [06-01|17:14:12] Mining aborted due to sync 
INFO [06-01|17:14:12] Imported new chain segment               blocks=1 txs=0 mgas=0.000 elapsed=986.446µs mgasps=0.000 number=1 hash=d65c9c…361536 cache=0.00B
INFO [06-01|17:14:12] Starting mining operation 
INFO [06-01|17:14:12] Commit new mining work                   number=2 txs=0 uncles=1 elapsed=60.644µs
INFO [06-01|17:14:12] Successfully sealed new block            number=2 hash=3e4bc0…155c06
INFO [06-01|17:14:12] 🔨 mined potential block                  number=2 hash=3e4bc0…155c06
INFO [06-01|17:14:12] Commit new mining work                   number=3 txs=0 uncles=1 elapsed=604.414µs
INFO [06-01|17:14:12] Signed recently, must wait for others 

```

## Vote to change the Signer node

#### Build a new node and Run

```
$ mkdir node3
$ geth --datadir node3/ account new
$ echo 'password3' >> node3/password.txt
$ geth --datadir node3/ init genesis.json
$ geth --datadir node3/ --syncmode 'full' --port 30313 --rpc --rpcaddr 'localhost' --rpcport 8503 --rpcapi 'personal,db,eth,net,web3,txpool,miner,net' --bootnodes 'enode://20940eac58b9e615706ea3c357c409aecbb44998d1388db49a8df61e727f92029019708b2ad69467f94eef9a49b5d4ffb2cc1e71bb06addeb134fe8bdbc62153@127.0.0.1:30310' --networkid 1014 --gasprice '1' -unlock '62739566c666df9a057d7e7c92898511d4e64c07' --password node3/password.txt --mine
```

Before vote by Signers, the message show in termintor should like
```
INFO [06-12|15:09:35] Imported new chain segment               blocks=1   txs=0  mgas=0.000 elapsed=361.629µs mgasps=0.000 number=1015 hash=6e83e3…c8a679 cache=1.72kB
INFO [06-12|15:09:35] Commit new mining work                   number=1016 txs=0  uncles=0 elapsed=142.422µs
WARN [06-12|15:09:35] Block sealing failed
```

#### Vote by signers
```
$ geth attach node1/geth.ipc console
```

```
> alien
{
  proposals: {
  },
  discard: function(),
  getProposals: function(callback),
  getSigners: function(),
  getSignersAtHash: function(),
  getSnapshot: function(),
  getSnapshotAtHash: function(),
  propose: function()
}
> alien.getSigners()
["0xfa846876ef5ed3826e483303f42d987a66af8e15","0x62739566c666df9a057d7e7c92898511d4e64c07"]
```

Two signers which create by puppeth when create the genesis.json file.

Now create propose by alien.propose, first params is the address you want to vote, second param is your opinion.
```
> alien.propose("0xd90a772c478863d11fa48b1a48ccd69cc45c56ce",true)


> alien.getSnapshot()
{
  hash: "0x4a21f5d21323f06242dc13964e5c91b0b328caf51b5256f14a9af90426bffe60",
  number: 3560,
  recents: {
    3559: "0xfa846876ef5ed3826e483303f42d987a66af8e15",
    3560: "0x62739566c666df9a057d7e7c92898511d4e64c07"
  },
  signers: {
    0xfa846876ef5ed3826e483303f42d987a66af8e15: {},
    0x62739566c666df9a057d7e7c92898511d4e64c07: {}
  },
  tally: {
    0xd90a772c478863d11fa48b1a48ccd69cc45c56ce: {
      authorize: true,
      votes: 1
    }
  },
  votes: [{
      address: "0xd90a772c478863d11fa48b1a48ccd69cc45c56ce",
      authorize: true,
      block: 3559,
      signer: "0x2a84f498d27805d49a92277edbe670b83036f14b"
  }]
}

```

When the number of vote 'YES' to the address is more the (all signer) / 2 + 1, the address can seal new block.

```
> alien.getSigners()
["0xfa846876ef5ed3826e483303f42d987a66af8e15","0x62739566c666df9a057d7e7c92898511d4e64c07","0xd90a772c478863d11fa48b1a48ccd69cc45c56ce"]
```

The new node can seal new block now!
```
INFO [06-12|15:21:29] Commit new mining work                   number=1118 txs=0  uncles=0 elapsed=146.143µs
INFO [06-12|15:21:36] Successfully sealed new block            number=1118 hash=da6dee…122995
INFO [06-12|15:21:36] 🔗 block reached canonical chain          number=1113 hash=877129…a96eed
INFO [06-12|15:21:36] 🔨 mined potential block                  number=1118 hash=da6dee…122995
```