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





