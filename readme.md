## Provable Support versions

This is a project to give it support  to more versions on solidity.

Thanks to this __Ethereum API__, enriching your smart-contracts with external data using __Provable__ is very easy!

In Solidity it is as simple as inheriting the __`usingProvable`__ contract that you'll find in this repository.

This will provide your contract with functions like __`provable_query(...)`__, which make it trivial for you to leverage our oracle technology straight away.

## Original Project
https://github.com/provable-things/ethereum-api/

### Compatible versions of Solidity
* 0.8.X

If you're using the __[Remix IDE](http://remix.ethereum.org)__ it's even easier still - simply import __Provable__ into your contract like so:


```solidity

import "github.com/jhospina/ethereum-provable-support-versions/provableAPI_0.8.sol";

```


## Example
```solidity
// SPDX-License-Identifier: MIT
pragma solidity >= 0.8.0 < 0.9.0;
import "github.com/jhospina/ethereum-provable-support-versions/provableAPI_0.8.sol";

contract ETHUSD is usingProvable {
uint public etherUSD;

    constructor () public {
        etherUSD = 0;
    }

    // __callback is an overridden function provided by oraclize api
    // to handle API responses
    function __callback(bytes32 myid, string memory result) override public {
        // provable_cbAddress() ensures that the data provided comes from
        // provable provider, and not anyone could update this data
        require(msg.sender == provable_cbAddress());

        etherUSD = parseInt(result, 2); // Recover 2 decimals
    }

    function update() public payable {
        // provable_query() is the way to query an API from the smart contract
        // Receives 2 parameters:
        // 1.- Data source
        // 2.- Parser helper with data
        provable_query("URL", "json(https://api.kraken.com/0/public/Ticker?pair=ETHUSD).result.XETHZUSD.c.0");
    }
}
```

To learn more about the Provable technology, please refer to our __[documentation here](https://docs.provable.xyz/)__.
