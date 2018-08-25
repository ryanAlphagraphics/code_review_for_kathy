GGC Smart Contract Code Suggetions
====================




Constant functions
------------
source code::

    function totalSupply() public constant returns (uint);

The function is declared as constant. Currently, for functions the constant modifier is a synonym for view (which is the preferred option). Consider using view for funcitons and constant for state variables.

`View and constant functions in Solidity <https://github.com/ethereum/solidity/issues/992>`_


Using the approve function of the ERC-20 standard
------------
source code::

    function approve(address _spender, uint _value) public returns (bool success)

Only use the approve function of the ERC-20 standard to change allowed amount to 0 or from 0 (wait till transaction is mined and approved).

`Ethereum: Approve <https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md#approve>`_


Costly loop
------------
source code::

    for (i = 0; i < ggePoolTransTrue.length; i++) {
                if(ggePoolTransTrue[i].time <= time){
                    outputGGETrans.push(ggePoolTransTrue[i]);
                }else{
                    ggePoolTransFalse.push(ggePoolTransTrue[i]);
                }
            }

Ethereum is a very resource-constrained environment. Prices per computational step are orders of magnitude higher than with centralized providers. Moreover, Ethereum miners impose a limit on the total number of gas consumed in a block. If ggePoolTransTrue.length is large enough, the function exceeds the block gas limit, and transactions calling it will never be confirmed.This becomes a security issue, if an external actor influences ggePoolTransTrue.length. E.g., if array enumerates all registered addresses, an adversary can register many addresses, causing the problem described above.

`Solidity: Gas Limit and Loops <https://solidity.readthedocs.io/en/develop/security-considerations.html#gas-limit-and-loops>`_


No payable fallback function
------------

Several contracts do not have payable fallback. All attempts to transfer or send ether to these contracts will be reverted.Please implement payable fallback, if the contract should accept payments via transfer or send.

`Solidity: Fallback Function <https://solidity.readthedocs.io/en/develop/contracts.html#fallback-function>`_



Compiler version not fixed
------------
source code::

    pragma solidity ^0.4.24; // not that good: compiles w 0.4.124 and above
    pragma solidity 0.4.24; // good : compiles w 0.4.124 only


Solidity source files indicate the versions of the compiler they can be compiled with.

It is recommended to follow the latter example, as future compiler versions may handle certain language constructions in a way the developer did not foresee.



Reentrancy
------------
source code::

    ApproveAndCallFallBack(_spender).receiveApproval(msg.sender, _value, this, data);


Any interaction from a contract (A) with another contract (B) and any transfer of Ether hands over control to that contract (B). This makes it possible for B to call back into A before this interaction is completed.

This pattern is experimental and can report false issues. This pattern might also be triggered when

* accessing struct's field
* using enum's element

