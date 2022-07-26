# AL-Homework
### Create KaseiCoin
```
pragma solidity ^0.5.0;

//  Import the following contracts from the OpenZeppelin library:
//    * `ERC20`
//    * `ERC20Detailed`
//    * `ERC20Mintable`
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/token/ERC20/ERC20.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/token/ERC20/ERC20Detailed.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/token/ERC20/ERC20Mintable.sol";

// Create a constructor for the KaseiCoin contract and have the contract inherit the libraries that you imported from OpenZeppelin.
contract KaseiCoin is ERC20, ERC20Detailed, ERC20Mintable {
    constructor(string memory name, string memory symbol, uint inital_supply) ERC20Detailed (name, symbol, 0) public {
        // dont do anything in constructor
    }
}
```

### Create Crowdsale
```
pragma solidity ^0.5.0;

import "./KaseiCoin.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/crowdsale/Crowdsale.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/crowdsale/emission/MintedCrowdsale.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/crowdsale/validation/CappedCrowdsale.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/crowdsale/validation/TimedCrowdsale.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/crowdsale/distribution/RefundablePostDeliveryCrowdsale.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/crowdsale/distribution/RefundableCrowdsale.sol";

// Have the KaseiCoinCrowdsale contract inherit the following OpenZeppelin:
// * Crowdsale
// * MintedCrowdsale
contract KaseiCoinCrowdsale is Crowdsale, MintedCrowdsale, CappedCrowdsale, TimedCrowdsale, RefundablePostDeliveryCrowdsale { // UPDATE THE CONTRACT SIGNATURE TO ADD INHERITANCE
    
    // Provide parameters for all of the features of your crowdsale, such as the `rate`, `wallet` for fundraising, and `token`.
    constructor(uint rate, address payable wallet, KaseiCoin token, uint goal, uint open, uint close
        // YOUR CODE HERE!
    ) public Crowdsale(rate, wallet, token)              
             CappedCrowdsale(goal) 
             TimedCrowdsale(open, close)
             RefundableCrowdsale(goal)
 {
        // constructor can stay empty
    }
}
```

### Coinsale Deployer
```

contract KaseiCoinCrowdsaleDeployer {
    // Create an `address public` variable called `kasei_token_address`.
    address public kasei_token_address;
    // Create an `address public` variable called `kasei_crowdsale_address`.
    address public kasei_crowdsale_address;


    // Add the constructor.
    constructor(
       string memory name, string memory symbol, address payable wallet, uint goal
    ) public {
        // Create a new instance of the KaseiCoin contract.
        KaseiCoin token = new KaseiCoin(name, symbol, 0);
        
        // Assign the token contract’s address to the `kasei_token_address` variable.
        kasei_token_address = address(token);

        // Create a new instance of the `KaseiCoinCrowdsale` contract
        KaseiCoinCrowdsale crowdsale = new KaseiCoinCrowdsale(1,wallet,token, goal, now, now + 5 minutes);
            
        // Aassign the `KaseiCoinCrowdsale` contract’s address to the `kasei_crowdsale_address` variable.
        kasei_crowdsale_address = address(crowdsale);

        // Set the `KaseiCoinCrowdsale` contract as a minter
        token.addMinter(kasei_crowdsale_address);
        
        // Have the `KaseiCoinCrowdsaleDeployer` renounce its minter role.
        token.renounceMinter();
    }
}
```

### result of crowdsale ( not including optional part)
#### deploy the contract
![](https://github.com/bleachevil/AL-Homework/blob/main/pic/1.png?raw=true)

#### result of deploy
![](https://github.com/bleachevil/AL-Homework/blob/main/pic/2.png?raw=true)

#### add crowdsale
![](https://github.com/bleachevil/AL-Homework/blob/main/pic/3.png?raw=true)

#### add token
![](https://github.com/bleachevil/AL-Homework/blob/main/pic/4.png?raw=true)

#### buy token
![](https://github.com/bleachevil/AL-Homework/blob/main/pic/5.png?raw=true)

#### result of supply
![](https://github.com/bleachevil/AL-Homework/blob/main/pic/6.png?raw=true)

### result of crowdsale ( including optional part)
#### deploy and result the contract
![](https://github.com/bleachevil/AL-Homework/blob/main/pic/11.png?raw=true)

#### add crowdsale
![](https://github.com/bleachevil/AL-Homework/blob/main/pic/13.png?raw=true)

#### add token
![](https://github.com/bleachevil/AL-Homework/blob/main/pic/14.png?raw=true)

#### result of crowdsale
![](https://github.com/bleachevil/AL-Homework/blob/main/pic/15.png?raw=true)

#### buy token
![](https://github.com/bleachevil/AL-Homework/blob/main/pic/16.png?raw=true)

#### token in the metamask
![](https://github.com/bleachevil/AL-Homework/blob/main/pic/17.png?raw=true)
