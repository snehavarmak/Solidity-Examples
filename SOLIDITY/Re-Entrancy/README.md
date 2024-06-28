# Types of Re-entrancy Attacks
- *Single Function Attack*: This is an attack where the malicious contract re-enters the same function from which it had made the external call to itself. The vault contract in the previous section is an example of such an attack. 

- *Cross-function Reentrancy*: This is an attack where the function from which the malicious external call is made is different from the function to which the re-entry is made by the malicious contract, but both the functions use require statements that check the same state variable. Below is an example of a cross function re-entrancy prone code. 

```
mapping (address => uint) private userBalances;
   function transfer(address to, uint amount) {
   if (userBalances[msg.sender] >= amount) {
       userBalances[to] += amount;
       userBalances[msg.sender] -= amount;
       }
   }
   function withdrawBalance() public {
   uint amountToWithdraw = userBalances[msg.sender];
   (bool success, ) = msg.sender.call.value(amountToWithdraw)("");
   // At this point, the caller's code is executed, and can call transfer()
   require(success);
   userBalances[msg.sender] = 0;
   }
```

Resources: [Openzeppelin’s Reentrancy Guard](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/security/ReentrancyGuard.sol)