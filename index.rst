GGC Smart Contract Code Suggetions
====================




Constant Functions
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

`Ethereum#Approve <https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md#approve>`_
