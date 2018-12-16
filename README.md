# Solidity-Testing
<ol>
  <li>
Solidity Testing - Best Practices, Methodologies and errors
  </li>
  <li>
This is a space reserved to collect,maintain and share best-practices of solidity smart contract Testing
  </li>
  </ol>

## Test Practices:

### Test Coverage:

https://medium.com/edgefund/testing-and-code-coverage-of-solidity-smart-contracts-660cb6291701

## Issues and Fixes:

### Issue-1:

While testing solidity smart-contracts using truffle, tests fails.
<ul>
  <li>Issue Description: </li>
  <b>ReferenceError: regeneratorRuntime is not defined</b>
  </li>
  <li>
Context: Testing a DNS-Smart contract @  https://github.com/kanthgithub/DNSDapp/blob/master/contracts/DNSDappMaster.sol
  </li>
 </ul>
 
### Sample Test:
```solidity
const DNSDappMaster = artifacts.require('DNSDappMaster')

contract('tests DNSDapp functions of DNSDappMaster contract', async () => {
  it('should be deployed to the test chain', deployToTestChain)
  it('assert that non-existent name check returns false', testNonExistentDNSName)
})

async function deployToTestChain () {

  const dNSDappMaster = await DNSDappMaster.deployed()

  assert.isNotNull(dNSDappMaster)
}

async function testNonExistentDNSName () {

  const dNSDappMaster = await DNSDappMaster.deployed()

  assert.isNotNull(dNSDappMaster)

  const fakeDNSName = 'alien'

  const actual = await dNSDappMaster.isAValidDNSNameString(fakeDNSName)

  assert.isFalse(actual)
}
```

Solution:

https://github.com/babel/babel/issues/5085

https://stackoverflow.com/questions/33527653/babel-6-regeneratorruntime-is-not-defined/36821986#36821986

```js
In my case installing babel-plugin-transform-runtime solved my issues:

npm install --save-dev babel-plugin-transform-runtime

Then, in .babelrc

{
  "plugins": [
    ["transform-runtime", {
      "polyfill": false,
      "regenerator": true
    }]
  ]
}
```

Credits To: https://github.com/iraklisg

------------------------------------------------------------------------------------------------------------------------------------------
### Events in Solidity Tests:

- Event emitted only in some cases in tests
- How to listen for contract events in JavaScript tests?

```js
I've discovered that not all events are displayed in the truffle output window, although they might have fired correctly with the execution of a contract. I believe this to still be an issue

After spending hours on this today I have come up with a solution to test that specific events are fired.

Here's my example truffle javascript test:

it("should do something that fires SaleMade", function() {
    return myContract
        .stockShelf("beer", "wine", {from: sellerAccount})
        .then(() => myContract.sell("water", "bread", {from: sellerAccount}))
        .then(() => utils.assertEvent(myContract, { event: "SaleMade", logIndex: 1, args: { name: "bread" }}));
}
The above filters the fired events that match the filter object passed to the assertEvent utility function I have in utils.js in the same folder. At the top of my javascript test I have declared:

var utils = require("./utils.js");
A snippet of my utils.js class is as follows:

var _ = require("lodash");
var Promise = require("bluebird");

module.exports = {
    assertEvent: function(contract, filter) {
        return new Promise((resolve, reject) => {
            var event = contract[filter.event]();
            event.watch();
            event.get((error, logs) => {
                var log = _.filter(logs, filter);
                if (log) {
                    resolve(log);
                } else {
                    throw Error("Failed to find filtered event for " + filter.event);
                }
            });
            event.stopWatching();
        });
    }
}
This requires other npm packages that aren't included with truffle by default. By default truffle doesn't include npm packages. I setup npm and installed the required packages like this:

npm init
npm install bluebird --save
npm install lodash --save
EDIT: Using testrpc
```

<p>Discussion and workaround:</p>
<ul><li>
https://ethereum.stackexchange.com/questions/15353/how-to-listen-for-contract-events-in-javascript-tests/21661#21661   </li>
  <li>https://github.com/trufflesuite/truffle/issues/136</li>
</ul>

Credits: https://ethereum.stackexchange.com/users/7132/mkaj

------------------------------------------------------------------------------------------------------------------------------------------
