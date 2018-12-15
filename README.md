# Solidity-Testing
Solidity Testing - Best Practices, Methodologies and errors
This is a space reserved to collect,maintain and share best-practices of solidity smart contract Testing




## Issues and Fixes:

### Issue-1:

While testing solidity smart-contracts using truffle, tests fails.
<ul>
<li>  
Issue Description: ReferenceError: regeneratorRuntime is not defined
  </li>
  <li>
Context: Testing a DNS-Smart contract @  https://github.com/kanthgithub/DNSDapp/blob/master/contracts/DNSDappMaster.sol
  </li>
  
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

