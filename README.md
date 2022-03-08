# Epirus-Generated OpenAPI project
This is a generated [Web3j-OpenAPI](https://docs.web3j.io/web3j_openapi) project using the [Epirus-CLI](https://docs.epirus.io/).
For more information, check the [Web3j-OpenAPI](https://docs.web3j.io/web3j_openapi) documentation.

## Manual Modifications for Circles UBI Contracts

1. Clone `git clone https://github.com/CirclesUBI/circles-contracts.git`
2. Copy `https://github.com/OpenZeppelin/openzeppelin-contracts/blob/solc-0.7/contracts/math/SafeMath.sol` to `deps/SafeMath.sol`
3. Copy `https://github.com/OpenZeppelin/openzeppelin-contracts/blob/solc-0.7/contracts/util/Address.sol` to `deps/Address.sol`
4. Change references in contracts to `SafeMath.sol` and `Address.sol` accordingly
5. Start _Ganache_, set mnemonic to *test test ... junk*
   * Set default gas x10 in _Ganache_ (67219750)
6. Create new OpenAPI Web3j Gradle project: `epirus openapi new`

### Changes in `build.gradle`

```
web3j {
    generatedPackageName = 'io.epirus'
    addressBitLength = 160
    excludedContracts = ['SafeMath','Address']
    openapi {
    contextPath = 'Web3App'
    }
}

processResources {
    from('swagger-ui-openapi') { into 'static' }
}

startScripts {
    doLast {
        windowsScript.text = windowsScript.text.replaceAll('set CLASSPATH=.*', 'set CLASSPATH=.;%APP_HOME%/lib/*')
    }
}
```

* Copy all Circles contracts (incl. changes from step 1..3) to `src/main/solidity`
* Create distribution: `gradlew distZip`

### Run OpenAPI Circles Contracts Proxy

* Run `start-local-node.bat` in unzipped `bin` directory