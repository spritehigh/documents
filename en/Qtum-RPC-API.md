# qtum-cli RPC API

##1. getblockcount

Returns the number of blocks in the longest blockchain.

####Result:

    n (numeric) The current block count

####Examples:
    qtum-cli getblockcount  
    
    curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblockcount", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test: 

    ./qtum-cli getblockcount
####test result: 

    395049

##2. getblockchaininfo 
Returns an object containing various state info regarding blockchain processing.
####Result:
    {
      "chain": "xxxx",  (string) current network name as defined in BIP70 (main, test, regtest)
      "blocks": xxxxxx, (numeric) the current number of blocks processed in the server
      "headers": xxxxxx,(numeric) the current number of headers we have validated
      "bestblockhash": "...",   (string) the hash of the currently best block
      "difficulty": xxxxxx, (numeric) the current difficulty
      "mediantime": xxxxxx, (numeric) median time for the current best block
      "verificationprogress": xxxx, (numeric) estimate of verification progress [0..1]
      "initialblockdownload": xxxx, (bool) (debug information) estimate of whether this node is in Initial Block Download mode.
      "chainwork": "xxxx"   (string) total amount of work in active chain, in hexadecimal
      "size_on_disk": xxxxxx,   (numeric) the estimated size of the block and undo files on disk
      "pruned": xx, (boolean) if the blocks are subject to pruning
      "pruneheight": xxxxxx,(numeric) lowest-height complete block stored (only present if pruning is enabled)
      "automatic_pruning": xx,  (boolean) whether automatic pruning is enabled (only present if pruning is enabled)
      "prune_target_size": xxxxxx,  (numeric) the target size used by pruning (only present if automatic pruning is enabled)
      "softforks": [(array) status of softforks in progress
     {
    "id": "xxxx",   (string) name of softfork
    "version": xx,  (numeric) block version
    "reject": { (object) progress toward rejecting pre-softfork blocks
       "status": xx,(boolean) true if threshold reached
    },
     }, ...
      ],
      "bip9_softforks": {   (object) status of BIP9 softforks in progress
     "xxxx" : { (string) name of the softfork
    "status": "xxxx",   (string) one of "defined", "started", "locked_in", "active", "failed"
    "bit": xx,  (numeric) the bit (0-28) in the block version field used to signal this softfork (only for "started" status)
    "startTime": xx,(numeric) the minimum median time past of a block at which the bit gains its meaning
    "timeout": xx,  (numeric) the median time past of a block at which the deployment is considered failed if not yet locked in
    "since": xx,(numeric) height of the first block to which the status applies
    "statistics": { (object) numeric statistics about BIP9 signalling for a softfork (only for "started" status)
       "period": xx,(numeric) the length in blocks of the BIP9 signalling period 
       "threshold": xx, (numeric) the number of blocks with the version bit set required to activate the feature 
       "elapsed": xx,   (numeric) the number of blocks elapsed since the beginning of the current period 
       "count": xx, (numeric) the number of blocks with the version bit set in the current period 
       "possible": xx   (boolean) returns false if there are not enough blocks left in this period to pass activation threshold 
    }
     }
      }
      "warnings" : "...",   (string) any network and blockchain warnings.
    }
####Examples:
    qtum-cli getblockchaininfo  
    curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblockchaininfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test: 

    ./qtum-cli getblockchaininfo

####test result:
    {  
      "chain": "main",  
      "blocks": 395059,  
      "headers": 395059,  
      "bestblockhash":     "dab486c7fbd8272d4778ac2deaa304115124dc6316ed10abca80cac90f5718b0",  
      "difficulty": 2088741.734870087,  
      "moneysupply": 101560236,  
      "mediantime": 1561102352,  
      "verificationprogress": 0.9999975023702973,  
      "initialblockdownload": false,             "chainwork":"00000000000000000000000000000000000000000000011028ade7c86f6bced1",  
      "size_on_disk": 1902329468,  
      "pruned": false,  
       "softforks": [  
    {  
      "id": "bip34",  
      "version": 2,  
      "reject": {  
    "status": true  
      }  
    },  
    {  
      "id": "bip66",  
      "version": 3,  
      "reject": {  
    "status": true  
      }  
    },  
    {  
      "id": "bip65",  
      "version": 4,  
      "reject": {  
    "status": true  
      }  
    }  
      ],  
##3. getblockhash height     

Returns hash of block in best-block-chain at height provided.

####Arguments:
```
1. height         (numeric, required) The height index
```
####Result:   
```
"hash"         (string) The block hash  
```


####Examples:
    qtum-cli getblockhash 1000  
    curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblockhash", "params": [1000] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test example:

    ./qtum-cli getblockhash 1
####test result

    0000d5dab5e76310ae640e9bcfa270c2eb23a1e5948bdf01fc7ed1f157110ab7
##4. getbestblockhash
Returns the hash of the best (tip) block in the longest blockchain.

####Result:
    "hex" (string) the block hash hex encoded

####Examples:
    qtum-cli getbestblockhash
    curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getbestblockhash", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

    
####test: 
    ./qtum-cli getbestblockhash

####test result
    e006ada4d1b7caf1559cc1b5b520ab8c54f51486230f2ea18d2692d3a095ba03

##5. getblock “blockhash” ( verbosity )
According the blockhash returns the info of the corresponding block
If verbosity is 0, returns a string that is serialized, hex-encoded data for block 'hash'.
If verbosity is 1, returns an Object with information about block <hash>.
If verbosity is 2, returns an Object with information about block <hash> and information about each transaction.

####Arguments:
    
    1. "blockhash" (string, required) The block hash
    2. verbosity (numeric, optional, default=1) 0 for hex encoded data, 1 for a json object, and 2 for json object with transaction data


####Result (for verbosity = 0):
"data" (string) A string that is serialized, hex-encoded data for block 'hash'.

####Result (for verbosity = 1):
    
    {
    "hash" : "hash", (string) the block hash (same as provided)
    "confirmations" : n, (numeric) The number of confirmations, or -1 if the block is not on the main chain
    "size" : n, (numeric) The block size
    "strippedsize" : n, (numeric) The block size excluding witness data
    "weight" : n (numeric) The block weight as defined in BIP 141
    "height" : n, (numeric) The block height or index
    "version" : n, (numeric) The block version
    "versionHex" : "00000000", (string) The block version formatted in hexadecimal
    "merkleroot" : "xxxx", (string) The merkle root
    "tx" : [ (array of string) The transaction ids
    "transactionid" (string) The transaction id
    ,...
    ],
    "time" : ttt, (numeric) The block time in seconds since epoch (Jan 1 1970 GMT)
    "mediantime" : ttt, (numeric) The median block time in seconds since epoch (Jan 1 1970 GMT)
    "nonce" : n, (numeric) The nonce
    "bits" : "1d00ffff", (string) The bits
    "difficulty" : x.xxx, (numeric) The difficulty
    "chainwork" : "xxxx", (string) Expected number of hashes required to produce the chain up to this block (in hex)
    "nTx" : n, (numeric) The number of transactions in the block.
    "previousblockhash" : "hash", (string) The hash of the previous block
    "nextblockhash" : "hash" (string) The hash of the next block
    }
    
####Result (for verbosity = 2):
    {
    ..., Same output as verbosity = 1.
    "tx" : [ (array of Objects) The transactions in the format of the getrawtransaction RPC. Different from verbosity = 1 "tx" result.
    ,...
    ],
    ,... Same output as verbosity = 1.
    }

####Examples:
    qtum-cli getblock "00000000c937983704a73af28acdec37b049d214adbda81d7e2a3dd146f6ed09"curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblock", "params": ["00000000c937983704a73af28acdec37b049d214adbda81d7e2a3dd146f6ed09"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test: ./qtum-cli getblock “e006ada4d1b7caf1559cc1b5b520ab8c54f51486230f2ea18d2692d3a095ba03”
####test result:
    {
      "hash": "e006ada4d1b7caf1559cc1b5b520ab8c54f51486230f2ea18d2692d3a095ba03",
      "confirmations": 2,
      "strippedsize": 2476,
      "size": 2512,
      "weight": 9940,
      "height": 395088,
      "version": 536870912,
      "versionHex": "20000000",
      "merkleroot": "5802715bc0fbf2e0c8b57897dce54588e84ad960bbc628fc0f1f32273c38083a",
      "hashStateRoot": "0005694a50658c62d899a721add29062d967afafd4a00127c1ac686411e21963",
      "hashUTXORoot": "9e729950c184acd011471252a0c1a4bc279cd4c1e86d543bead4af6df787b2dd",
      "tx": [
    "33431772ee0cd8f25199b72cf7376ccbc0537071404da7063d000445a4526d63",
    "8db77cd5cb3ede61d8ec74120849622a4b55ff8f558446847bf3b38231268f72",
    "e90bb01ea9394e4ab52d8fc48f372eb84b73103809aec933f177ce82815a74cc",
    "d96440303ee90c580cf1c673eb755de8071afac99a1fd2dc482e89dc7d384736",
    "c75406bb75843e34ba261895aaf335255087a140ae9034dcef3b7d477f2102fa"
      ],
      "time": 1561106576,
      "mediantime": 1561106112,
      "nonce": 0,
      "bits": "1a0517d8",
      "difficulty": 3294031.021738609,
      "chainwork": "0000000000000000000000000000000000000000000001102cf321f6db3a4252",
      "nTx": 5,
      "previousblockhash": "a09450133593a207dca70952ce7bd998b630769959677374086cf625736bd54c",
      "nextblockhash": "0526fc099637edf47f99b03c97e3ee800912bb40c97e37aa67634efe7799e4d9",
      "flags": "proof-of-stake",
      "proofhash": "0000517443e18dd77519e269d5ab2827cf136f72af0dc2ed965f49d45d129a59",
      "modifier": "1737e6f76fbf16f0b93345030a6ad82b9447254fdbf46673beb2c08e58ff2415",
      "signature": "304402204b22223c5137101e63dae31e2b6de74781f4179a8ebaf4c1284af7f10f9ceded022057e0091402acf50b725b030b22e82ddf84f7c5a78a04f56cd75235c0051f9df7"
    }
 
##6. getchaintips

Return information about all known tips in the block tree, including the main chain as well as orphaned branches.

####Result:
    [
    {
    "height": xxxx, (numeric) height of the chain tip
    "hash": "xxxx", (string) block hash of the tip
    "branchlen": 0 (numeric) zero for main chain
    "status": "active" (string) "active" for the main chain
    },
    {
    "height": xxxx,
    "hash": "xxxx",
    "branchlen": 1 (numeric) length of branch connecting the tip to the main chain
    "status": "xxxx" (string) status of the chain (active, valid-fork, valid-headers, headers-only, invalid)
    }
    ]
Possible values for status:
1. "invalid" This branch contains at least one invalid block
2. "headers-only" Not all blocks for this branch are available, but the headers are valid
3. "valid-headers" All blocks are available for this branch, but they were never fully validated
4. "valid-fork" This branch is not part of the active chain, but is fully validated
5. "active" This is the tip of the active main chain, which is certainly valid

####Examples:
    > qtum-cli getchaintips
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getchaintips", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test:

    ./qtum-cli getchaintips

####test result
    [
      
    "height": 395704,
    "hash": "48d5de2867af7f6deb82fde7ca3d7bfcfc8c1ed9dee7c3cf24448751cb659780",
    "branchlen": 582,
    "status": "headers-only"
      },
      
    "height": 395122,
    "hash": "56151c72011de07544b862a94fa61a8f364cc3c5a1560946b490aa943b847c34",
    "branchlen": 0,
    "status": "active"
      },
      
    "height": 395056,
    "hash": "cdc100382a0baaa0b050ded1e3ca1a3b7a7a94701b6b50752dff08d7fe28ee9b",
    "branchlen": 1,
    "status": "valid-fork"
      },
      
    "height": 389780,
    "hash": "f7bd50a97eb66c85083e171fabf53ce68ecb6e56fba962b385794d95d476312e",
    "branchlen": 1,
    "status": "valid-fork"
      },
      
    "height": 389625,
    "hash": "49d390c457affbb2cd594fa6c3f4f3c8c3eb7967a13002efd4e4e1dbc0108b81",
    "branchlen": 1,
    "status": "valid-fork"
      },
      
    "height": 388910,
    "hash": "ebd10c9b338247a9ccfd45493b484ae5638a5d97ddaa68c44c6ef214ea443c19",
    "branchlen": 1,
    "status": "valid-fork"
      },
      
    "height": 388905,
    "hash": "6c9806ab069badb011c94b6802f56ad40f5cc2ea58b23ce5f7bc1c6c597c3cce",
    "branchlen": 1,
    "status": "valid-fork"
      },
      
    "height": 388280,
    "hash": "fcab5b4f196f1a20582bd545cf763d7bb8411bc226676182d9bb6aaf9fa036ad",
    "branchlen": 1,
    "status": "valid-fork"
      
    ]
##7. getdifficulty(notice the difference with Bitcoin)
Returns the proof-of-work difficulty as a multiple of the minimum difficulty.

Returns the proof-of-stake difficulty as a multiple of the minimum difficulty.

####Result:

    n.nnn (numeric) the proof-of-work difficulty as a multiple of the minimum difficulty.

####Examples:
    > qtum-cli getdifficulty
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getdifficulty", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/
    
####test 

    ./qtum-cli getdifficulty
####test result:
    {
      "proof-of-work": 1.52587890625e-05,
      "proof-of-stake": 7022116.100551808
    }
##8. getblockheader “hash”( verbose )

Returns the corresponding block header information according to the given index
If verbose is false, returns a string that is serialized, hex-encoded data for blockheader 'hash'.
If verbose is true, returns an Object with information about blockheader <hash>.

####Arguments:
1. "hash" (string, required) The block hash
2. verbose (boolean, optional, default=true) true for a json object, false for the hex encoded data

####Result (for verbose = true):
    {
    "hash" : "hash", (string) the block hash (same as provided)
    "confirmations" : n, (numeric) The number of confirmations, or -1 if the block is not on the main chain
    "height" : n, (numeric) The block height or index
    "version" : n, (numeric) The block version
    "versionHex" : "00000000", (string) The block version formatted in hexadecimal
    "merkleroot" : "xxxx", (string) The merkle root
    "time" : ttt, (numeric) The block time in seconds since epoch (Jan 1 1970 GMT)
    "mediantime" : ttt, (numeric) The median block time in seconds since epoch (Jan 1 1970 GMT)
    "nonce" : n, (numeric) The nonce
    "bits" : "1d00ffff", (string) The bits
    "difficulty" : x.xxx, (numeric) The difficulty
    "chainwork" : "0000...1f3" (string) Expected number of hashes required to produce the current chain (in hex)
    "nTx" : n, (numeric) The number of transactions in the block.
    "previousblockhash" : "hash", (string) The hash of the previous block
    "nextblockhash" : "hash", (string) The hash of the next block
    }

####Result (for verbose=false):
"data" (string) A string that is serialized, hex-encoded data for block 'hash'.

####Examples:
        > qtum-cli getblockheader "00000000c937983704a73af28acdec37b049d214adbda81d7e2a3dd146f6ed09"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblockheader", "params": ["00000000c937983704a73af28acdec37b049d214adbda81d7e2a3dd146f6ed09"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test example:

    ./qtum-cli getblockheader “ebd10c9b338247a9ccfd45493b484ae5638a5d97ddaa68c44c6ef214ea443c19”
####test result:
    {
      "hash": "ebd10c9b338247a9ccfd45493b484ae5638a5d97ddaa68c44c6ef214ea443c19",
      "confirmations": -1,
      "height": 388910,
      "version": 536870912,
      "versionHex": "20000000",
      "merkleroot": "b1a21dd48f978ea8671383f9454d058d2047d19666348340bea543cf89e31aca",
      "time": 1560222144,
      "mediantime": 1560221568,
      "nonce": 0,
      "bits": "1a097561",
      "difficulty": 1773742.122273433,
      "chainwork": "00000000000000000000000000000000000000000000010ca5944ed9b867aaef",
      "nTx": 7,
      "hashStateRoot": "10504057696a3ad9f96254b86424cc8f49f3ef2b271893f933b18174e538b828",
      "hashUTXORoot": "9e729950c184acd011471252a0c1a4bc279cd4c1e86d543bead4af6df787b2dd",
      "previousblockhash": "56044826105d66a95ab6f97f945a7cd18eef7109c59da64a7b6c57c377eaf4bb",
      "flags": "proof-of-stake",
      "proofhash": "0000000000000000000000000000000000000000000000000000000000000000",
      "modifier": "1551ed22c1a43da60aebcb2d66a1e42d9bf6a007276367a4a189325ea37a1f91"
    }
##9. getpeerinfo
Returns data about each connected network node as a json array of objects.

