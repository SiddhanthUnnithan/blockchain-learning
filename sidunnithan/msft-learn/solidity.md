Introduction
- use solidity to write operation logic (supply chain, online marketplace)
- can also specify user actions
- create own blockchain network
What is Solidity
- object-oriented language for writing smart contracts
    - smart contract = program stored inside blockchain, specifying rules and behaviour for transfer of digital assets
- transactions result in functions executed in smart contracts
- smart contracts enable creation of business workflow
- statically typed
- inheritance supported; fn, variables can be used in another
- structs and enums supported
- solidity contracts run on Ethereum Virtual Machine (EVM)
    - sandboxed environment, completely isolated
    - doesn't access anything on network but contract it executes
Language Basics
- contracts include: pragma directive, state variables, fns, events
- pragma = keyword to check whether solidity version matches required
- always use latest version of solidity in contract def
- state vars
    - stored in contract storage permanently
- contract stource files always start with definition contract ContractName
```
    pragma solidity <version_info>;

    contract <definition>{
        <type> <var>; // state variable
    }
```
- all variables must have type and name
- contract = object
    - supports public, private, internal vars
    - public = can be accessed from other contracts
    - internal = can be accessed from current contract (but visible?)
    - private = only visible for source contract
- functions
    - describe single action for one task
    - can be called from other source files
```
    contract <definition>{
        function <func_name>(<params>) <visibility_type> returns (<type>){
            // fn definition...
        }
    }
```
    - support external visibility type
- function modifier
    - used to change behaviour of function
    - works by checking condition before fn executes
```
    contract Marketplace {
        address public seller;

        modifier onlySeller(){
            require(
                msg.sender == seller, "Only seller can put an item up for sale."
            );
            _;
        }
        
        function listItem() public view onlySeller {
            // listitem uses onlyseller modifier
        }
    }
```
    - what is the address type? (another contract?)
    - address = stores 20-byte Ethereum address
    - onlySeller = modifier which requires that only seller can list item
    - _; indicates where fn body gets inserted
    - modifiers
        - pure = describe fns that don't allow modifications or access of state
        - view = fns that don't allow modification of state
        - payable = fns that can receive ether
- events
    - describe actions taken in contract
    - parameters that need to be specified when event is called
    - use keyword "emit" with event name and params
    - when called, captured as transaction in transaction log
        - log = special data structure in blockchain
        - associated with address of contract, forever in blockchain
```
    pragma solidity >0.7.0 <0.8.0;

    contract Marketplace {
        event PurchasedItem(address buyer, uint price);

        function buy() public {
            // ..
            emit PurchasedItem(msg.sender, msg.value);
        }
    }
```
Value Types
    - int; signed (+, -), unsigned (+ only)
        - 256 bits default
    - booleans
    - string literals
        - "String <name>" 
    - address
        - 20-byte value that represents user account
        - can be regular address or address payable
        - payable allows Ether transactions
            - contains members transfer and send
    - enums
Reference Types
    - value types pass independent copy of value
    - ref types provide location to value
    - location
        - memory = where fn arguments are stored, lifetime limited to external fn call
        - storage = where state variables stored, lifetime limited to contract lifetime
        - calldata = where fn arguments stored, required for external functions, lifetime limited to fn call
```
    contract c {
        
        // array x
        uint[] x;

        function g(uint[] storage) internal pure {}
        function h(uint[] memory) public pure {}

        function buy(uint[] memory values) public { 
            // values arg stored in memory (not external fn)
            x = value; // copies array to storage
            uint[] storage y = x; // data location of y is storage
            g(x); // calls g, handing reference to x
            h(x); // calls h, creates temp copy in memory
        }
    }
```
- arrays
    - supports fixed or dynamic size
    - index 0
    - fixed size k, element type T => T[k], dynamic = T[]
    - length() => get length of array
    - push() => append element at the end of the array
    - pop() => remove element from end
    - stack implementation
- structs
    - user-defined
- mapping
    - key-value pairs
    - closest to dictionaries or objects inJS
    - supports complex types
    - 