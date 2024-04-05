---
layout: post
title: "Want to win your first hackathon? Here's how JamWallet did it."
sitemap: true
hide_last_modified: true
---

Every computer science student dreams of that hackathon victory.  I was no different – from my first month of college, the thrill of the challenge consumed me. I practiced relentlessly, went to hackathons, even snagged a few podium finishes. But that elusive first-place win... that always seemed just beyond reach. It was late September 2022, my 5th semester, and our tech fest "Gravitas" and the flagship hackathon of our college “DevJams” organized by Google Developers Students Club, VIT Vellore was the stage. It was a 48 hour hackathon. Web3 was my new obsession, and my half-baked idea for JamWallet felt like the perfect contender. With zero web3 experience, I teamed up with a frontend developer and charged into the unknown, fueled by pure determination and excitement. Skip over to the last paragraph if you want to read my advice to you to win your first hackathon.

Picture this: you've invested in Ethereum, carefully building up your holdings. But the world of crypto is fraught with risk – one lost private key, and your hard-earned assets could vanish into the digital ether. Even worse, if something happens to you, your loved ones might never be able to access your crypto wealth. Traditional inheritance processes often fall short when it comes to the complexities of wallets and keys.

This is where JAMWallet steps in. It's more than just an Ethereum-based wallet. With its own native cryptocurrency (IERC20 contract “link”), JAMCoin, and an inbuilt nominee selection feature, JAMWallet tackles the issue of asset recovery head-on.  Imagine having the peace of mind knowing that even if you lose your private key, your designated nominee can seamlessly recover your assets. 

As we know, we can deploy smart contracts in Ethereum blockchain, we implemented an IERC20 contract and devised an algorithm for nominee function (explained later).

But you must have a question that every blockchain transaction requires the private key and without the private key we are sending all the amount to the nominee, doesn’t this defies the purpose of decentralization? Well, it only defies the main use case of blockchain only when we are sitting as middleman, spectating the transactional flow. But in this case, it’s been automated through smart contracts which is explained in the next paragraph. 

**How the Nominee Selection System Works**

- The JAMWallet essentially allows every user to set up a nominee (back-up) account for himself along with a specified time period which measures the user's inactivity duration.
- In case of a prolonged period of inactivity of the main account, the Smart Contract assumes that the owner of the main account has lost access to his/her private key.
- This is where the account recovery functionality kicks in. Once the user's inactivity period exceed the time-limit set by the user, the user gets the option to transfer all the JAMCoins in his JAMWallet to the chosen nominee's account.
- This can be accomplished easily by calling the burn function from the nominee's account. **NOTE: The burn function can only be called by the chosen account's nominee and is only functional after the set timeperiod expires and burn function doesn’t require private key.**

How the most important functions in our IERC20 Smart Contract works: 

1) Firstly we initialize the global variables. *time_minutes* is a constant like 6 months. *timeLimit* is that future timestamp, till that if no activity is recorded, burn function could be called. After every transaction *lastUpdate* gets set to that time. There are two maps *backup* and *isBackup*. backup

```jsx
uint public lastUpdate;
uint public timeLimit;
uint public time_minutes;

mapping(address => address) public backup;
mapping(address => bool) public isBackup;
```

2) These functions provide functionality for setting up and retrieving backup accounts for main accounts within the smart contract, allowing for a mechanism to transfer control to the backup account under certain conditions (such as exceeding a time limit).

```jsx
function setBackup(address main, address backupAccount, uint _timeLimit) public returns(bool success){
        require(msg.sender == main);
        backup[main] = backupAccount;
        isBackup[backupAccount] = true;
        time_minutes = _timeLimit;
        timeLimit = block.timestamp + _timeLimit * 60; // Setting the timelimit in minutes
        return true;
    }

    function getBackupAccount(address main) public view returns(address backupAccount){
        require(msg.sender == main);
        return backup[main];
    }
```

3) This function facilitates the purchase of tokens (referred to as "Jams") by sending ether to the contract.

- It checks that the sender has a sufficient balance to purchase the specified number of tokens (**`require(msg.sender.balance >= tokens)`**).
- It sends the specified amount of ether to the **`founder`** address, which is presumably the address of the contract creator or an entity responsible for receiving payments.
- It deducts the purchased tokens from the balance of the **`founder`** address and adds them to the balance of the sender.
- It updates the **`timeLimit`** variable by adding the specified number of minutes converted to seconds.
- Finally, it returns **`true`** to indicate the success of the transaction.

```jsx
function buyJams(uint tokens) public payable returns(bool success){
        require(msg.sender.balance >= tokens);
        (bool s, ) = founder.call{ value: tokens } ("");
        balances[founder] -= tokens;
        balances[msg.sender] += tokens;
        timeLimit = block.timestamp + time_minutes * 60; // Setting the timelimit in minutes
        return s;
    }
```

4) This function is designed to burn tokens associated with a main account when triggered by the backup account after a certain time limit has passed.

- It first checks that the caller of the function is the backup account associated with the specified main account (**`require(msg.sender == backup[main])`**).
- It then ensures that the time limit has expired (**`require(timeLimit < block.timestamp, "Time limit not yet expired")`**).
- If both conditions are met, it transfers the balance of tokens from the main account to the backup account.
- It sets the balance of the main account to zero.
- Finally, it returns **`true`** to indicate the success of the operation.

```jsx
function burn(address main) public returns(bool success){
        require(msg.sender == backup[main]);
        require(timeLimit < block.timestamp, "Time limit not yet expired");
        balances[backup[main]] += balances[main];
        balances[main] -= balances[main];
        return success;
    }
```

We tested our smart contract to https://remix.ethereum.org/ which was linked to our local blockchain Ganache. We deployed our contract, copied the ABI Code and  contract address and now we were good to go.

For bridging our web app to blockchain we used metamask as our crypto wallet ( configured in local blockchain, ganache) , ganache as local ethereum blockchain. 

After making an appealing UI and fixing bugs, we were ready to present. 

After an influential pitch, we were the 1st runner ups and won a prize of 45000 Rs. 

After experiencing many hackathons, my advices to you would be: 

- Hackathons aren’t meant to be attended only if you have a staggering idea.
- Go with the motivation to complete the project, don’t think about the end results.
- If you want to learn a new skill, the very first thing you should do is to ideate a project around that skill and register for any hackathon ( even remote) and promise yourself to complete that project within the time limit and make a successful submission. This way, you are triggering your subconscious brain to learn exponentially.
- Last but not the least, hackathons aren’t meant to be won, they are meant to connect you with like minded people, introduce you to new things that could potentially influence your career.

![devjams](/assets/img/devjams.jpeg)