####Result:
    [
    {
    "id": n, (numeric) Peer index
    "addr":"host:port", (string) The IP address and port of the peer
    "addrbind":"ip:port", (string) Bind address of the connection to the peer
    "addrlocal":"ip:port", (string) Local address as reported by the peer
    "services":"xxxxxxxxxxxxxxxx", (string) The services offered
    "relaytxes":true|false, (boolean) Whether peer has asked us to relay transactions to it
    "lastsend": ttt, (numeric) The time in seconds since epoch (Jan 1 1970 GMT) of the last send
    
    "lastrecv": ttt, (numeric) The time in seconds since epoch (Jan 1 1970 GMT) of the last receive
    "bytessent": n, (numeric) The total bytes sent
    "bytesrecv": n, (numeric) The total bytes received
    "conntime": ttt, (numeric) The connection time in seconds since epoch (Jan 1 1970 GMT)
    "timeoffset": ttt, (numeric) The time offset in seconds
    "pingtime": n, (numeric) ping time (if available)
    "minping": n, (numeric) minimum observed ping time (if any at all)
    "pingwait": n, (numeric) ping wait (if non-zero)
    "version": v, (numeric) The peer version, such as 70001
    "subver": "/Satoshi:0.8.5/", (string) The string version
    "inbound": true|false, (boolean) Inbound (true) or Outbound (false)
    "addnode": true|false, (boolean) Whether connection was due to addnode/-connect or if it was an automatic/inbound connection
    "startingheight": n, (numeric) The starting height (block) of the peer
    "banscore": n, (numeric) The ban score
    "synced_headers": n, (numeric) The last header we have in common with this peer
    "synced_blocks": n, (numeric) The last block we have in common with this peer
    "inflight": [
    n, (numeric) The heights of blocks we're currently asking from this peer
    ...
    ],
    "whitelisted": true|false, (boolean) Whether the peer is whitelisted
    "bytessent_per_msg": {
    "addr": n, (numeric) The total bytes sent aggregated by message type
    ...
    },
    "bytesrecv_per_msg": {
    "addr": n, (numeric) The total bytes received aggregated by message type
    ...
    }
    }
    ,...
    ]

####Examples:
    > qtum-cli getpeerinfo
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getpeerinfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/
####test example：

    ./qtum-cli getpeerinfo
####test result：
    "bytesrecv_per_msg": 
    {
      "addr": 55,
      "feefilter": 32,
      "getheaders": 1021,
      "headers": 277,
      "ping": 32,
      "pong": 32,
      "sendcmpct": 66,
      "sendheaders": 24,
      "verack": 24,
      "version": 126
    }
    
  
##10. getblocktemplate ( TemplateRequest )

If the request parameters include a 'mode' key, that is used to explicitly select between the default 'template' request or a 'proposal'.
It returns data needed to construct a block to work on.
For full specification, see BIPs 22, 23, 9, and 145:
https://github.com/bitcoin/bips/blob/master/bip-0022.mediawiki
https://github.com/bitcoin/bips/blob/master/bip-0023.mediawiki
https://github.com/bitcoin/bips/blob/master/bip-0009.mediawiki#getblocktemplate_changes
https://github.com/bitcoin/bips/blob/master/bip-0145.mediawiki

####Arguments:
    1. template_request (json object, optional) A json object in the following spec
    {
    "mode":"template" (string, optional) This must be set to "template", "proposal" (see BIP 23), or omitted
    "capabilities":[ (array, optional) A list of strings
    "support" (string) client side supported feature, 'longpoll', 'coinbasetxn', 'coinbasevalue', 'proposal', 'serverlist', 'workid'
    ,...
    ],
    "rules":[ (array, optional) A list of strings
    "support" (string) client side supported softfork deployment
    ,...
    ]
    }


####Result:
    {
    "version" : n, (numeric) The preferred block version
    "rules" : [ "rulename", ... ], (array of strings) specific block rules that are to be enforced
    "vbavailable" : { (json object) set of pending, supported versionbit (BIP 9) softfork deployments
    "rulename" : bitnumber (numeric) identifies the bit number as indicating acceptance and readiness for the named softfork rule
    ,...
    },
    "vbrequired" : n, (numeric) bit mask of versionbits the server requires set in submissions
    "previousblockhash" : "xxxx", (string) The hash of current highest block
    "transactions" : [ (array) contents of non-coinbase transactions that should be included in the next block
    {
    "data" : "xxxx", (string) transaction data encoded in hexadecimal (byte-for-byte)
    "txid" : "xxxx", (string) transaction id encoded in little-endian hexadecimal
    "hash" : "xxxx", (string) hash encoded in little-endian hexadecimal (including witness data)
    "depends" : [ (array) array of numbers
    n (numeric) transactions before this one (by 1-based index in 'transactions' list) that must be present in the final block if this one is
    ,...
    ],
    "fee": n, (numeric) difference in value between transaction inputs and outputs (in satoshis); for coinbase transactions, this is a negative Number of the total collected block fees (ie, not including the block subsidy); if key is not present, fee is unknown and clients MUST NOT assume there isn't one
    "sigops" : n, (numeric) total SigOps cost, as counted for purposes of block limits; if key is not present, sigop cost is unknown and clients MUST NOT assume it is zero
    "weight" : n, (numeric) total transaction weight, as counted for purposes of block limits
    }
    ,...
    ],
    "coinbaseaux" : { (json object) data that should be included in the coinbase's scriptSig content
    "flags" : "xx" (string) key name is to be ignored, and value included in scriptSig
    },
    "coinbasevalue" : n, (numeric) maximum allowable input to coinbase transaction, including the generation award and transaction fees (in satoshis)
    "coinbasetxn" : { ... }, (json object) information for coinbase transaction
    "target" : "xxxx", (string) The hash target
    "mintime" : xxx, (numeric) The minimum timestamp appropriate for next block time in seconds since epoch (Jan 1 1970 GMT)
    "mutable" : [ (array of string) list of ways the block template may be changed
    "value" (string) A way the block template may be changed, e.g. 'time', 'transactions', 'prevblock'
    ,...
    ],
    "noncerange" : "00000000ffffffff",(string) A range of valid nonces
    "sigoplimit" : n, (numeric) limit of sigops in blocks
    "sizelimit" : n, (numeric) limit of block size
    "weightlimit" : n, (numeric) limit of block weight
    "curtime" : ttt, (numeric) current timestamp in seconds since epoch (Jan 1 1970 GMT)
    "bits" : "xxxxxxxx", (string) compressed target of next block
    "height" : n (numeric) The height of the next block
    }

####Examples:
    > qtum-cli getblocktemplate {"rules": ["segwit"]}
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblocktemplate", "params": [{"rules": ["segwit"]}] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test example:

    ./qtum-cli getblocktemplate
####test result:
    {
      "capabilities": [
    "proposal"
      ],
      "version": 536870912,
      "rules": [
    "csv",
    "segwit"
      ],
      "vbavailable": {
      },
      "vbrequired": 0,
      "previousblockhash": "655f82168ae4d8e1daece29c689efb97e40d0cd84030991f89ebb4684d71edeb",
      "transactions": [
    
      "data": "0200000000020000000000000000000084d71700000000015100000000",
      "txid": "65b1a6eb24a3e0833a986eb88f969784811e2143ffce51d630873c1a49a2b596",
      "hash": "65b1a6eb24a3e0833a986eb88f969784811e2143ffce51d630873c1a49a2b596",
      "depends": [
      ],
      "fee": 0,
      "sigops": 140596761067656,
      "weight": 116
    
      ],
      "coinbaseaux": {
    "flags": ""
      },
      "coinbasevalue": 0,
      "longpollid": "655f82168ae4d8e1daece29c689efb97e40d0cd84030991f89ebb4684d71edeb1394",
      "target": "00000000000003f3700000000000000000000000000000000000000000000000",
      "mintime": 1561300241,
      "mutable": [
    "time",
    "transactions",
    "prevblock"
      ],
      "noncerange": "00000000ffffffff",
      "sigoplimit": 80000,
      "sizelimit": 8000000,
      "weightlimit": 8000000,
      "curtime": 1561300872,
      "bits": "1a03f370",
      "height": 396447
    }
##11. getconnectioncount
Returns the number of connections to other nodes.

####Result:

    n (numeric) The connection count

####Examples:
    > qtum-cli getconnectioncount
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getconnectioncount", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/
####test example:

    ./qtum-cli getconnectioncount
####test result


    7
##12. getmininginfo
Returns a json object containing mining-related information.
####Result:
    {
    "blocks": nnn, (numeric) The current block
    "currentblockweight": nnn, (numeric) The last block weight
    "currentblocktx": nnn, (numeric) The last block transaction
    "difficulty": xxx.xxxxx (numeric) The current difficulty
    "networkhashps": nnn, (numeric) The network hashes per second
    "pooledtx": n (numeric) The size of the mempool
    "chain": "xxxx", (string) current network name as defined in BIP70 (main, test, regtest)
    "warnings": "..." (string) any network and blockchain warnings
    }

####Examples:
     qtum-cli getmininginfo  
     curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getmininginfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/
####test example:

    ./qtum-cli getmininginfo
####test result:
    {
      "blocks": 396733,
      "currentblockweight": 4000,
      "currentblocktx": 0,
      "difficulty": {
    "proof-of-work": 1.52587890625e-05,
    "proof-of-stake": 1542341.073682025,
    "search-interval": 0
      },
      "blockvalue": 400000000,  
      "netmhashps": 0,
      "netstakeweight": 1073403030567011,
      "errors": "",
      "networkhashps": 64552713419283.81,
      "pooledtx": 2,
      "stakeweight": {
    "minimum": 0,
    "maximum": 0,
    "combined": 0
      },
      "chain": "main",
      "warnings": ""
    }
##13. getmempoolinfo
Returns details on the active state of the TX memory pool.

####Result:
    {
    "size": xxxxx, (numeric) Current tx count  
    "bytes": xxxxx, (numeric) Sum of all virtual transaction sizes as defined in BIP 141. Differs from actual serialized size because witness data is discounted  
    "usage": xxxxx, (numeric) Total memory usage for the mempool  
    "maxmempool": xxxxx, (numeric) Maximum memory usage for the mempool  
    "mempoolminfee": xxxxx (numeric) Minimum fee rate in QTUM/kB for tx to be accepted. Is the maximum of minrelaytxfee and minimum mempool fee  
    "minrelaytxfee": xxxxx (numeric) Current minimum relay fee for transactions  
    }

####Examples:
    > qtum-cli getmempoolinfo
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getmempoolinfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test example:


    ./qtum-cli getmempoolinfo
####test result:
    {
      "size": 10,
      "bytes": 3582,
      "usage": 14176,
      "maxmempool": 300000000,
      "mempoolminfee": 0.00400000,
      "minrelaytxfee": 0.00400000
    }
##14. getmempoolancestors txid(verbose)

If txid is in the mempool, returns all in-mempool ancestors.

####Arguments:
        1. "txid" (string, required) The transaction id (must be in mempool)
    2. verbose (boolean, optional, default=false) True for a json object, false for array of transaction ids
    
   #### Result (for verbose=false):
    [ (json array of strings)
    "transactionid" (string) The transaction id of an in-mempool ancestor transaction
    ,...
    ]

####Result (for verbose=true):
    { (json object)
    "transactionid" : { (json object)
    "size" : n, (numeric) virtual transaction size as defined in BIP 141. This is different from actual serialized size for witness transactions as witness data is discounted.
    "fee" : n, (numeric) transaction fee in QTUM (DEPRECATED)
    "modifiedfee" : n, (numeric) transaction fee with fee deltas used for mining priority (DEPRECATED)
    "time" : n, (numeric) local time transaction entered pool in seconds since 1 Jan 1970 GMT
    "height" : n, (numeric) block height when transaction entered pool
    "descendantcount" : n, (numeric) number of in-mempool descendant transactions (including this one)
    "descendantsize" : n, (numeric) virtual transaction size of in-mempool descendants (including this one)
    "descendantfees" : n, (numeric) modified fees (see above) of in-mempool descendants (including this one) (DEPRECATED)
    "ancestorcount" : n, (numeric) number of in-mempool ancestor transactions (including this one)
    "ancestorsize" : n, (numeric) virtual transaction size of in-mempool ancestors (including this one)
    "ancestorfees" : n, (numeric) modified fees (see above) of in-mempool ancestors (including this one) (DEPRECATED)
    "wtxid" : hash, (string) hash of serialized transaction, including witness data
    "fees" : {
    "base" : n, (numeric) transaction fee in QTUM
    "modified" : n, (numeric) transaction fee with fee deltas used for mining priority in QTUM
    "ancestor" : n, (numeric) modified fees (see above) of in-mempool ancestors (including this one) in QTUM
    "descendant" : n, (numeric) modified fees (see above) of in-mempool descendants (including this one) in QTUM
    }
    "depends" : [ (array) unconfirmed transactions used as inputs for this transaction
    "transactionid", (string) parent transaction id
    ... ]
    "spentby" : [ (array) unconfirmed transactions spending outputs from this transaction
    "transactionid", (string) child transaction id
    ... ]
    }, ...
    }
    
####Examples:
    > qtum-cli getmempoolancestors "mytxid"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getmempoolancestors", "params": ["mytxid"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test example:
`./qtum-cli getmempoolancestors a957d309824b760814feb6426ba386d082f3b8bc95837e3e7ebada6538cf7e2c   `   
####test result:
    [
      "c3d044940534fd94fd0c901a895f62505e7beba0dfa44b1563c7aea980279135"
    ]
##15. getmempooldescendants txid (verbose)

If txid is in the mempool, returns all in-mempool descendants.

####Arguments:
    1. "txid" (string, required) The transaction id (must be in mempool)
    2. verbose (boolean, optional, default=false) True for a json object, false for array of transaction ids

####Result (for verbose=false):
    [ (json array of strings)
    "transactionid" (string) The transaction id of an in-mempool descendant transaction
    ,...
    ]

####Result (for verbose=true):
    { (json object)
    "transactionid" : { (json object)
    "size" : n, (numeric) virtual transaction size as defined in BIP 141. This is different from actual serialized size for witness transactions as witness data is discounted.
    "fee" : n, (numeric) transaction fee in QTUM (DEPRECATED)
    "modifiedfee" : n, (numeric) transaction fee with fee deltas used for mining priority (DEPRECATED)
    "time" : n, (numeric) local time transaction entered pool in seconds since 1 Jan 1970 GMT
    "height" : n, (numeric) block height when transaction entered pool
    "descendantcount" : n, (numeric) number of in-mempool descendant transactions (including this one)
    "descendantsize" : n, (numeric) virtual transaction size of in-mempool descendants (including this one)
    "descendantfees" : n, (numeric) modified fees (see above) of in-mempool descendants (including this one) (DEPRECATED)
    "ancestorcount" : n, (numeric) number of in-mempool ancestor transactions (including this one)
    "ancestorsize" : n, (numeric) virtual transaction size of in-mempool ancestors (including this one)
    "ancestorfees" : n, (numeric) modified fees (see above) of in-mempool ancestors (including this one) (DEPRECATED)
    "wtxid" : hash, (string) hash of serialized transaction, including witness data
    "fees" : {
    "base" : n, (numeric) transaction fee in QTUM
    "modified" : n, (numeric) transaction fee with fee deltas used for mining priority in QTUM
    "ancestor" : n, (numeric) modified fees (see above) of in-mempool ancestors (including this one) in QTUM
    "descendant" : n, (numeric) modified fees (see above) of in-mempool descendants (including this one) in QTUM
    }
    "depends" : [ (array) unconfirmed transactions used as inputs for this transaction
    "transactionid", (string) parent transaction id
    ... ]
    "spentby" : [ (array) unconfirmed transactions spending outputs from this transaction
    "transactionid", (string) child transaction id
    ... ]
    }, ...
    }

####Examples:
    > qtum-cli getmempooldescendants "mytxid"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getmempooldescendants", "params": ["mytxid"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test result:
    [
    "9d980a4fcdf13fb2c9a5c7769ad6f3e8668aba1f0608be09ef84a11afaf3d03f",
    "89874d6f44bb3b8a526c50cecda1cbe06c6c6e8107623b79222ee75b79f91d5a",
    "0c2d893fdc510a6fddb18fc3d441b02d5b6050b754dc6f5d5ddd251707c3d995"
    ]
##16. getrawmempool

Returns all transaction ids in memory pool as a json array of string transaction ids.

Hint: use getmempoolentry to fetch a specific transaction from the mempool.

####Arguments:
    1. verbose (boolean, optional, default=false) True for a json object, false for array of transaction ids
    
    Result: (for verbose = false):
    [ (json array of string)
    "transactionid" (string) The transaction id
    ,...
    ]

