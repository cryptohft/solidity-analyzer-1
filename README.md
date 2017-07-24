# solidity-analyzer
A dev repository for analyzing and finding bugs smart contracts. 

## Introduction
Given a smart contract, the analyzer finds any public method that directly or indirectly exposes a non-public state variable modification. The prototype uses multiple visitors to extract variables, call-graphs, statements from a given contract.

## Install
Simply install using the package manager

```Shell
$ npm install solidity-analyzer
```

## Demo
Pass the solidity file and the script finds whether there is a path from public method to a sensitive state variable (assuming private). For example, in the following solidity code:

```javascript
contract MyContract {
  uint owner;
  function init(uint i_owner) private {
    owner = i_owner;
  }
  
  function restetOwner() {
  	owner = 0;
  }
}
```
the analyzer returns the following report:
```Shell
Unsafe modification of 'owner' inside 'restetOwner'.
```

Or the analyzer finds the public methods that could indirectly alter any sensitive variable.

```javascript
contract MyContract {
  uint owner;
  function init(uint i_owner) private {
    owner = i_owner;
  }
  
  function restetOwner() {
  	init(0);
  }
}
```
The Warning is:

```shell
Unsafe modification of 'owner' indirectly from 'restetOwner'.
```


