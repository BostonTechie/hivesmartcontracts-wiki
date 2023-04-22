
When running the application, a JSON RPC server is made available to query the sidechain and its database on port 5000.

 ## 1. The "blockchain" endpoint (http://localhost:5000/blockchain)
Available methods:

### getLatestBlockInfo(): get the latest block of the sidechain

Command:
```
{
    "jsonrpc": "2.0",
    "method": "getLatestBlockInfo",
    "id": 1
}
```

Result:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "blockNumber": 2,
        "previousHash": "2f0467273993608f2fb08311ae6c1f8a94577427f395331051f92c5def94cda6",
        "timestamp": "2018-06-28T17:26:54",
        "transactions": [
            {
                "refBlockNumber": 23723607,
                "transactionId": "b42fed149252a83ddda03d1be7f02080ebb67492",
                "sender": "harpagon",
                "contract": "users_contract",
                "action": "addUser",
                "payload": "{ \"username\": \"AwesomeUsername\" }",
                "hash": "a90155eab610384d027fbc1ce706c24acb5a1151d91483e76e39e61f59ed65cb",
                "logs": "{\"events\":[{\"event\":\"newUserCreated\",\"data\":{\"id\":\"harpagon\",\"username\":\"AwesomeUsername\",\"verified\":false,\"meta\":{\"revision\":0,\"created\":1530230516595,\"version\":0},\"_id\":1}}]}"
            }
        ],
        "hash": "48bd97fbc0b98a764d8f8d87e4469e21a3ab9f1c3eaa5d5d46d342b419f1b2f4",
        "merkleRoot": "1bd248954fa35533b2c9a637fad97f04406564f45366a6e4a7eb12d4f24f5b85"
    }
}
```

### getBlockInfo(blockNumber): get the block with the specified block number of the sidechain

Command:
```
{
    "jsonrpc": "2.0",
    "method": "getBlockInfo",
    "params": {
        "blockNumber": BLOCK_NUMBER
    },
    "id": 1
}
```

Result:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "blockNumber": 1,
        "previousHash": "643fdd62e4c8e99049db8f19d6604f38dcf97aaf17d994741d8fee9925856c70",
        "timestamp": "2018-06-28T17:25:51",
        "transactions": [
            {
                "refBlockNumber": 23723586,
                "transactionId": "73fbb10207533ba32bb50a899ec64322a8b04a79",
                "sender": "harpagon",
                "contract": "contract",
                "action": "deploy",
                "payload": "{\"name\": \"users_contract\", \"code\": \"..."}",
                "hash": "e27d43e9c68fea1c65bc40bc09d5f34509373fc1945fd313d4ca1ffb4f652c2a",
                "logs": "{}"
            }
        ],
        "hash": "2f0467273993608f2fb08311ae6c1f8a94577427f395331051f92c5def94cda6",
        "merkleRoot": "46c0ca94ec8323e3594318adbe0b79ab813837bf854e5a4b6becf670849c5d9d"
    }
}
```

### getBlockRangeInfo(startBlockNumber, count): get a range of multiple consecutive blocks

This API can be used instead of calling getBlockInfo multiple times. startBlockNumber is the first block to retrieve, and count is how many blocks to retrieve, inclusive of the starting block. Note that count cannot be greater than 1000.

Command:
```
{
    "jsonrpc": "2.0",
    "method": "getBlockRangeInfo",
    "params": {
        "startBlockNumber": BLOCK_NUMBER,
	"count": NUMBER_OF_BLOCKS
    },
    "id": 1
}
```

This API is processor intensive, and so is recommended for use on private nodes only. It is disabled by default on most public Engine nodes.

### getTransactionInfo(txid): retrieve the specified transaction info of the sidechain

Command:
```
{
    "jsonrpc": "2.0",
    "method": "getTransactionInfo",
    "params": {
        "txid": TX_ID
    },
    "id": 1
}
```

Result:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
	    "blockNumber": 12,
	    "refSteemBlockNumber": 25797141,
	    "transactionId": "b299d24be543cd50369dbc83cf6ce10e2e8abc9b",
	    "sender": "smmarkettoken",
	    "contract": "smmkt",
	    "action": "updateBeneficiaries",
	    "payload": {
		"beneficiaries": [
		    "harpagon"
		],
		"isSignedWithActiveKey": true
	    },
	    "hash": "ac33d2fcaf2d72477483ab1f2ed4bf3bb077cdb55d5371aa896e8f3fd034e6fd",
	    "logs": "{}"
	}
}
```

### getStatus(): retrieve the status of the SSC node

Command:
```
{
    "jsonrpc": "2.0",
    "method": "getStatus",
    "params": {
    },
    "id": 1
}
```

Result:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "lastBlockNumber": 1,
        "lastBlockRefSteemBlockNumber": 37899120,
        "lastParsedSteemBlockNumber": 37900295,
        "SSCnodeVersion": "0.1.0"
    }
}
```

## 3. The "contracts" endpoint (http://localhost:5000/contracts)
Available methods:

### getContract(contract): get the contract specified from the database

Command:

```
{
    "jsonrpc": "2.0",
    "method": "getContract",
    "params": {
        "name": "CONTRAC_NAME"
    },
    "id": 1
}
```

Result:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "name": "users_contract",
        "owner": "harpagon",
        "code": {
            "code": "...",
            "filename": "vm.js",
            "_compiled": {}
        },
        "tables": [
            "users_contract_users"
        ],
        "_id": 1
    }
}
```

### findOne(query): get the object that matches the query from the table of the specified contract

Command:

```
{
    "jsonrpc": "2.0",
    "method": "findOne",
    "params": {
        "contract": "CONTRACT_NAME",
        "table": "TABLE_NAME",
        "query": {}
    },
    "id": 1
}
```

Result:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "id": "harpagon",
        "username": "AwesomeUsername",
        "verified": false,
        "_id": 1
    }
}
```

### find(query, limit, offset, index, descending): get an array of objects that match the query from the table of the specified contract

Command:

```
{
    "jsonrpc": "2.0",
    "method": "find",
    "params": {
        "contract": "CONTRACT_NAME",
        "table": "TABLE_NAME",
        "query": {},
        "limit": 20,  // default: 1000
	"offset": 20, // default: 0
	"indexes": [], // default: empty, an index is an object { index: string, descending: boolean }
    },
    "id": 1
}
```

Result:

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": [
        {
            "id": "harpagon",
            "username": "AwesomeUsername",
            "verified": false,
            "_id": 1
        }
    ]
}
```