####Result: (for verbose = true):
    { (json object)
    "transactionid" : { (json object)
    "size" : n, (numeric) virtual transaction size as defined in BIP 141. This is different from actual serialized size for witness transactions as witness data is discounted.
    "fee" : n, (numeric) transaction fee in QTUM (DEPRECATED)
    "modifiedfee" : n, (numeric) transaction fee with fee deltas used for mining priority (DEPRECATED)
    "time" : n, (numeric) local time transaction entered pool in seconds since 1 Jan 1970 GMT
    "height" : n, (numeric) block height when transaction entered pool
    "descendantcount" : n, (numeric) number of in-mempool descendant transactions (including this one)
    "descendantsize" : n, (numeric) virtual transaction size of in-mempool descendants (including this one)
    "descendantfees" : n, (numeric) modified fees (see above) of in-mempool descendants (including this one) (DEPRECATED)
    "ancestorcount" : n, (numeric) number of in-mempool ancestor transactions (including this one)
    "ancestorsize" : n, (numeric) virtual transaction size of in-mempool ancestors (including this one)
    "ancestorfees" : n, (numeric) modified fees (see above) of in-mempool ancestors (including this one) (DEPRECATED)
    "wtxid" : hash, (string) hash of serialized transaction, including witness data
    "fees" : {
    "base" : n, (numeric) transaction fee in QTUM
    "modified" : n, (numeric) transaction fee with fee deltas used for mining priority in QTUM
    "ancestor" : n, (numeric) modified fees (see above) of in-mempool ancestors (including this one) in QTUM
    "descendant" : n, (numeric) modified fees (see above) of in-mempool descendants (including this one) in QTUM
    }
    "depends" : [ (array) unconfirmed transactions used as inputs for this transaction
    "transactionid", (string) parent transaction id
    ... ]
    "spentby" : [ (array) unconfirmed transactions spending outputs from this transaction
    "transactionid", (string) child transaction id
    ... ]
    }, ...
    }

####Examples:
    > qtum-cli getrawmempool true
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getrawmempool", "params": [true] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test example: 
    ./qtum-cli getrawmempool
####test result:
    [
      "d4e995b2e0b5ef44f90659323e662f408f7938b5b97c345a40192ca6d0b06704",
      "9e41daae52ece96732305cb0873b8528e7011c866eb7c3c9bee2c22c03e5bf65",
      "354297aef68f48044af17c3f01616597dad298304e34728fa9894b3ff73a0f33",
      "e2c407e5468a3e885b73760e3c6115d6d493786fff5ec070e91ad9b4db58b2ef",
      "15f9f80b536041ebf5c3958c7b7d9bab72c800882235fc782925137c2addbdd2",
      "109483b1b06746cf4215f3786907b26213adc71a14145acccbba4f0952a751a3",
      "b49e03dd14a242803bf8108a10a9f82120e21e7d4b0a9255c632e7b92d879136",
      "40a295fa44931750c048feb817fad96e079ce193c4eda0770d68be6c1f7241c3",
      "f1e4f8814a404adce808e9b0aed56f61ec151c745cba3a6ac1f0a917884adb59",
      "b6772f160e21c4633f95b1c5831c6d0451cab1501c375730ea32de64860f1809",
    "65894a5fcf0e19e13375ba0d0f2afa6e73f52512bc69cf8225c40af824f740d9",
      "8199c4f6c9f8be79fff4fc9664755bc0502de92d9856083112e55caaa2cb1974",
      "c3fb5ef13f87f2988c879c274cfbc1613240a12a66e9259388101d82364f1934",
      "ca78e53edcfba07d4eee2a1bbfc07e26e79e5677104399c794e51d098c735056",
      "7f66e7f6443ebd0d3a3a8e6bd83e3a5ffc6d32ef5a1ee6487c7abb5cfae7d409",
      "97966a9f3baeac334cb547d553baf71bf7c68720f350444d7411e0855f39a574",
      "78285303f6deef553cd5fcd5084db0bae3ccf504ac66f614cb29a5a6f6b5e815",
      "e5ae989b9782c1afd2c6f71379e46d162ffbe804c5b8db70b7bc85d939df2efe",
      "2f09732e25eab121a347471a91be55c798cde96aa7f8009da51c2a2d322e6410",
      "c302d3177a64a4a3c7f2ba1ea196b69754475192afa8249eb18f6f055556c8e1",
      "7f1de19f02347ea3744bcb11364c9883b91e4f4883b13f15e9e9f52595c1d3d6",
      "c241f5896723fec65b3af4f94491679ae8a79c434b2e0749651cf416952abac0",
      "4d8932af7a7073e2c4a5527767215f7bf5b8c7759a5b49d976f0031d5184fd18"
    ]
##17. getmempoolentry txid
Returns mempool data for given transaction

####Arguments:

    1. "txid" (string, required) The transaction id (must be in mempool)

####Result:
    { (json object)
    "size" : n, (numeric) virtual transaction size as defined in BIP 141. This is different from actual serialized size for witness transactions as witness data is discounted.
    "fee" : n, (numeric) transaction fee in QTUM (DEPRECATED)
    "modifiedfee" : n, (numeric) transaction fee with fee deltas used for mining priority (DEPRECATED)
    "time" : n, (numeric) local time transaction entered pool in seconds since 1 Jan 1970 GMT
    "height" : n, (numeric) block height when transaction entered pool
    "descendantcount" : n, (numeric) number of in-mempool descendant transactions (including this one)
    "descendantsize" : n, (numeric) virtual transaction size of in-mempool descendants (including this one)
    "descendantfees" : n, (numeric) modified fees (see above) of in-mempool descendants (including this one) (DEPRECATED)
    "ancestorcount" : n, (numeric) number of in-mempool ancestor transactions (including this one)
    "ancestorsize" : n, (numeric) virtual transaction size of in-mempool ancestors (including this one)
    "ancestorfees" : n, (numeric) modified fees (see above) of in-mempool ancestors (including this one) (DEPRECATED)
    "wtxid" : hash, (string) hash of serialized transaction, including witness data
    "fees" : {
    "base" : n, (numeric) transaction fee in QTUM
    "modified" : n, (numeric) transaction fee with fee deltas used for mining priority in QTUM
    "ancestor" : n, (numeric) modified fees (see above) of in-mempool ancestors (including this one) in QTUM
    "descendant" : n, (numeric) modified fees (see above) of in-mempool descendants (including this one) in QTUM
    }
    "depends" : [ (array) unconfirmed transactions used as inputs for this transaction
    "transactionid", (string) parent transaction id
    ... ]
    "spentby" : [ (array) unconfirmed transactions spending outputs from this transaction
    "transactionid", (string) child transaction id
    ... ]
    }

####Examples:
    > qtum-cli getmempoolentry "mytxid"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getmempoolentry", "params": ["mytxid"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test result:
    {
      "fees": {
    "base": 0.10187027,
    "modified": 0.10187027,
    "ancestor": 0.10187027,
    "descendant": 0.10187027
      },
      "size": 446,
      "fee": 0.10187027,
      "modifiedfee": 0.10187027,
      "time": 1561349354,
      "height": 396786,
      "descendantcount": 1,
      "descendantsize": 446,
      "descendantfees": 10187027,
      "ancestorcount": 1,
      "ancestorsize": 446,
      "ancestorfees": 10187027,
      "wtxid": "110b5a1946c5d8673ddd07c28924263c57230c04e7f7b4209938c7e343935efd",
      "depends": [
      ],
      "spentby": [
      
    }
##18. getstorage(the unique of QTUM)

Get data stored by smart contracts

#### Argument:
     1. "address" (string, required) The address to get the storage from
     2. "blockNum" (string, optional) Number of block to get state from,  "latest" keyword supported. Latest if not passed.
     3. "index" (number, optional) Zero-based index position of the storage 
####Examples:
    > qtum-cli getstorage “contract address”
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getstorage", "params": ["address"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test example:

    getstorage "fdb9d0873ba524ef3ea67c1719666968e1eeb110"
####Result:
    {
      "000756ed9982214a7ba55dbe32d0321f5891e75d3c3c467d4b575d55991e03b1": {
    "52adc13b5e472402f13052709f282024cc52e564f96b2183a5737c545229228d": "0000000000000000000000000000000000000000000000000000000000000001"
      },
    }
##19. getaccountinfo “address”（QTUM独有）

根据地址获取对应的账户信息
Contract details including balance, storage data and code

####Argument:


    1. "address"(string, required) The contract address

####Result:

    Contract details including balance, storage data and code

####test example:

    ./qtum-cli getaccountinfo "fdb9d0873ba524ef3ea67c1719666968e1eeb110"


##20. getaccount “address”（deprecated）


Returns the account associated with the given address

####test example: 

    ./qtum-cli getaccount “QQrm6av1tWtTmvkTpft3FygcmLFcrEWGWk”

##21. getaccountaddress “account” (deprecated)

returns the current qtum address for receiving payments to this account

####Example:
    ./qtum-cli getaccountaddress
    ./qtum-cli getaccountaddress ""
    ./qtum-cli getaccountaddress "myaccount"
##22. getaddressesbyaccount (deprecated)

returns a list of every address assigned to a particular account.

####test examples:

    ./qtum-cli getaddressesbyaccount "taddy"
##23. getnewaddress
Returns a new Qtum address for receiving payments.
If 'label' is specified, it is added to the address book
so payments received with the address will be associated with 'label'.

####Arguments:
    1. "label" (string, optional) The label name for the address to be linked to. If not provided, the default label "" is used. It can also be set to the empty string "" to represent the default label. The label does not need to exist, it will be created if there is no label by the given name.
    2. "address_type" (string, optional) The address type to use. Options are "legacy", "p2sh-segwit", and "bech32". Default is set by -addresstype.

####Result:

    "address" (string) The new qtum address

####Examples:
    > qtum-cli getnewaddress
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getnewaddress", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test example:
    `./qtum-cli getnewaddress`
####test result:

    QcMKCuHMFwwGNBdYYes8vKN1Qm1MJkwsQf
##24. getaddressesbylabel:

Returns the list of addresses assigned the specified label.

####Arguments:

    1. "label" (string, required) The label.

####Result:
    { (json object with addresses as keys)
    "address": { (json object with information about address)
    "purpose": "string" (string) Purpose of address ("send" for sending address, "receive" for receiving address)
    },...
    }

####Examples:
    > qtum-cli getaddressesbylabel "tabby"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getaddressesbylabel", "params": ["tabby"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test example:

    ./qtum-cli getaddressesbylabel ""
####test result:
    {
      "QUTYNrXYMQUzZyUekmfakR8bzTuLqYcLtf": {
    "purpose": "receive"
      },
      "QZmURd7wViLVKjUu8b3mvdXeSJjFmH7hf2": {
    "purpose": "receive"
      
    }
##25. abandontransaction

Mark in-wallet transaction <txid> as abandoned
This will mark this transaction and all its in-wallet descendants as abandoned which will allow
for their inputs to be respent. It can be used to replace "stuck" or evicted transactions.
It only works on transactions which are not included in a block and are not currently in the mempool.
It has no effect on transactions which are already abandoned.

####Arguments:

    1. "txid" (string, required) The transaction id

####Result:

####Examples:
    > qtum-cli abandontransaction "1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "abandontransaction", "params": ["1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test:

    ./qtum-cli abandontransaction 23162caff532c72377f48b06d1f32b6383b20f4b1c67b3cf01f265a1e6a054b9
####test result：

    If the call succeeds, return null
##26. addmultisigaddress

Add a nrequired-to-sign multisignature address to the wallet. Requires a new wallet backup.
Each key is a Qtum address or hex-encoded public key.
This functionality is only intended for use with non-watchonly addresses.
See `importaddress` for watchonly p2sh address support.
If 'label' is specified, assign address to that label.

####Arguments:
            1. nrequired (numeric, required) The number of required signatures out of the n keys or addresses.
    2. "keys" (string, required) A json array of qtum addresses or hex-encoded public keys
    [
    "address" (string) qtum address or hex-encoded public key
    ...,
    ]
    3. "label" (string, optional) A label to assign the addresses to.
    4. "address_type" (string, optional) The address type to use. Options are "legacy", "p2sh-segwit", and "bech32". Default is set by -addresstype.

####Result:
    {
    "address":"multisigaddress", (string) The value of the new multisig address.
    "redeemScript":"script" (string) The string value of the hex-encoded redemption script.
    }

####Examples:

Add a multisig address from 2 addresses

    > qtum-cli addmultisigaddress 2 "[\"QjWnDZxwLhrJDcp4Hisse8RfBo2jRDZY5Z\",\"Q6sSauSf5pF2UkUwvKGq4qjNRzBZYqgEL5\"]"

As json rpc call

    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "addmultisigaddress", "params": [2, "[\"QjWnDZxwLhrJDcp4Hisse8RfBo2jRDZY5Z\",\"Q6sSauSf5pF2UkUwvKGq4qjNRzBZYqgEL5\"]"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/
##27.    abortrescan

Stops current wallet rescan triggered by an RPC call, e.g. by an importprivkey call.

####Examples:

    Import a private key
    > qtum-cli importprivkey "mykey"
    
    Abort the running wallet rescan
    > qtum-cli abortrescan
    
    As a JSON-RPC call
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "abortrescan", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test example:

    ./qtum-cli abortrescan
####test result:
    false

##28. addnode
addnode "node" "add|remove|onetry"

Attempts to add or remove a node from the addnode list.
Or try a connection to a node once.
Nodes added using addnode (or -connect) are protected from DoS disconnection and are not required to be
full nodes/support SegWit as other outbound peers are (though such peers will not be synced from).

####Arguments:
    1. "node" (string, required) The node (see getpeerinfo for nodes)
    
    2. "command" (string, required) 'add' to add a node to the list, 'remove' to remove a node from the list, 'onetry' to try a connection to the node once

####Examples 
    - Mainnet port 3888, Testnet port 13888, Regtest port 23888:
    > qtum-cli addnode "192.168.0.6:3888" "onetry"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "addnode", "params": ["192.168.0.6:3888", "onetry"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##29. witnessaddress(DEPRECATED)
DEPRECATED: set the address_type argument of getnewaddress, or option -addresstype=[bech32|p2sh-segwit] instead.
Add a witness address for a script (with pubkey or redeemscript known). Requires a new wallet backup.
It returns the witness script.

####Arguments:
    1. "address" (string, required) An address known to the wallet
    2. p2sh (bool, optional, default=true) Embed inside P2SH

####Result:
    "witnessaddress", (string) The value of the new address (P2SH or BIP173).
    }
##30. backupwallet 
backupwallet "destination"

Safely copies current wallet file to destination, which can be a directory or a path with filename.

####Arguments:

    1. "destination" (string) The destination directory or file

####Examples:
    > qtum-cli backupwallet "backup.dat"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "backupwallet", "params": ["backup.dat"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##31.bumpfee
bumpfee "txid" ( options )

Bumps the fee of an opt-in-RBF transaction T, replacing it with a new transaction B.
An opt-in RBF transaction with the given txid must be in the wallet.
The command will pay the additional fee by decreasing (or perhaps removing) its change output.
If the change output is not big enough to cover the increased fee, the command will currently fail
instead of adding new inputs to compensate. (A future implementation could improve this.)
The command will fail if the wallet or mempool contains a transaction that spends one of T's outputs.
By default, the new fee will be calculated automatically using estimatesmartfee.
The user can specify a confirmation target for estimatesmartfee.
Alternatively, the user can specify totalFee, or use RPC settxfee to set a higher fee rate.
At a minimum, the new fee rate must be high enough to pay an additional new relay fee (incrementalfee
returned by getnetworkinfo) to enter the node's mempool.

####Arguments:
    1. txid (string, required) The txid to be bumped
    2. options (object, optional)
    {
    "confTarget" (numeric, optional) Confirmation target (in blocks)
    "totalFee" (numeric, optional) Total fee (NOT feerate) to pay, in satoshis.
    In rare cases, the actual fee paid might be slightly higher than the specified
    totalFee if the tx change output has to be removed because it is too close to
    the dust threshold.
    "replaceable" (boolean, optional, default true) Whether the new transaction should still be
    marked bip-125 replaceable. If true, the sequence numbers in the transaction will
    be left unchanged from the original. If false, any input sequence numbers in the
    original transaction that were less than 0xfffffffe will be increased to 0xfffffffe
    so the new transaction will not be explicitly bip-125 replaceable (though it may
    still be replaceable in practice, for example if it has unconfirmed ancestors which
    are replaceable).
    "estimate_mode" (string, optional, default=UNSET) The fee estimate mode, must be one of:
    "UNSET"
    "ECONOMICAL"
    "CONSERVATIVE"
    }

####Result:
    {
    "txid": "value", (string) The id of the new transaction
    "origfee": n, (numeric) Fee of the replaced transaction
    "fee": n, (numeric) Fee of the new transaction
    "errors": [ str... ] (json array of strings) Errors encountered during processing (may be empty)
    }

####Examples:

Bump the fee, get the new transaction's txid

    > qtum-cli bumpfee <txid>

##32.callcontract
callcontract "address" "data" ( address )

####Argument:
    1. "address" (string, required) The account address
    2. "data" (string, required) The data hex string
    3. address (string, optional) The sender address hex string
    4. gasLimit (string, optional) The gas limit for executing the contract

####Result:
    "address": "XXX", (string) The address of the contract called
    "executionResult": {
    "gasUsed": n, (numeric) Fee of the new transaction
    "excepted": "None",
    "newAddress": “XXX”, a new contract address
    "output": “”, (a hex num), according input returns a hex number
    "codeDeposit": 0, Failed if an attempt deposit failed due to lack of gas.
    "gasRefunded": n, (numeric) Fee of returned gas
    "depositSize": 0,  amount of code of the creation’s attempted deposit
    "gasForDeposit": 0, amount of gas remaining for the code deposit phase
    },



####test example: 

    ./qtum-cli callcontract "74045ec0dc26ec1861473828bc140ebc4c1f3eff" "00000000000000000000000000000000000000000000000000000000000000a9"
####test result:
    {
    "address": "74045ec0dc26ec1861473828bc140ebc4c1f3eff",
    "executionResult": {
    "gasUsed": 39999999,
    "excepted": "None",
    "newAddress": "74045ec0dc26ec1861473828bc140ebc4c1f3eff",
    "output": "",
    "codeDeposit": 0,
    "gasRefunded": 0,
    "depositSize": 0,
    "gasForDeposit": 0
    },
    "transactionReceipt": {
    "stateRoot": "1253c56cf79597e89ce179f14e6a86a493356dac410c30efc576503687ad2670",
    "gasUsed": 39999999,
    "bloom": "00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
    "log": [
    ]
    }
    }

##33.clearbanned
Clear all banned IPs.

####Examples:
    > qtum-cli clearbanned
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "clearbanned", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####Result:
null

##34.combinepsbt ["psbt",...]

Combine multiple partially signed Qtum transactions into one transaction.
Implements the Combiner role.

####Arguments:
    1. "txs" (string) A json array of base64 strings of partially signed transactions
    [
    "psbt" (string) A base64 string of a PSBT
    ,...
    ]

####Result:

    "psbt" (string) The base64-encoded partially signed transaction

####Examples:

##35. combinerawtransaction ["hexstring",...]

Combine multiple partially signed transactions into one transaction.
The combined transaction may be another partially signed transaction or a
fully signed transaction.
####Arguments:
    1. "txs" (string) A json array of hex strings of partially signed transactions
    [
    "hexstring" (string) A transaction hash
    ,...
    ]

####Result:

    "hex" (string) The hex-encoded raw transaction with signature(s)

####Examples:

    > qtum-cli combinerawtransaction ["myhex1", "myhex2", "myhex3"]


##36.listaccounts (deprecated)
listaccounts is deprecated and will be removed in V0.18. To use this command, start qtumd with -deprecatedrpc=accounts. (code -32)

##37.listaddressgroupings

Lists groups of addresses which have had their common ownership
made public by common use as inputs or as the resulting change
in past transactions

####Result:
    [
    [
    [
    "address", (string) The qtum address
    amount, (numeric) The amount in QTUM
    "label" (string, optional) The label
    ]
    ,...
    ]
    ,...
    ]

####Examples:
    > qtum-cli listaddressgroupings
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listaddressgroupings", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test example:

    ./qtum-cli listaddressgroupings
####test result:
    [
    [
    [
    "QZmURd7wViLVKjUu8b3mvdXeSJjFmH7hf2",
    0.00000000,
    ""
    ]
    ]
    ]

##38. listbanned

List all banned IPs/Subnets.

####Examples:
    > qtum-cli listbanned
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listbanned", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test result:
    [
    ]

##39.listcontracts (start maxDisplay)

list all the contracts and default accounts is 20

####Argument:
    1. start (numeric or string, optional) The starting account index, default 1
    2. maxDisplay (numeric or string, optional) Max accounts to list, default 20

####Example:

    > ./qtum-cli listcontracts 1 5

####Result:
    {
    "82155d35dc1e0b5dc3d6ca7e536af42394a7003c": 0.00000000,
    "c50116ca622b4dbd12205fb9cc61a64f7b63cb8a": 0.00000000,
    "28d1140499604664be0037272eb287e1742dcafe": 0.00000000,
    "b9fe4ba102c33ba078d90a2cb6fe8fa94fd114a1": 0.00000000,
    "954999d28fd46c6de806f9587a82321437771ab2": 0.00000000
    }

##39.converttopsbt "hexstring" ( permitsigdata iswitness )

Converts a network serialized transaction to a PSBT. This should be used only with createrawtransaction and fundrawtransaction
createpsbt and walletcreatefundedpsbt should be used for new applications.

####Arguments:
    1. "hexstring" (string, required) The hex string of a raw transaction
    2. permitsigdata (boolean, optional, default=false) If true, any signatures in the input will be discarded and conversion.
    will continue. If false, RPC will fail if any signatures are present.
    3. iswitness (boolean, optional) Whether the transaction hex is a serialized witness transaction.
    If iswitness is not present, heuristic tests will be used in decoding. If true, only witness deserializaion
    will be tried. If false, only non-witness deserialization wil be tried. Only has an effect if
    permitsigdata is true.

####Result:

    "psbt" (string) The resulting raw transaction (base64-encoded string)

####Examples:

    Create a transaction
    > qtum-cli createrawtransaction "[{\"txid\":\"myid\",\"vout\":0}]" "[{\"data\":\"00010203\"}]"

    Convert the transaction to a PSBT
    > qtum-cli converttopsbt "rawtransaction"

##40. createwallet "wallet_name" ( disable_private_keys )

Creates and loads a new wallet.

####Arguments:
    1. "wallet_name" (string, required) The name for the new wallet. If this is a path, the wallet will be created at the path location.
    2. disable_private_keys (boolean, optional, default: false) Disable the possibility of private keys (only watchonlys are possible in this mode).

####Result:
    {
    "name" : <wallet_name>, (string) The wallet name if created successfully. If the wallet was created using a full path, the wallet_name will be the full path.
    "warning" : <warning>, (string) Warning message if wallet was not loaded cleanly.
    }

####Examples:
    > qtum-cli createwallet "testwallet"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "createwallet", "params": ["testwallet"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test example: 

    createwallet “ancd”
####test result:
    {
    "name": "ancd",
    "warning": ""
    }
    
##41. createpsbt [{"txid":"id","vout":n},...] 

[{"address":amount},{"data":"hex"},...] ( locktime ) ( replaceable )

Creates a transaction in the Partially Signed Transaction format.
Implements the Creator role.

####Arguments:
    1. "inputs" (array, required) A json array of json objects
    [
    {
    "txid":"id", (string, required) The transaction id
    "vout":n, (numeric, required) The output number
    "sequence":n (numeric, optional) The sequence number
    }
    ,...
    ]
    2. "outputs" (array, required) a json array with outputs (key-value pairs), where none of the keys are duplicated.
    That is, each address can only appear once and there can only be one 'data' object.
    [
    {
    "address": x.xxx, (obj, optional) A key-value pair. The key (string) is the qtum address, the value (float or string) is the amount in QTUM
    },
    {
    "data": "hex" (obj, optional) A key-value pair. The key must be "data", the value is hex encoded data
    }
    ,... More key-value pairs of the above form. For compatibility reasons, a dictionary, which holds the key-value pairs directly, is also
    accepted as second parameter.
    ]
    3. locktime (numeric, optional, default=0) Raw locktime. Non-0 value also locktime-activates inputs
    4. replaceable (boolean, optional, default=false) Marks this transaction as BIP125 replaceable.
    Allows this transaction to be replaced by a transaction with higher fees. If provided, it is an error if explicit sequence numbers are incompatible.

####Result:

    "psbt" (string) The resulting raw transaction (base64-encoded string)

####Examples:

    > qtum-cli createpsbt "[{\"txid\":\"myid\",\"vout\":0}]" "[{\"data\":\"00010203\"}]"

##42. createcontract
createcontract "bytecode" (gaslimit gasprice "senderaddress" broadcast)
Create a contract with bytcode.

####Arguments:
    1. "bytecode" (string, required) contract bytcode.
    2. gasLimit (numeric or string, optional) gasLimit, default: 2500000, max: 40000000
    3. gasPrice (numeric or string, optional) gasPrice QTUM price per gas unit, default: 0.0000004, min:0.0000004
    4. "senderaddress" (string, optional) The quantum address that will be used to create the contract.
    5. "broadcast" (bool, optional, default=true) Whether to broadcast the transaction or not.
    6. "changeToSender" (bool, optional, default=true) Return the change to the sender.

####Result:
    [
    {
    "txid" : (string) The transaction id.
    "sender" : (string) QTUM address of the sender.
    "hash160" : (string) ripemd-160 hash of the sender.
    "address" : (string) expected contract address.
    }
    ]

####Examples:
    > qtum-cli createcontract "60606040525b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff02191690836c010000000000000000000000009081020402179055506103786001600050819055505b600c80605b6000396000f360606040526008565b600256"
    > qtum-cli createcontract "60606040525b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff02191690836c010000000000000000000000009081020402179055506103786001600050819055505b600c80605b6000396000f360606040526008565b600256" 6000000 0.0000004 "QM72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd" true
    
##43. createrawtransaction
createrawtransaction [{"txid":"id","vout":n},...] [{"address":amount},{"data":"hex"},...] ( locktime ) ( replaceable )

Create a transaction spending the given inputs and creating new outputs.
Outputs can be addresses or data.
Returns hex-encoded raw transaction.
Note that the transaction's inputs are not signed, and
it is not stored in the wallet or transmitted to the network.

####Arguments:
    1. "inputs" (array, required) A json array of json objects
    [
    {
    "txid":"id", (string, required) The transaction id
    "vout":n, (numeric, required) The output number
    "sequence":n (numeric, optional) The sequence number
    }
    ,...
    ]
    2. "outputs" (array, required) a json array with outputs (key-value pairs), where none of the keys are duplicated.
    That is, each address can only appear once and there can only be one 'data' object.
    [
    {
    "address": x.xxx, (obj, optional) A key-value pair. The key (string) is the qtum address, the value (float or string) is the amount in QTUM
    },
    {
    "data": "hex" (obj, optional) A key-value pair. The key must be "data", the value is hex encoded data
    },
    {
    "contract":{
    "contractAddress":"address", (string, required) Valid contract address (valid hash160 hex data)
    "data":"hex", (string, required) Hex data to add in the call output
    "amount":x.xxx, (numeric, optional) Value in QTUM to send with the call, should be a valid amount, default 0
    "gasLimit":x, (numeric, optional) The gas limit for the transaction
    "gasPrice":x.xxx (numeric, optional) The gas price for the transaction
    }
    {
    ,... More key-value pairs of the above form. For compatibility reasons, a dictionary, which holds the key-value pairs directly, is also
    accepted as second parameter.
    ]
    3. locktime (numeric, optional, default=0) Raw locktime. Non-0 value also locktime-activates inputs
    4. replaceable (boolean, optional, default=false) Marks this transaction as BIP125 replaceable.
    Allows this transaction to be replaced by a transaction with higher fees. If provided, it is an error if explicit sequence numbers are incompatible.

####Result:

    "transaction" (string) hex string of the transaction

####Examples:
    > qtum-cli createrawtransaction "[{\"txid\":\"myid\",\"vout\":0}]" "[{\"address\":0.01}]"
    > qtum-cli createrawtransaction "[{\"txid\":\"myid\",\"vout\":0}]" "[{\"data\":\"00010203\"}]"
    > qtum-cli createrawtransaction "[{\"txid\":\"myid\",\"vout\":0}]" "[{\"contract\":{\"contractAddress\":\"mycontract\",\"data\":\"00\", \"gasLimit\":250000, \"gasPrice\":0.00000040, \"amount\":0}}]"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "createrawtransaction", "params": ["[{\"txid\":\"myid\",\"vout\":0}]", "[{\"address\":0.01}]"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "createrawtransaction", "params": ["[{\"txid\":\"myid\",\"vout\":0}]", "[{\"data\":\"00010203\"}]"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "createrawtransaction", "params": ["[{\"txid\":\"myid\",\"vout\":0}]", "[{\"contract\":{\"contractAddress\":\"mycontract\",\"data\":\"00\", \"gasLimit\":250000, \"gasPrice\":0.00000040, \"amount\":0}}]"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##44. decodepsbt "psbt"

Return a JSON object representing the serialized, base64-encoded partially signed Qtum transaction.

####Arguments:

    1. "psbt" (string, required) The PSBT base64 string

####Result:
    {
    "tx" : { (json object) The decoded network-serialized unsigned transaction.
    ... The layout is the same as the output of decoderawtransaction.
    },
    "unknown" : { (json object) The unknown global fields
    "key" : "value" (key-value pair) An unknown key-value pair
    ...
    },
    "inputs" : [ (array of json objects)
    {
    "non_witness_utxo" : { (json object, optional) Decoded network transaction for non-witness UTXOs
    ...
    },
    "witness_utxo" : { (json object, optional) Transaction output for witness UTXOs
    "amount" : x.xxx, (numeric) The value in QTUM
    "scriptPubKey" : { (json object)
    "asm" : "asm", (string) The asm
    "hex" : "hex", (string) The hex
    "type" : "pubkeyhash", (string) The type, eg 'pubkeyhash'
    "address" : "address" (string) Qtum address if there is one
    }
    },
    "partial_signatures" : { (json object, optional)
    "pubkey" : "signature", (string) The public key and signature that corresponds to it.
    ,...
    }
    "sighash" : "type", (string, optional) The sighash type to be used
    "redeem_script" : { (json object, optional)
    "asm" : "asm", (string) The asm
    "hex" : "hex", (string) The hex
    "type" : "pubkeyhash", (string) The type, eg 'pubkeyhash'
    }
    "witness_script" : { (json object, optional)
    "asm" : "asm", (string) The asm
    "hex" : "hex", (string) The hex
    "type" : "pubkeyhash", (string) The type, eg 'pubkeyhash'
    }
    "bip32_derivs" : { (json object, optional)
    "pubkey" : { (json object, optional) The public key with the derivation path as the value.
    "master_fingerprint" : "fingerprint" (string) The fingerprint of the master key
    "path" : "path", (string) The path
    }
    ,...
    }
    "final_scriptsig" : { (json object, optional)
    "asm" : "asm", (string) The asm
    "hex" : "hex", (string) The hex
    }
    "final_scriptwitness": ["hex", ...] (array of string) hex-encoded witness data (if any)
    "unknown" : { (json object) The unknown global fields
    "key" : "value" (key-value pair) An unknown key-value pair
    ...
    },
    }
    ,...
    ]
    "outputs" : [ (array of json objects)
    {
    "redeem_script" : { (json object, optional)
    "asm" : "asm", (string) The asm
    "hex" : "hex", (string) The hex
    "type" : "pubkeyhash", (string) The type, eg 'pubkeyhash'
    }
    "witness_script" : { (json object, optional)
    "asm" : "asm", (string) The asm
    "hex" : "hex", (string) The hex
    "type" : "pubkeyhash", (string) The type, eg 'pubkeyhash'
    }
    "bip32_derivs" : [ (array of json objects, optional)
    {
    "pubkey" : "pubkey", (string) The public key this path corresponds to
    "master_fingerprint" : "fingerprint" (string) The fingerprint of the master key
    "path" : "path", (string) The path
    }
    }
    ,...
    ],
    "unknown" : { (json object) The unknown global fields
    "key" : "value" (key-value pair) An unknown key-value pair
    ...
    },
    }
    ,...
    ]
    "fee" : fee (numeric, optional) The transaction fee paid if all UTXOs slots in the PSBT have been filled.
    }

####Examples:

    > qtum-cli decodepsbt "psbt"

##45. decoderawtransaction "hexstring" ( iswitness )
Return a JSON object representing the serialized, hex-encoded transaction.

####Arguments:
    1. "hexstring" (string, required) The transaction hex string
    2. iswitness (boolean, optional) Whether the transaction hex is a serialized witness transaction
    If iswitness is not present, heuristic tests will be used in decoding

####Result:
    {
    "txid" : "id", (string) The transaction id
    "hash" : "id", (string) The transaction hash (differs from txid for witness transactions)
    "size" : n, (numeric) The transaction size
    "vsize" : n, (numeric) The virtual transaction size (differs from size for witness transactions)
    "weight" : n, (numeric) The transaction's weight (between vsize*4 - 3 and vsize*4)
    "version" : n, (numeric) The version
    "locktime" : ttt, (numeric) The lock time
    "vin" : [ (array of json objects)
    {
    "txid": "id", (string) The transaction id
    "vout": n, (numeric) The output number
    "scriptSig": { (json object) The script
    "asm": "asm", (string) asm
    "hex": "hex" (string) hex
    },
    "txinwitness": ["hex", ...] (array of string) hex-encoded witness data (if any)
    "sequence": n (numeric) The script sequence number
    }
    ,...
    ],
    "vout" : [ (array of json objects)
    {
    "value" : x.xxx, (numeric) The value in QTUM
    "n" : n, (numeric) index
    "scriptPubKey" : { (json object)
    "asm" : "asm", (string) the asm
    "hex" : "hex", (string) the hex
    "reqSigs" : n, (numeric) The required sigs
    "type" : "pubkeyhash", (string) The type, eg 'pubkeyhash'
    "addresses" : [ (json array of string)
    "Q2tvKAXCxZjSmdNbao16dKXC8tRWfcF5oc" (string) qtum address
    ,...
    ]
    }
    }
    ,...
    ],
    }

####Examples:
    > qtum-cli decoderawtransaction "hexstring"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "decoderawtransaction", "params": ["hexstring"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test example: 

    ./qtum-cli decoderawtransaction "0100000001697f98c004bbb7d184119a31b2b8c96683fa8c7ca0d7755c6196888fb6ba046e010000006a473044022077de54191ae91b502d03a83bd1d580a8b4467fc3d205c7bce169f13e6abc1c91022064f759845c960b04ddf9dd5a635da5f8b990fc934fced06747d52f2658603735012103c426034f05b3b66700f151991e6e45f2d63545a73b36c0a8e8c4200c53f7fd2cfeffffff02a0f53813000000001976a914e85324d4402d9758122c5498de80d1cfcc6330cb88aca09f6636000000001976a914093f4c533deb449f6bd0a427bddb0f02c297101388ac5dad0700"
####test result:
    {
    "txid": "27f5f35b447e219d0b3a3ac55f9dc6aacf2d4145fbd930207ef5b7710c47d883",
    "hash": "27f5f35b447e219d0b3a3ac55f9dc6aacf2d4145fbd930207ef5b7710c47d883",
    "version": 1,
    "size": 225,
    "vsize": 225,
    "weight": 900,
    "locktime": 503133,
    "vin": [
    {
    "txid": "6e04bab68f8896615c75d7a07c8cfa8366c9b8b2319a1184d1b7bb04c0987f69",
    "vout": 1,
    "scriptSig": {
    "asm": "3044022077de54191ae91b502d03a83bd1d580a8b4467fc3d205c7bce169f13e6abc1c91022064f759845c960b04ddf9dd5a635da5f8b990fc934fced06747d52f2658603735[ALL] 03c426034f05b3b66700f151991e6e45f2d63545a73b36c0a8e8c4200c53f7fd2c",
    "hex": "473044022077de54191ae91b502d03a83bd1d580a8b4467fc3d205c7bce169f13e6abc1c91022064f759845c960b04ddf9dd5a635da5f8b990fc934fced06747d52f2658603735012103c426034f05b3b66700f151991e6e45f2d63545a73b36c0a8e8c4200c53f7fd2c"
    },
    "sequence": 4294967294
    }
    ],
    "vout": [
    {
    "value": 3.22500000,
    "n": 0,
    "scriptPubKey": {
    "asm": "OP_DUP OP_HASH160 e85324d4402d9758122c5498de80d1cfcc6330cb OP_EQUALVERIFY OP_CHECKSIG",
    "hex": "76a914e85324d4402d9758122c5498de80d1cfcc6330cb88ac",
    "reqSigs": 1,
    "type": "pubkeyhash",
    "addresses": [
    "qejoXUP4LJ7V56UbdtrPir6GkbjmxFnzLJ"
    ]
    }
    },
    {
    "value": 9.12695200,
    "n": 1,
    "scriptPubKey": {
    "asm": "OP_DUP OP_HASH160 093f4c533deb449f6bd0a427bddb0f02c2971013 OP_EQUALVERIFY OP_CHECKSIG",
    "hex": "76a914093f4c533deb449f6bd0a427bddb0f02c297101388ac",
    "reqSigs": 1,
    "type": "pubkeyhash",
    "addresses": [
    "qJQH4nmrEnujUkUaVceJUYYGQgEQTfqkxi"
    ]
    }
    }
    ]
    }

##46.decodescript "hexstring"

Decode a hex-encoded script.

####Arguments:

    1. "hexstring" (string) the hex encoded script

####Result:
    {
    "asm":"asm", (string) Script public key
    "hex":"hex", (string) hex encoded public key
    "type":"type", (string) The output type
    "reqSigs": n, (numeric) The required signatures
    "addresses": [ (json array of string)
    "address" (string) qtum address
    ,...
    ],
    "p2sh","address" (string) address of P2SH script wrapping this redeem script (not returned if the script is already a P2SH).
    }
    
####Examples:
    > qtum-cli decodescript "hexstring"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "decodescript", "params": ["hexstring"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/


##47. ehco
echo|echojson "message" ...

Simply echo back the input arguments. This command is for testing.

The difference between echo and echojson is that echojson has argument conversion enabled in the client-side table inqtum-cli and the GUI. There is no server-side difference.
##48. encryptwallet "passphrase"
Encrypts the wallet with 'passphrase'. This is for first time encryption.
After this, any calls that interact with private keys such as sending or signing
will require the passphrase to be set prior the making these calls.
Use the walletpassphrase call for this, and then walletlock call.
If the wallet is already encrypted, use the wallet passphrase change call.
Note that this will shutdown the server.

####Arguments:
1. "passphrase" (string) The pass phrase to encrypt the wallet with. It must be at least 1 character, but should be long.

####Examples:

Encrypt your wallet

    > qtum-cli encryptwallet "my pass phrase"

Now set the passphrase to use the wallet, such as for signing or sending qtum

    > qtum-cli walletpassphrase "my pass phrase"

Now we can do something like sign

    > qtum-cli signmessage "address" "test message"

Now lock the wallet again by removing the passphrase

    > qtum-cli walletlock

As a json rpc call
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "encryptwallet", "params": ["my pass phrase"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/
    
##49.estimaterawfee conf_target (threshold)

WARNING: This interface is unstable and may disappear or change!

WARNING: This is an advanced API call that is tightly coupled to the specific
implementation of fee estimation. The parameters it can be called with
and the results it returns will change if the internal implementation changes.

Estimates the approximate fee per kilobyte needed for a transaction to begin
confirmation within conf_target blocks if possible. Uses virtual transaction size as
defined in BIP 141 (witness data is discounted).

####Arguments:
    1. conf_target (numeric) Confirmation target in blocks (1 - 1008)
    2. threshold (numeric, optional) The proportion of transactions in a given feerate range that must have been
    confirmed within conf_target in order to consider those feerates as high enough and proceed to check
    lower buckets. Default: 0.95

####Result:
    {
    "short" : { (json object, optional) estimate for short time horizon
    "feerate" : x.x, (numeric, optional) estimate fee rate in QTUM/kB
    "decay" : x.x, (numeric) exponential decay (per block) for historical moving average of confirmation data
    "scale" : x, (numeric) The resolution of confirmation targets at this time horizon
    "pass" : { (json object, optional) information about the lowest range of feerates to succeed in meeting the threshold
    "startrange" : x.x, (numeric) start of feerate range
    "endrange" : x.x, (numeric) end of feerate range
    "withintarget" : x.x, (numeric) number of txs over history horizon in the feerate range that were confirmed within target
    "totalconfirmed" : x.x, (numeric) number of txs over history horizon in the feerate range that were confirmed at any point
    "inmempool" : x.x, (numeric) current number of txs in mempool in the feerate range unconfirmed for at least target blocks
    "leftmempool" : x.x, (numeric) number of txs over history horizon in the feerate range that left mempool unconfirmed after target
    },
    "fail" : { ... }, (json object, optional) information about the highest range of feerates to fail to meet the threshold
    "errors": [ str... ] (json array of strings, optional) Errors encountered during processing
    },
    "medium" : { ... }, (json object, optional) estimate for medium time horizon
    "long" : { ... } (json object) estimate for long time horizon
    }
    
    Results are returned for any horizon which tracks blocks up to the confirmation target.

####Example:

    > qtum-cli estimaterawfee 6 0.9


##50. estimatesmartfee conf_target ("estimate_mode")

Estimates the approximate fee per kilobyte needed for a transaction to begin
confirmation within conf_target blocks if possible and return the number of blocks
for which the estimate is valid. Uses virtual transaction size as defined
in BIP 141 (witness data is discounted).

####Arguments:
    1. conf_target (numeric) Confirmation target in blocks (1 - 1008)
    2. "estimate_mode" (string, optional, default=CONSERVATIVE) The fee estimate mode.
    Whether to return a more conservative estimate which also satisfies
    a longer history. A conservative estimate potentially returns a
    higher feerate and is more likely to be sufficient for the desired
    target, but is not as responsive to short term drops in the
    prevailing fee market. Must be one of:
    "UNSET" (defaults to CONSERVATIVE)
    "ECONOMICAL"
    "CONSERVATIVE"

####Result:
    {
    "feerate" : x.x, (numeric, optional) estimate fee-per-kilobyte (in QTUM)
    "errors": [ str... ] (json array of strings, optional) Errors encountered during processing
    "blocks" : n (numeric) block number where estimate was found
    }
    
    The request target will be clamped between 2 and the highest target
    fee estimation is able to return based on how long it has been running.
    An error is returned if not enough transactions and blocks
    have been observed to make an estimate for any number of blocks.

####Example:

    > qtum-cli estimatesmartfee 6

##51. fromhexaddress "hexaddress"
Converts a raw hex address to a base58 pubkeyhash address

##Arguments:

    1. "hexaddress" (string, required) The raw hex address

####Result:

    "address" (string) The base58 pubkeyhash address

####Examples:
    > qtum-cli fromhexaddress "hexaddress"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "fromhexaddress", "params": ["hexaddress"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

####test example:

    fromhexaddress "6c89a1a6ca2ae7c00b248bb2832d6f480f27da68"
####test result:

    qTTH1Yr2eKCuDLqfxUyBLCAjmomQ8pyrBt
##52.gethexaddress "address"

Converts a base58 pubkeyhash address to a hex address for use in smart contracts.

####Arguments:

    1. "address" (string, required) The base58 address

####Result:

    "hexaddress" (string) The raw hex pubkeyhash address for use in smart contracts

####Examples:
    > qtum-cli gethexaddress "address"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "gethexaddress", "params": ["address"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/
    
####test example:

    gethexaddress "qTTH1Yr2eKCuDLqfxUyBLCAjmomQ8pyrBt"
####test result:

    6c89a1a6ca2ae7c00b248bb2832d6f480f27da68

##53.finalizepsbt "psbt" ( extract )
Finalize the inputs of a PSBT. If the transaction is fully signed, it will produce a network serialized transaction which can be broadcast with sendrawtransaction. Otherwise a PSBT will be created which has the final_scriptSig and final_scriptWitness fields filled for inputs that are complete. Implements the Finalizer and Extractor roles.

####Arguments:
    1. "psbt" (string) A base64 string of a PSBT
    2. "extract" (boolean, optional, default=true) If true and the transaction is complete,
    extract and return the complete transaction in normal network serialization instead of the PSBT.

####Result:
    {
    "psbt" : "value", (string) The base64-encoded partially signed transaction if not extracted
    "hex" : "value", (string) The hex-encoded network transaction if extracted
    "complete" : true|false, (boolean) If the transaction has a complete set of signatures
    ]
    }

####Examples:

    > qtum-cli finalizepsbt "psbt"

##54. fundrawtransaction "hexstring" ( options iswitness )

Add inputs to a transaction until it has enough in value to meet its out value.
This will not modify existing inputs, and will add at most one change output to the outputs.
No existing outputs will be modified unless "subtractFeeFromOutputs" is specified.
Note that inputs which were signed may need to be resigned after completion since in/outputs have been added.
The inputs added will not be signed, use signrawtransaction for that.
Note that all existing inputs must have their previous output transaction be in the wallet.
Note that all inputs selected must be of standard form and P2SH scripts must be
in the wallet using importaddress or addmultisigaddress (to calculate fees).
You can see whether this is the case by checking the "solvable" field in the listunspent output.
Only pay-to-pubkey, multisig, and P2SH versions thereof are currently supported for watch-only

####Arguments:
    1. "hexstring" (string, required) The hex string of the raw transaction
    2. options (object, optional)
    {
    "changeAddress" (string, optional, default pool address) The qtum address to receive the change
    "changePosition" (numeric, optional, default random) The index of the change output
    "change_type" (string, optional) The output type to use. Only valid if changeAddress is not specified. Options are "legacy", "p2sh-segwit", and "bech32". Default is set by -changetype.
    "includeWatching" (boolean, optional, default false) Also select inputs which are watch only
    "lockUnspents" (boolean, optional, default false) Lock selected unspent outputs
    "feeRate" (numeric, optional, default not set: makes wallet determine the fee) Set a specific fee rate in QTUM/kB
    "subtractFeeFromOutputs" (array, optional) A json array of integers.
    The fee will be equally deducted from the amount of each specified output.
    The outputs are specified by their zero-based index, before any change output is added.
    Those recipients will receive less qtums than you enter in their corresponding amount field.
    If no outputs are specified here, the sender pays the fee.
    [vout_index,...]
    "replaceable" (boolean, optional) Marks this transaction as BIP125 replaceable.
    Allows this transaction to be replaced by a transaction with higher fees
    "conf_target" (numeric, optional) Confirmation target (in blocks)
    "estimate_mode" (string, optional, default=UNSET) The fee estimate mode, must be one of:
    "UNSET"
    "ECONOMICAL"
    "CONSERVATIVE"
    }
    for backward compatibility: passing in a true instead of an object will result in {"includeWatching":true}
    3. iswitness (boolean, optional) Whether the transaction hex is a serialized witness transaction
    If iswitness is not present, heuristic tests will be used in decoding

Result:
{
"hex": "value", (string) The resulting raw transaction (hex-encoded string)
"fee": n, (numeric) Fee in QTUM the resulting transaction pays
"changepos": n (numeric) The position of the added change output, or -1
}

####Examples:

Create a transaction with no inputs

    > qtum-cli createrawtransaction "[]" "{\"myaddress\":0.01}"

Add sufficient unsigned inputs to meet the output value

    > qtum-cli fundrawtransaction "rawtransactionhex"

Sign the transaction

    > qtum-cli signrawtransaction "fundedtransactionhex"

Send the transaction

    > qtum-cli sendrawtransaction "signedtransactionhex"


##55.generate nblocks ( maxtries )

Mine up to nblocks blocks immediately (before the RPC call returns) to an address in the wallet.

####Arguments:
    1. nblocks (numeric, required) How many blocks are generated immediately.
    2. maxtries (numeric, optional) How many iterations to try (default = 1000000).

####Result:

    [ blockhashes ] (array) hashes of blocks generated

####Examples:

    Generate 11 blocks
    > qtum-cli generate 11

##56.generatetoaddress nblocks address (maxtries)
Mine blocks immediately to a specified address (before the RPC call returns)

####Arguments:
    1. nblocks (numeric, required) How many blocks are generated immediately.
    2. address (string, required) The address to send the newly generated qtum to.
    3. maxtries (numeric, optional) How many iterations to try (default = 1000000).

####Result:

    [ blockhashes ] (array) hashes of blocks generated

####Examples:

Generate 11 blocks to myaddress

    > qtum-cli generatetoaddress 11 "myaddress"

57. getaddednodeinfo ( "node" )
Returns information about the given added node, or all added nodes
(note that onetry addnodes are not listed here)

####Arguments:
1. "node" (string, optional) If provided, return information about this specific node, otherwise all nodes are returned.

####Result:
    [
    {
    "addednode" : "192.168.0.201", (string) The node IP address or name (as provided to addnode)
    "connected" : true|false, (boolean) If connected
    "addresses" : [ (list of objects) Only when connected = true
    {
    "address" : "192.168.0.201:3888", (string) The qtum server IP and port we're connected to
    "connected" : "outbound" (string) connection, inbound or outbound
    }
    ]
    }
    ,...
    ]

####Examples:
    > qtum-cli getaddednodeinfo "192.168.0.201"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getaddednodeinfo", "params": ["192.168.0.201"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##58. getblockstats hash_or_height ( stats )

Compute per block statistics for a given window. All amounts are in satoshis.
It won't work for some heights with pruning.
It won't work without -txindex for utxo_size_inc, *fee or *feerate stats.

####Arguments:
    1. "hash_or_height" (string or numeric, required) The block hash or height of the target block
    2. "stats" (array, optional) Values to plot, by default all values (see result below)
    [
    "height", (string, optional) Selected statistic
    "time", (string, optional) Selected statistic
    ,...
    ]

####Result:
    { (json object)
    "avgfee": xxxxx, (numeric) Average fee in the block
    "avgfeerate": xxxxx, (numeric) Average feerate (in satoshis per virtual byte)
    "avgtxsize": xxxxx, (numeric) Average transaction size
    "blockhash": xxxxx, (string) The block hash (to check for potential reorgs)
    "feerate_percentiles": [ (array of numeric) Feerates at the 10th, 25th, 50th, 75th, and 90th percentile weight unit (in satoshis per virtual byte)
    "10th_percentile_feerate", (numeric) The 10th percentile feerate
    "25th_percentile_feerate", (numeric) The 25th percentile feerate
    "50th_percentile_feerate", (numeric) The 50th percentile feerate
    "75th_percentile_feerate", (numeric) The 75th percentile feerate
    "90th_percentile_feerate", (numeric) The 90th percentile feerate
    ],
    "height": xxxxx, (numeric) The height of the block
    "ins": xxxxx, (numeric) The number of inputs (excluding coinbase)
    "maxfee": xxxxx, (numeric) Maximum fee in the block
    "maxfeerate": xxxxx, (numeric) Maximum feerate (in satoshis per virtual byte)
    "maxtxsize": xxxxx, (numeric) Maximum transaction size
    "medianfee": xxxxx, (numeric) Truncated median fee in the block
    "mediantime": xxxxx, (numeric) The block median time past
    "mediantxsize": xxxxx, (numeric) Truncated median transaction size
    "minfee": xxxxx, (numeric) Minimum fee in the block
    "minfeerate": xxxxx, (numeric) Minimum feerate (in satoshis per virtual byte)
    "mintxsize": xxxxx, (numeric) Minimum transaction size
    "outs": xxxxx, (numeric) The number of outputs
    "subsidy": xxxxx, (numeric) The block subsidy
    "swtotal_size": xxxxx, (numeric) Total size of all segwit transactions
    "swtotal_weight": xxxxx, (numeric) Total weight of all segwit transactions divided by segwit scale factor (4)
    "swtxs": xxxxx, (numeric) The number of segwit transactions
    "time": xxxxx, (numeric) The block time
    "total_out": xxxxx, (numeric) Total amount in all outputs (excluding coinbase and thus reward [ie subsidy + totalfee])
    "total_size": xxxxx, (numeric) Total size of all non-coinbase transactions
    "total_weight": xxxxx, (numeric) Total weight of all non-coinbase transactions divided by segwit scale factor (4)
    "totalfee": xxxxx, (numeric) The fee total
    "txs": xxxxx, (numeric) The number of transactions (excluding coinbase)
    "utxo_increase": xxxxx, (numeric) The increase/decrease in the number of unspent outputs
    "utxo_size_inc": xxxxx, (numeric) The increase/decrease in size for the utxo index (not discounting op_return and similar)
    }
    
####Examples:
    > qtum-cli getblockstats 1000 '["minfeerate","avgfeerate"]'
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblockstats", "params": [1000 '["minfeerate","avgfeerate"]'] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##59.getchaintxstats ( nblocks blockhash )

Compute statistics about the total number and rate of transactions in the chain.

####Arguments:
    1. nblocks (numeric, optional) Size of the window in number of blocks (default: one month).
    2. "blockhash" (string, optional) The hash of the block that ends the window.

####Result:
    {
    "time": xxxxx, (numeric) The timestamp for the final block in the window in UNIX format.
    "txcount": xxxxx, (numeric) The total number of transactions in the chain up to that point.
    "window_final_block_hash": "...", (string) The hash of the final block in the window.
    "window_block_count": xxxxx, (numeric) Size of the window in number of blocks.
    "window_tx_count": xxxxx, (numeric) The number of transactions in the window. Only returned if "window_block_count" is > 0.
    "window_interval": xxxxx, (numeric) The elapsed time in the window in seconds. Only returned if "window_block_count" is > 0.
    "txrate": x.xx, (numeric) The average rate of transactions per second in the window. Only returned if "window_interval" is > 0.
    }

####Examples:
    > qtum-cli getchaintxstats
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getchaintxstats", "params": [2016] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/
    

####test result:
    {
    "time": 1561602624,
    "txcount": 866823,
    "window_final_block_hash": "ea6b26303facc34404da3174962a5c1d8d00369a3ff27aa50238ba8f24170280",
    "window_block_count": 20250,
    "window_tx_count": 41012,
    "window_interval": 2655920,
    "txrate": 0.01544173017259556
    }


##60.getnettotals

Returns information about network traffic, including bytes in, bytes out,
and current time.

####Result:
    {
    "totalbytesrecv": n, (numeric) Total bytes received
    "totalbytessent": n, (numeric) Total bytes sent
    "timemillis": t, (numeric) Current UNIX time in milliseconds
    "uploadtarget":
    {
    "timeframe": n, (numeric) Length of the measuring timeframe in seconds
    "target": n, (numeric) Target in bytes
    "target_reached": true|false, (boolean) True if target is reached
    "serve_historical_blocks": true|false, (boolean) True if serving historical blocks
    "bytes_left_in_cycle": t, (numeric) Bytes left in current time cycle
    "time_left_in_cycle": t (numeric) Seconds left in current time cycle
    }
    }

##Examples:
    > qtum-cli getnettotals
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getnettotals", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##61. getnetworkhashps ( nblocks height )

Returns the estimated network hashes per second based on the last n blocks (for PoW only).
Pass in [blocks] to override # of blocks, -1 specifies since last difficulty change.
Pass in [height] to estimate the network speed at the time when a certain block was found.

####Arguments:
    1. nblocks (numeric, optional, default=120) The number of blocks, or -1 for blocks since last difficulty change.
    2. height (numeric, optional, default=-1) To estimate at the time of the given height.

####Result:

    x (numeric) Hashes per second estimated

####Examples:
    > qtum-cli getnetworkhashps
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getnetworkhashps", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/
    

####test result:


    3.514351301619956e-008

##62.getstakinginfo
Returns an object containing staking-related information.

####test result:
    {
    "enabled": true,
    "staking": false,
    "errors": "",
    "currentblocktx": 0,
    "pooledtx": 0,
    "difficulty": 4.656542373906925e-010,
    "search-interval": 0,
    "weight": 0,
    "netstakeweight": 0,
    "expectedtime": 0
    }


####Example:

    ./qtum-cli getstakinginfo

####Result:
    {
    "enabled": true or false,(bool),if HD key is enabled, the value if true,else is false.
    "staking": true or false,(bool),if wallet staking token or not.
    "errors": "XXX", (string), any network and blockchain errors
    "currentblocktx": n,(numberic),the last block transaction
    "pooledtx": n, (numberic), The size of the mempool
    "difficulty": n,(number),the current proof-of-work difficulty
    "search-interval": n,
    "weight": n,(number), the total staking token number
    "netstakeweight": 0, 
    "expectedtime": 0
    }


##63. getsubsidy [nTarget]
Returns subsidy value for the specified value of target.

####test result:

    2000000000000

##64. getwalletinfo

Returns an object containing various wallet state info.

####Result:
    {
    "walletname": xxxxx, (string) the wallet name
    "walletversion": xxxxx, (numeric) the wallet version
    "balance": xxxxxxx, (numeric) the total confirmed balance of the wallet in QTUM
    "stake": xxxxxxx, (numeric) the total stake balance of the wallet in QTUM
    "unconfirmed_balance": xxx, (numeric) the total unconfirmed balance of the wallet in QTUM
    "immature_balance": xxxxxx, (numeric) the total immature balance of the wallet in QTUM
    "txcount": xxxxxxx, (numeric) the total number of transactions in the wallet
    "keypoololdest": xxxxxx, (numeric) the timestamp (seconds since Unix epoch) of the oldest pre-generated key in the key pool
    "keypoolsize": xxxx, (numeric) how many new keys are pre-generated (only counts external keys)
    "keypoolsize_hd_internal": xxxx, (numeric) how many new keys are pre-generated for internal use (used for change outputs, only appears if the wallet is using this feature, otherwise external keys are used)
    "unlocked_until": ttt, (numeric) the timestamp in seconds since epoch (midnight Jan 1 1970 GMT) that the wallet is unlocked for transfers, or 0 if the wallet is locked
    "paytxfee": x.xxxx, (numeric) the transaction fee configuration, set in QTUM/kB
    "hdseedid": "<hash160>" (string, optional) the Hash160 of the HD seed (only present when HD is enabled)
    "hdmasterkeyid": "<hash160>" (string, optional) alias for hdseedid retained for backwards-compatibility. Will be removed in V0.18.
    "private_keys_enabled": true|false (boolean) false if privatekeys are disabled for this wallet (enforced watch-only wallet)
    }
    
    Examples:
    > qtum-cli getwalletinfo
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getwalletinfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/


##65.getzmqnotifications

Returns information about the active ZeroMQ notifications.

####Result:
    [
    { (json object)
    "type": "pubhashtx", (string) Type of notification
    "address": "..." (string) Address of the publisher
    },
    ...
    ]

####Examples:
    > qtum-cli getzmqnotifications
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getzmqnotifications", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##66.getunconfirmedbalance
Returns the server's total unconfirmed balance


##67.gettxoutsetinfo

Returns statistics about the unspent transaction output set.
Note this call may take some time.

####Result:
    {
    "height":n, (numeric) The current block height (index)
    "bestblock": "hex", (string) The hash of the block at the tip of the chain
    "transactions": n, (numeric) The number of transactions with unspent outputs
    "txouts": n, (numeric) The number of unspent transaction outputs
    "bogosize": n, (numeric) A meaningless metric for UTXO set size
    "hash_serialized_2": "hash", (string) The serialized hash
    "disk_size": n, (numeric) The estimated size of the chainstate on disk
    "total_amount": x.xxx (numeric) The total amount
    }

####Examples:
    > qtum-cli gettxoutsetinfo
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "gettxoutsetinfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/


##68. gettxoutproof ["txid",...] ( blockhash )
Returns a hex-encoded proof that "txid" was included in a block.

NOTE: By default this function only works sometimes. This is when there is an
unspent output in the utxo for this transaction. To make it always work,
you need to maintain a transaction index, using the -txindex command line option or
specify the block in which the transaction is included manually (by blockhash).

####Arguments:
    1. "txids" (string,required) A json array of txids to filter
    [
    "txid" (string) A transaction hash
    ,...
    ]
    2. "blockhash" (string, optional) If specified, looks for txid in the block with this hash

####Result:

    "data" (string) A string that is a serialized, hex-encoded data for the proof.

####test example: 
    ./qtum-cli gettxoutproof [\"5caa24c8c78f441a5c37dff602cdacc27b4530b03c09569f62dc3cd20e674918\"]

####test result:

    0000002081d3145a457b724b725171603a991b8d8186f0506c65722e436a6a33d039690ed689a1e4bdea746f8a3c47d6856765282fb5f7f20c9c43cc9e0170b6ba1214076010135d8683001b0000000052ef386ec7ae80719e408c3ea4193583bd0665fffd633d5e10b19e26375ac9b6386faa7484bfd98fc4789fd584229d5c20f72f772a8b3024ea94d1563e84e964b7e989413b1f509a5c14f24dadcf6da7e4f9e8559e5f6ff185cbc978fa1693fc0100000046304402205c0fbeff48e49b24848fba7428ea1c821ef4942135d60f51f6a4260e76941ac5022012a051fc518ec6b684a49eaf75631cdfa5574b170ccab6a0612da44585eab5600300000002fc77727661996828f410e89871d981a1c37f951d35d4ed196745d348cc74ca611849670ed23cdc629f56093cb030457bc2accd02f6df375c1a448fc7c824aa5c010d


##69. gettxout "txid" n ( include_mempool )

Returns details about an unspent transaction output.

####Arguments:
    1. "txid" (string, required) The transaction id
    2. "n" (numeric, required) vout number
    3. "include_mempool" (boolean, optional) Whether to include the mempool. Default: true. Note that an unspent output that is spent in the mempool won't appear.

####Result:
    {
    "bestblock": "hash", (string) The hash of the block at the tip of the chain
    "confirmations" : n, (numeric) The number of confirmations
    "value" : x.xxx, (numeric) The transaction value in QTUM
    "scriptPubKey" : { (json object)
    "asm" : "code", (string)
    "hex" : "hex", (string)
    "reqSigs" : n, (numeric) Number of required signatures
    "type" : "pubkeyhash", (string) The type, eg pubkeyhash
    "addresses" : [ (array of string) array of qtum addresses
    "address" (string) qtum address
    ,...
    ]
    },
    "coinbase" : true|false (boolean) Coinbase or not
    }

####Examples:

Get unspent transactions

    > qtum-cli listunspent

View the details

    > qtum-cli gettxout "txid" 1

As a json rpc call

    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "gettxout", "params": ["txid", 1] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##70.gettransactionreceipt "hash"
requires -logevents to be enabled
####Argument:

    1. "hash" (string, required) The contract transaction hash

####Result:


    "blockHash": "XXX" (String), 32 bytes, the blockhash included this tx.
    "blockNumber": n (Number), the blocknumber included this transaction.
    "transaction": "XXX": (String), 32 bytes，the hash of this transaction.
    "transactionIndex": n(numberic), the index in the block for this tx.
    "from": "XXX"(String), 20 bytes，the sender address of this tx.
    "to": "XXXX"(String), 20 bytes，the receiver address of this tx. if this  address is created by a contract,return null. 
    "cumulativeGasUsed": n (numberic), The total amount of gas used after execution of the current transaction
    "gasUsed": n (numberic), The gas cost alone to execute the current transaction。
    "contractAddress": "XXX"(String), 20 bytes，the created contract address.
    if this tx is created by the contract, return the contract address. else return null.
    "logs": 


####test result:
    [
    {
    "blockHash": "1e34edb316f9c442d1db71ad5bd5376650387c6deb275c63c459b6624880180b",
    "blockNumber": 196529,
    "transactionHash": "acccfb57aaeb94127560f4762d5372af3dcb4faddf9de3b2ce6bde0fdd1d57d5",
    "transactionIndex": 2,
    "from": "83c2436854450b0895d4c1d965720ef5e6a125be",
    "to": "74045ec0dc26ec1861473828bc140ebc4c1f3eff",
    "cumulativeGasUsed": 23619,
    "gasUsed": 23619,
    "contractAddress": "74045ec0dc26ec1861473828bc140ebc4c1f3eff",
    "excepted": "None",
    "log": [
    ]
    }
    ]



####71.importaddress "address" ( "label" rescan p2sh )

Adds an address or script (in hex) that can be watched as if it were in your wallet but cannot be used to spend. Requires a new wallet backup.

####Arguments:
    1. "address" (string, required) The Bitcoin address (or hex-encoded script)
    2. "label" (string, optional, default="") An optional label
    3. rescan (boolean, optional, default=true) Rescan the wallet for transactions
    4. p2sh (boolean, optional, default=false) Add the P2SH version of the script as well

**Note: This call can take over an hour to complete if rescan is true, during that time, other rpc calls
may report that the imported address exists but related transactions are still missing, leading to temporarily incorrect/bogus balances and unspent outputs until rescan completes.
If you have the full public key, you should call importpubkey instead of this.

Note: If you import a non-standard raw script in hex form, outputs sending to it will be treated
as change, and not show up in many RPCs.**

####Examples:

Import an address with rescan

    > qtum-cli importaddress "myaddress"

Import using a label without rescan

    > qtum-cli importaddress "myaddress" "testing" false

As a JSON-RPC call

    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "importaddress", "params": ["myaddress", "testing", false] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##72. importwallet "filename"

Imports keys from a wallet dump file (see dumpwallet). Requires a new wallet backup to include imported keys.

####Arguments:
1. "filename" (string, required) The wallet file

####Examples:

Dump the wallet

    > qtum-cli dumpwallet "test"

Import the wallet

    > qtum-cli importwallet "test"

Import using the json rpc call

    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "importwallet", "params": ["test"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/


##73. importmulti "requests" ( "options" )

Import addresses/scripts (with private or public keys, redeem script (P2SH)), rescanning all addresses in one-shot-only (rescan can be disabled via options). Requires a new wallet backup.

####Arguments:
    1. requests (array, required) Data to be imported
    [ (array of json objects)
    {
    "scriptPubKey": "<script>" | { "address":"<address>" }, (string / json, required) Type of scriptPubKey (string for script, json for address)
    "timestamp": timestamp | "now" , (integer / string, required) Creation time of the key in seconds since epoch (Jan 1 1970 GMT),
    or the string "now" to substitute the current synced blockchain time. The timestamp of the oldest
    key will determine how far back blockchain rescans need to begin for missing wallet transactions.
    "now" can be specified to bypass scanning, for keys which are known to never have been used, and
    0 can be specified to scan the entire blockchain. Blocks up to 2 hours before the earliest key
    creation time of all keys being imported by the importmulti call will be scanned.
    "redeemscript": "<script>" , (string, optional) Allowed only if the scriptPubKey is a P2SH address or a P2SH scriptPubKey
    "pubkeys": ["<pubKey>", ... ] , (array, optional) Array of strings giving pubkeys that must occur in the output or redeemscript
    "keys": ["<key>", ... ] , (array, optional) Array of strings giving private keys whose corresponding public keys must occur in the output or redeemscript
    "internal": <true> , (boolean, optional, default: false) Stating whether matching outputs should be treated as not incoming payments aka change
    "watchonly": <true> , (boolean, optional, default: false) Stating whether matching outputs should be considered watched even when they're not spendable, only allowed if keys are empty
    "label": <label> , (string, optional, default: '') Label to assign to the address (aka account name, for now), only allowed with internal=false
    }
    ,...
    ]
    2. options (json, optional)
    {
    "rescan": <false>, (boolean, optional, default: true) Stating if should rescan the blockchain after all imports
    }
    
    Note: This call can take over an hour to complete if rescan is true, during that time, other rpc calls
    may report that the imported keys, addresses or scripts exists but related transactions are still missing.

##Examples:
    > qtum-cli importmulti '[{ "scriptPubKey": { "address": "<my address>" }, "timestamp":1455191478 }, { "scriptPubKey": { "address": "<my 2nd address>" }, "label": "example 2", "timestamp": 1455191480 }]'
    > qtum-cli importmulti '[{ "scriptPubKey": { "address": "<my address>" }, "timestamp":1455191478 }]' '{ "rescan": false}'
    
    Response is an array with the same size as the input that has the execution result :
    [{ "success": true } , { "success": false, "error": { "code": -1, "message": "Internal Server Error"} }, ... ]


##74. importprivkey "qtumprivkey" ( "label" ) ( rescan )

Adds a private key (as returned by dumpprivkey) to your wallet. Requires a new wallet backup.
Hint: use importmulti to import more than one private key.

####Arguments:
    1. "qtumprivkey" (string, required) The private key (see dumpprivkey)
    2. "label" (string, optional, default="") An optional label
    3. rescan (boolean, optional, default=true) Rescan the wallet for transactions

**Note: This call can take over an hour to complete if rescan is true, during that time, other rpc calls
may report that the imported key exists but related transactions are still missing, leading to temporarily incorrect/bogus balances and unspent outputs until rescan completes.**

####Examples:

Dump a private key

    > qtum-cli dumpprivkey "myaddress"

Import the private key with rescan

    > qtum-cli importprivkey "mykey"

Import using a label and without rescan

    > qtum-cli importprivkey "mykey" "testing" false

Import using default blank label and without rescan

    > qtum-cli importprivkey "mykey" "" false

As a JSON-RPC call

    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "importprivkey", "params": ["mykey", "testing", false] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##74 importprunedfunds

Imports funds without rescan. Corresponding address or script must previously be included in wallet. Aimed towards pruned wallets. The end-user is responsible to import additional transactions that subsequently spend the imported outputs or rescan after the point in the blockchain the transaction is included.

####Arguments:
    1. "rawtransaction" (string, required) A raw transaction in hex funding an already-existing address in wallet
    2. "txoutproof" (string, required) The hex output from gettxoutproof that contains the transaction

##75. importpubkey "pubkey" ( "label" rescan )

Adds a public key (in hex) that can be watched as if it were in your wallet but cannot be used to spend. Requires a new wallet backup.

####Arguments:
    1. "pubkey" (string, required) The hex-encoded public key
    2. "label" (string, optional, default="") An optional label
    3. rescan (boolean, optional, default=true) Rescan the wallet for transactions
    
    Note: This call can take over an hour to complete if rescan is true, during that time, other rpc calls
    may report that the imported pubkey exists but related transactions are still missing, leading to temporarily incorrect/bogus balances and unspent outputs until rescan completes.

####Examples:

Import a public key with rescan

    > qtum-cli importpubkey "mypubkey"

Import using a label without rescan

    > qtum-cli importpubkey "mypubkey" "testing" false

As a JSON-RPC call

    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "importpubkey", "params": ["mypubkey", "testing", false] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/


##76. keypoolrefill ( newsize )

Fills the keypool.

####Arguments

    1. newsize (numeric, optional, default=100) The new keypool size

####Examples:
    > qtum-cli keypoolrefill
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "keypoolrefill", "params": [] }' -H 'con


####77. listaddressgroupings

Lists groups of addresses which have had their common ownership
made public by common use as inputs or as the resulting change
in past transactions

####Result:
    [
    [
    [
    "address", (string) The qtum address
    amount, (numeric) The amount in QTUM
    "label" (string, optional) The label
    ]
    ,...
    ]
    ,...
    ]

####Examples:
    > qtum-cli listaddressgroupings
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listaddressgroupings", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##78. listcontracts (start maxDisplay)

####Argument:
    1. start (numeric or string, optional) The starting account index, default 1
    2. maxDisplay (numeric or string, optional) Max accounts to list, default 2
    
    2. maxDisplay (numeric or string, optional) Max accounts to list, default 20

##79. listlabels ( "purpose" )

Returns the list of all labels, or labels that are assigned to addresses with a specific purpose.

####Arguments:

    1. "purpose" (string, optional) Address purpose to list labels for ('send','receive'). An empty string is the same as not providing this argument.

####Result:
    [ (json array of string)
    "label", (string) Label name
    ...
    ]

####Examples:

List all labels

    > qtum-cli listlabels

List labels that have receiving addresses

    > qtum-cli listlabels receive

List labels that have sending addresses

    > qtum-cli listlabels send

As json rpc call

    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listlabels", "params": [receive] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##80. listlockunspent

Returns list of temporarily unspendable outputs.
See the lockunspent call to lock and unlock transactions for spending.

####Result:
    [
    {
    "txid" : "transactionid", (string) The transaction id locked
    "vout" : n (numeric) The vout value
    }
    ,...
    ]

####Examples:

List the unspent transactions

    > qtum-cli listunspent

Lock an unspent transaction

    > qtum-cli lockunspent false "[{\"txid\":\"a08e6907dbbd3d809776dbfc5d82e371b764ed838b5655e72f463568df1aadf0\",\"vout\":1}]"

List the locked transactions

    > qtum-cli listlockunspent

Unlock the transaction again

    > qtum-cli lockunspent true "[{\"txid\":\"a08e6907dbbd3d809776dbfc5d82e371b764ed838b5655e72f463568df1aadf0\",\"vout\":1}]"

As a json rpc call

    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listlockunspent", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##listreceivedbyaddress ( minconf include_empty include_watchonly address_filter )

List balances by receiving address.

####Arguments:
    1. minconf (numeric, optional, default=1) The minimum number of confirmations before payments are included.
    2. include_empty (bool, optional, default=false) Whether to include addresses that haven't received any payments.
    3. include_watchonly (bool, optional, default=false) Whether to include watch-only addresses (see 'importaddress').
    4. address_filter (string, optional) If present, only return information on this address.

####Result:
    [
    {
    "involvesWatchonly" : true, (bool) Only returned if imported addresses were involved in transaction
    "address" : "receivingaddress", (string) The receiving address
    "account" : "accountname", (string) DEPRECATED. Backwards compatible alias for label.
    "amount" : x.xxx, (numeric) The total amount in QTUM received by the address
    "confirmations" : n, (numeric) The number of confirmations of the most recent transaction included
    "label" : "label", (string) The label of the receiving address. The default label is "".
    "txids": [
    "txid", (string) The ids of transactions received with the address
    ...
    ]
    }
    ,...
    ]

####Examples:
    > qtum-cli listreceivedbyaddress
    > qtum-cli listreceivedbyaddress 6 true
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listreceivedbyaddress", "params": [6, true, true] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listreceivedbyaddress", "params": [6, true, true, "1M72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##81. listsinceblock ( "blockhash" target_confirmations include_watchonly include_removed )


Get all transactions in blocks since block [blockhash], or all transactions if omitted.
If "blockhash" is no longer a part of the main chain, transactions from the fork point onward are included.
Additionally, if include_removed is set, transactions affecting the wallet which were removed are returned in the "removed" array.

####Arguments:
    1. "blockhash" (string, optional) The block hash to list transactions since
    2. target_confirmations: (numeric, optional, default=1) Return the nth block hash from the main chain. e.g. 1 would mean the best block hash. Note: this is not used as a filter, but only affects [lastblock] in the return value
    3. include_watchonly: (bool, optional, default=false) Include transactions to watch-only addresses (see 'importaddress')
    4. include_removed: (bool, optional, default=true) Show transactions that were removed due to a reorg in the "removed" array
    (not guaranteed to work on pruned nodes)

####Result:
    {
    "transactions": [
    "account":"accountname", (string) DEPRECATED. This field will be removed in V0.18. To see this deprecated field, start qtumd with -deprecatedrpc=accounts. The account name associated with the transaction. Will be "" for the default account.
    "address":"address", (string) The qtum address of the transaction. Not present for move transactions (category = move).
    "category":"send|receive", (string) The transaction category. 'send' has negative amounts, 'receive' has positive amounts.
    "amount": x.xxx, (numeric) The amount in QTUM. This is negative for the 'send' category, and for the 'move' category for moves
    outbound. It is positive for the 'receive' category, and for the 'move' category for inbound funds.
    "vout" : n, (numeric) the vout value
    "fee": x.xxx, (numeric) The amount of the fee in QTUM. This is negative and only available for the 'send' category of transactions.
    "confirmations": n, (numeric) The number of confirmations for the transaction. Available for 'send' and 'receive' category of transactions.
    When it's < 0, it means the transaction conflicted that many blocks ago.
    "blockhash": "hashvalue", (string) The block hash containing the transaction. Available for 'send' and 'receive' category of transactions.
    "blockindex": n, (numeric) The index of the transaction in the block that includes it. Available for 'send' and 'receive' category of transactions.
    "blocktime": xxx, (numeric) The block time in seconds since epoch (1 Jan 1970 GMT).
    "txid": "transactionid", (string) The transaction id. Available for 'send' and 'receive' category of transactions.
    "time": xxx, (numeric) The transaction time in seconds since epoch (Jan 1 1970 GMT).
    "timereceived": xxx, (numeric) The time received in seconds since epoch (Jan 1 1970 GMT). Available for 'send' and 'receive' category of transactions.
    "bip125-replaceable": "yes|no|unknown", (string) Whether this transaction could be replaced due to BIP125 (replace-by-fee);
    may be unknown for unconfirmed transactions not in the mempool
    "abandoned": xxx, (bool) 'true' if the transaction has been abandoned (inputs are respendable). Only available for the 'send' category of transactions.
    "comment": "...", (string) If a comment is associated with the transaction.
    "label" : "label" (string) A comment for the address/transaction, if any
    "to": "...", (string) If a comment to is associated with the transaction.
    ],
    "removed": [
    <structure is the same as "transactions" above, only present if include_removed=true>
    Note: transactions that were re-added in the active chain will appear as-is in this array, and may thus have a positive confirmation count.
    ],
    "lastblock": "lastblockhash" (string) The hash of the block (target_confirmations-1) from the best block on the main chain. This is typically used to feed back into listsinceblock the next time you call it. So you would generally use a target_confirmations of say 6, so you will be continually re-notified of transactions until they've reached 6 confirmations plus any new ones
    }

####Examples:
    > qtum-cli listsinceblock
    > qtum-cli listsinceblock "000000000000000bacf66f7497b7dc45ef753ee9a7d38571037cdb1a57f663ad" 6
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listsinceblock", "params": ["000000000000000bacf66f7497b7dc45ef753ee9a7d38571037cdb1a57f663ad", 6] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##82. listreceivedbylabel ( minconf include_empty include_watchonly)


List received transactions by label.

####Arguments:
    1. minconf (numeric, optional, default=1) The minimum number of confirmations before payments are included.
    2. include_empty (bool, optional, default=false) Whether to include labels that haven't received any payments.
    3. include_watchonly (bool, optional, default=false) Whether to include watch-only addresses (see 'importaddress').

####Result:
    [
    {
    "involvesWatchonly" : true, (bool) Only returned if imported addresses were involved in transaction
    "account" : "accountname", (string) DEPRECATED. Backwards compatible alias for label.
    "amount" : x.xxx, (numeric) The total amount received by addresses with this label
    "confirmations" : n, (numeric) The number of confirmations of the most recent transaction included
    "label" : "label" (string) The label of the receiving address. The default label is "".
    }
    ,...
    ]

####Examples:
    > qtum-cli listreceivedbylabel
    > qtum-cli listreceivedbylabel 6 true
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "listreceivedbylabel", "params": [6, true, true] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##83. ping

Requests that a ping be sent to all other nodes, to measure ping time.
Results provided in getpeerinfo, pingtime and pingwait fields are decimal seconds.
Ping command is handled in queue with all other commands, so it measures processing backlog, not just network ping.

####Examples:
    > qtum-cli ping
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "ping", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##84. preciousblock "blockhash"

Treats a block as if it were received before others with the same work.

A later preciousblock call can override the effect of an earlier one.

The effects of preciousblock are not retained across restarts.

####Arguments:
    1. "blockhash" (string, required) the hash of the block to mark as precious
    
####Result:

####Examples:
    > qtum-cli preciousblock "blockhash"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "preciousblock", "params": ["blockhash"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##85.prioritisetransaction <txid> <dummy value> <fee delta>

Accepts the transaction into mined blocks at a higher (or lower) priority

####Arguments:
    1. "txid" (string, required) The transaction id.
    2. dummy (numeric, optional) API-Compatibility for previous API. Must be zero or null.
    DEPRECATED. For forward compatibility use named arguments and omit this parameter.
    3. fee_delta (numeric, required) The fee value (in satoshis) to add (or subtract, if negative).
    Note, that this value is not a fee rate. It is a value to modify absolute fee of the TX.
    The fee is not actually paid, only the algorithm for selecting transactions into a block
    considers the transaction as it would have paid a higher (or lower) fee.

####Result:

    true (boolean) Returns true

####Examples:
    > qtum-cli prioritisetransaction "txid" 0.0 10000
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "prioritisetransaction", "params": ["txid", 0.0, 10000] }' -H 'content-type: text/plain;' http://127.0.0.1:3889


##86. pruneblockchain
 prune the spend tx to reduce the size of the block
####Arguments:
    1. "height" (numeric, required) The block height to prune up to. May be set to a discrete height, or a unix timestamp
    to prune blocks whose block time is at least 2 hours older than the provided timestamp.

####Result:

    n (numeric) Height of the last block pruned.

####Examples:
    > qtum-cli pruneblockchain 1000
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "pruneblockchain", "params": [1000] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##87. removeprunedfunds "txid"

Deletes the specified transaction from the wallet. Meant for use with pruned wallets and as a companion to importprunedfunds. This will affect wallet balances.

####Arguments:

    1. "txid" (string, required) The hex-encoded id of the transaction you are deleting

####Examples:


    > qtum-cli removeprunedfunds "a8d0c0184dde994a09ec054286f1ce581bebf46446a512166eae7628734ea0a5"

As a JSON-RPC call
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "removeprunedfunds", "params": ["a8d0c0184dde994a09ec054286f1ce581bebf46446a512166eae7628734ea0a5"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##88.rescanblockchain ("start_height") ("stop_height")

Rescan the local blockchain for wallet related transactions.

####Arguments:
    1. "start_height" (numeric, optional) block height where the rescan should start
    2. "stop_height" (numeric, optional) the last block height that should be scanned
    
####Result:
    {
    "start_height" (numeric) The block height where the rescan has started. If omitted, rescan started from the genesis block.
    "stop_height" (numeric) The height of the last rescanned block. If omitted, rescan stopped at the chain tip.
    }

####Examples:
    > qtum-cli rescanblockchain 100000 120000
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "rescanblockchain", "params": [100000, 120000] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##89. resendwallettransactions
Immediately re-broadcast unconfirmed wallet transactions to all peers.
Intended only for testing; the wallet code periodically re-broadcasts
automatically.
Returns an RPC error if -walletbroadcast is set to false.
Returns array of transaction ids that were re-broadcast.

##90. reservebalance [<reserve> [amount]]
<reserve> is true or false to turn balance reserve on or off.
<amount> is a real and rounded to cent.
Set reserve amount not participating in network protection.
If no parameters provided current setting is printed.

##91.testmempoolaccept ["rawtxs"] ( allowhighfees )

Returns if raw transaction (serialized, hex-encoded) would be accepted by mempool.

This checks if the transaction violates the consensus or policy rules.

See sendrawtransaction call.

####Arguments:
    1. ["rawtxs"] (array, required) An array of hex strings of raw transactions.
    Length must be one for now.
    2. allowhighfees (boolean, optional, default=false) Allow high fees

####Result:
    [ (array) The result of the mempool acceptance test for each raw transaction in the input array.
    Length is exactly one for now.
    {
    "txid" (string) The transaction hash in hex
    "allowed" (boolean) If the mempool allows this tx to be inserted
    "reject-reason" (string) Rejection string (only present when 'allowed' is false)
    }
    ]

####Examples:

Create a transaction
    > qtum-cli createrawtransaction "[{\"txid\" : \"mytxid\",\"vout\":0}]" "{\"myaddress\":0.01}"
    Sign the transaction, and get back the hex
    > qtum-cli signrawtransaction "myhex"

Test acceptance of the transaction (signed hex)

    > qtum-cli testmempoolaccept ["signedhex"]

As a json rpc call

    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "testmempoolaccept", "params": [["signedhex"]] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##92. unloadwallet ( "wallet_name" )
Unloads the wallet referenced by the request endpoint otherwise unloads the wallet specified in the argument.
Specifying the wallet name on a wallet endpoint is invalid.
####Arguments:

    1. "wallet_name" (string, optional) The name of the wallet to unload.

####Examples:
    > qtum-cli unloadwallet wallet_name
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "unloadwallet", "params": [wallet_name] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/
    
##93. uptime

Returns the total uptime of the server.

####Result:
ttt (numeric) The number of seconds that the server has been running

####Examples:
    > qtum-cli uptime
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "uptime", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/
    

##94. validateaddress "address"
Return information about the given qtum address.
DEPRECATION WARNING: Parts of this command have been deprecated and moved to getaddressinfo. Clients must
transition to using getaddressinfo to access this information before upgrading to v0.18. The following deprecated
fields have moved to getaddressinfo and will only be shown here with -deprecatedrpc=validateaddress: ismine, iswatchonly,
script, hex, pubkeys, sigsrequired, pubkey, addresses, embedded, iscompressed, account, timestamp, hdkeypath, kdmasterkeyid.

####Arguments:

    1. "address" (string, required) The qtum address to validate

####Result:
    {
    "isvalid" : true|false, (boolean) If the address is valid or not. If not, this is the only property returned.
    "address" : "address", (string) The qtum address validated
    "scriptPubKey" : "hex", (string) The hex encoded scriptPubKey generated by the address
    "isscript" : true|false, (boolean) If the key is a script
    "iswitness" : true|false, (boolean) If the address is a witness address
    "witness_version" : version (numeric, optional) The version number of the witness program
    "witness_program" : "hex" (string, optional) The hex value of the witness program
    }

####Examples:
    > qtum-cli validateaddress "QPSSGeFHDnKNxiEyFrD1wcEaHr9hrQDDWc"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "validateaddress", "params": ["QPSSGeFHDnKNxiEyFrD1wcEaHr9hrQDDWc"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##95. verifychain ( checklevel nblocks )

Verifies blockchain database.

####Arguments:
    1. checklevel (numeric, optional, 0-4, default=3) How thorough the block verification is.
    2. nblocks (numeric, optional, default=6, 0=all) The number of blocks to check.

##Result:

    true|false (boolean) Verified or not

####Examples:
    > qtum-cli verifychain
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "verifychain", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##96. verifymessage "address" "signature" "message"

Verify a signed message

####Arguments:
    1. "address" (string, required) The qtum address to use for the signature.
    2. "signature" (string, required) The signature provided by the signer in base 64 encoding (see signmessage).
    3. "message" (string, required) The message that was signed.

##Result:
    true|false (boolean) If the signature is verified or not.
    
##Examples:

Unlock the wallet for 30 seconds

    > qtum-cli walletpassphrase "mypassphrase" 30

Create the signature

    > qtum-cli signmessage "QD1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX" "my message"

Verify the signature

    > qtum-cli verifymessage "QD1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX" "signature" "my message"

As json rpc

    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "verifymessage", "params": ["QD1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX", "signature", "my message"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##97. verifytxoutproof "proof"

Verifies that a proof points to a transaction in a block, returning the transaction it commits to
and throwing an RPC error if the block is not in our best chain

####Arguments:

    1. "proof" (string, required) The hex-encoded proof generated by gettxoutproof

####Result:

    ["txid"] (array, strings) The txid(s) which the proof commits to, or empty array if the proof can not be validated.
####test example

    verifytxoutproof "0000002081d3145a457b724b725171603a991b8d8186f0506c65722e436a6a33d039690ed689a1e4bdea746f8a3c47d6856765282fb5f7f20c9c43cc9e0170b6ba1214076010135d8683001b0000000052ef386ec7ae80719e408c3ea4193583bd0665fffd633d5e10b19e26375ac9b6386faa7484bfd98fc4789fd584229d5c20f72f772a8b3024ea94d1563e84e964b7e989413b1f509a5c14f24dadcf6da7e4f9e8559e5f6ff185cbc978fa1693fc0100000046304402205c0fbeff48e49b24848fba7428ea1c821ef4942135d60f51f6a4260e76941ac5022012a051fc518ec6b684a49eaf75631cdfa5574b170ccab6a0612da44585eab5600300000002fc77727661996828f410e89871d981a1c37f951d35d4ed196745d348cc74ca611849670ed23cdc629f56093cb030457bc2accd02f6df375c1a448fc7c824aa5c010d"


####test result:
    [
    "5caa24c8c78f441a5c37dff602cdacc27b4530b03c09569f62dc3cd20e674918"
    ]

##98. savemempool

Dumps the mempool to disk. It will fail until the previous dump is fully loaded.

####Examples:
    > qtum-cli savemempool
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "savemempool", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/
    
##99. scantxoutset <action> ( <scanobjects> )

EXPERIMENTAL warning: this call may be removed or changed in future releases.

Scans the unspent transaction output set for entries that match certain output descriptors.
Examples of output descriptors are:
addr(<address>) Outputs whose scriptPubKey corresponds to the specified address (does not include P2PK)
raw(<hex script>) Outputs whose scriptPubKey equals the specified hex scripts
combo(<pubkey>) P2PK, P2PKH, P2WPKH, and P2SH-P2WPKH outputs for the given pubkey
pkh(<pubkey>) P2PKH outputs for the given pubkey
sh(multi(<n>,<pubkey>,<pubkey>,...)) P2SH-multisig outputs for the given threshold and pubkeys

In the above, <pubkey> either refers to a fixed public key in hexadecimal notation, or to an xpub/xprv optionally followed by one
or more path elements separated by "/", and optionally ending in "/*" (unhardened), or "/*'" or "/*h" (hardened) to specify all
unhardened or hardened child keys.
In the latter case, a range needs to be specified by below if different from 1000.
For more information on output descriptors, see the documentation in the doc/descriptors.md file.

####Arguments:
    1. "action" (string, required) The action to execute
    "start" for starting a scan
    "abort" for aborting the current scan (returns true when abort was successful)
    "status" for progress report (in %) of the current scan
    2. "scanobjects" (array, required) Array of scan objects
    [ Every scan object is either a string descriptor or an object:
    "descriptor", (string, optional) An output descriptor
    { (object, optional) An object with output descriptor and metadata
    "desc": "descriptor", (string, required) An output descriptor
    "range": n, (numeric, optional) Up to what child index HD chains should be explored (default: 1000)
    },
    ...
    ]

####Result:
    {
    "unspents": [
    {
    "txid" : "transactionid", (string) The transaction id
    "vout": n, (numeric) the vout value
    "scriptPubKey" : "script", (string) the script key
    "amount" : x.xxx, (numeric) The total amount in QTUM of the unspent output
    "height" : n, (numeric) Height of the unspent transaction output
    }
    ,...],
    "total_amount" : x.xxx, (numeric) The total amount of all found unspent outputs in QTUM
    ]
##100. sendmany "" {"address":amount,...} ( minconf "comment" ["address",...] replaceable conf_target "estimate_mode")

Send multiple times. Amounts are double-precision floating point numbers.
Note that the "fromaccount" argument has been removed in V0.17. To use this RPC with a "fromaccount" argument, restart
qtumd with -deprecatedrpc=accounts

Requires wallet passphrase to be set with walletpassphrase call.

####Arguments:
    1. "dummy" (string, required) Must be set to "" for backwards compatibility.
    2. "amounts" (string, required) A json object with addresses and amounts
    {
    "address":amount (numeric or string) The qtum address is the key, the numeric amount (can be string) in QTUM is the value
    ,...
    }
    3. minconf (numeric, optional, default=1) Only use the balance confirmed at least this many times.
    4. "comment" (string, optional) A comment
    5. subtractfeefrom (array, optional) A json array with addresses.
    The fee will be equally deducted from the amount of each selected address.
    Those recipients will receive less qtums than you enter in their corresponding amount field.
    If no addresses are specified here, the sender pays the fee.
    [
    "address" (string) Subtract fee from this address
    ,...
    ]
    6. replaceable (boolean, optional) Allow this transaction to be replaced by a transaction with higher fees via BIP 125
    7. conf_target (numeric, optional) Confirmation target (in blocks)
    8. "estimate_mode" (string, optional, default=UNSET) The fee estimate mode, must be one of:
    "UNSET"
    "ECONOMICAL"
    "CONSERVATIVE"

####Result:
    "txid" (string) The transaction id for the send. Only 1 transaction is created regardless of
    the number of addresses.

####Examples:

Send two amounts to two different addresses:

    > qtum-cli sendmany "" "{\"QD1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX\":0.01,\"Q353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz\":0.02}"

Send two amounts to two different addresses setting the confirmation and comment:

    > qtum-cli sendmany "" "{\"QD1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX\":0.01,\"Q353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz\":0.02}" 6 "testing"

Send two amounts to two different addresses, subtract fee from amount:

    > qtum-cli sendmany "" "{\"QD1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX\":0.01,\"Q353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz\":0.02}" 1 "" "[\"QD1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX\",\"Q353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz\"]"

As a json rpc call

    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "sendmany", "params": ["", {"QD1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX":0.01,"Q353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz":0.02}, 6, "testing"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/


##101.sendmanywithdupes "fromaccount" {"address":amount,...} ( minconf "comment" ["address",...] )

Send multiple times. Amounts are double-precision floating point numbers. Supports duplicate addresses
Requires wallet passphrase to be set with walletpassphrase call.

####Arguments:
    1. "fromaccount" (string, required) DEPRECATED. The account to send the funds from. Should be "" for the default account
    2. "amounts" (string, required) A json object with addresses and amounts
    {
    "address":amount (numeric or string) The qtum address is the key, the numeric amount (can be string) in QTUM is the value
    ,...
    }
    3. minconf (numeric, optional, default=1) Only use the balance confirmed at least this many times.
    4. "comment" (string, optional) A comment
    5. subtractfeefrom (array, optional) A json array with addresses.
    The fee will be equally deducted from the amount of each selected address.
    Those recipients will receive less qtums than you enter in their corresponding amount field.
    If no addresses are specified here, the sender pays the fee.
    [
    "address" (string) Subtract fee from this address
    ,...
    ]

####Result:
    "txid" (string) The transaction id for the send. Only 1 transaction is created regardless of
    the number of addresses.
    
####Examples:

Send two amounts to two different addresses:

    > qtum-cli sendmanywithdupes "" "{\"QD1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX\":0.01,\"Q353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz\":0.02}"

Send two amounts to two different addresses setting the confirmation and comment:

    > qtum-cli sendmanywithdupes "" "{\"QD1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX\":0.01,\"Q353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz\":0.02}" 6 "testing"

Send two amounts to two different addresses, subtract fee from amount:
    > qtum-cli sendmanywithdupes "" "{\"QD1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX\":0.01,\"Q353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz\":0.02}" 1 "" "[\"QD1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX\",\"Q353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz\"]"
    
As a json rpc call

    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "sendmanywithdupes", "params": ["", "{\"QD1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX\":0.01,\"Q353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz\":0.02}", 6, "testing"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##102. sendrawtransaction "hexstring" ( allowhighfees )

Submits raw transaction (serialized, hex-encoded) to local node and network.

Also see createrawtransaction and signrawtransaction calls.

####Arguments:
    1. "hexstring" (string, required) The hex string of the raw transaction)
    2. allowhighfees (boolean, optional, default=false) Allow high fees

####Result:

    "hex" (string) The transaction hash in hex

####Examples:

Create a transaction
    > qtum-cli createrawtransaction "[{\"txid\" : \"mytxid\",\"vout\":0}]" "{\"myaddress\":0.01}"
    Sign the transaction, and get back the hex
    > qtum-cli signrawtransaction "myhex"

Send the transaction (signed hex)

    > qtum-cli sendrawtransaction "signedhex"

As a json rpc call

    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "sendrawtransaction", "params": ["signedhex"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/


##102.sendtoaddress "address" amount ( "comment" "comment_to" subtractfeefromamount replaceable conf_target "estimate_mode")

Send an amount to a given address.

Requires wallet passphrase to be set with walletpassphrase call.
####Arguments:
    1. "address" (string, required) The qtum address to send to.
    2. "amount" (numeric or string, required) The amount in QTUM to send. eg 0.1
    3. "comment" (string, optional) A comment used to store what the transaction is for.
    This is not part of the transaction, just kept in your wallet.
    4. "comment_to" (string, optional) A comment to store the name of the person or organization
    to which you're sending the transaction. This is not part of the
    transaction, just kept in your wallet.
    5. subtractfeefromamount (boolean, optional, default=false) The fee will be deducted from the amount being sent.
    The recipient will receive less qtums than you enter in the amount field.
    6. replaceable (boolean, optional) Allow this transaction to be replaced by a transaction with higher fees via BIP 125
    7. conf_target (numeric, optional) Confirmation target (in blocks)
    8. "estimate_mode" (string, optional, default=UNSET) The fee estimate mode, must be one of:
    "UNSET"
    "ECONOMICAL"
    "CONSERVATIVE"
    9. "senderaddress" (string, optional) The quantum address that will be used to send money from.
    10."changeToSender" (bool, optional, default=false) Return the change to the sender.

####Result:

    "txid" (string) The transaction id.

####Examples:
    > qtum-cli sendtoaddress "QM72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd" 0.1
    > qtum-cli sendtoaddress "QM72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd" 0.1 "donation" "seans outpost"
    > qtum-cli sendtoaddress "QM72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd" 0.1 "" "" true
    > qtum-cli sendtoaddress "QM72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd", 0.1, "donation", "seans outpost", false, null, null, "", "QX1GkJdye9WoUnrE2v6ZQhQ72EUVDtGXQX", true
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "sendtoaddress", "params": ["QM72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd", 0.1, "donation", "seans outpost"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "sendtoaddress", "params": ["QM72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd", 0.1, "donation", "seans outpost", false, null, null, "", "QX1GkJdye9WoUnrE2v6ZQhQ72EUVDtGXQX", true] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##103. sendtocontract "contractaddress" "data" (amount gaslimit gasprice senderaddress broadcast)
Send funds and data to a contract.

Requires wallet passphrase to be set with walletpassphrase call.
####Arguments:
    1. "contractaddress" (string, required) The contract address that will receive the funds and data.
    2. "datahex" (string, required) data to send.
    3. "amount" (numeric or string, optional) The amount in QTUM to send. eg 0.1, default: 0
    4. gasLimit (numeric or string, optional) gasLimit, default: 250000, max: 40000000
    5. gasPrice (numeric or string, optional) gasPrice Qtum price per gas unit, default: 0.0000004, min:0.0000004
    6. "senderaddress" (string, optional) The quantum address that will be used as sender.
    7. "broadcast" (bool, optional, default=true) Whether to broadcast the transaction or not.
    8. "changeToSender" (bool, optional, default=true) Return the change to the sender.

####Result:
    [
    {
    "txid" : (string) The transaction id.
    "sender" : (string) QTUM address of the sender.
    "hash160" : (string) ripemd-160 hash of the sender.
    }
    ]

####Examples:
    > qtum-cli sendtocontract "c6ca2697719d00446d4ea51f6fac8fd1e9310214" "54f6127f"
    > qtum-cli sendtocontract "c6ca2697719d00446d4ea51f6fac8fd1e9310214" "54f6127f" 12.0015 6000000 0.0000004 "QM72Sfpbz1BPpXFHz9m3CdqATR44Jvaydd"
##104. setmocktime timestamp

Set the local time to given timestamp (-regtest only)

####Arguments:
    1. timestamp (integer, required) Unix seconds-since-epoch timestamp
    Pass 0 to go back to using the system time.

##105.sethdseed ( "newkeypool" "seed" )

Set or generate a new HD wallet seed. Non-HD wallets will not be upgraded to being a HD wallet. Wallets that are already HD will have a new HD seed set so that new keys added to the keypool will be derived from this new seed.

Note that you will need to MAKE A NEW BACKUP of your wallet after setting the HD wallet seed.

Requires wallet passphrase to be set with walletpassphrase call.
####Arguments:
    1. "newkeypool" (boolean, optional, default=true) Whether to flush old unused addresses, including change addresses, from the keypool and regenerate it.
    If true, the next address from getnewaddress and change address from getrawchangeaddress will be from this new seed.
    If false, addresses (including change addresses if the wallet already had HD Chain Split enabled) from the existing
    keypool will be used until it has been depleted.
    2. "seed" (string, optional) The WIF private key to use as the new HD seed; if not provided a random seed will be used.
    The seed value can be retrieved using the dumpwallet command. It is the private key marked hdseed=1
    
####Examples:
    > qtum-cli sethdseed
    > qtum-cli sethdseed false
    > qtum-cli sethdseed true "wifkey"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "sethdseed", "params": [true, "wifkey"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/

##106. submitblock "hexdata" ( "dummy" )

Attempts to submit new block to network.
See https://en.bitcoin.it/wiki/BIP_0022 for full specification.

####Arguments
    1. "hexdata" (string, required) the hex-encoded block data to submit
    2. "dummy" (optional) dummy value, for compatibility with BIP22. This value is ignored.
    
####Result:


####Examples:
    > qtum-cli submitblock "mydata"
    > curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "submitblock", "params": ["mydata"] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/




