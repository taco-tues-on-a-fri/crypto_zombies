
string = are used for arbitarylength UTF-8 data
uint = unsigned integer, its value must be non-negative
uint is an alias for uint256 - you can assign uints with less bits - uint8/uint16/uint32

Math is same as other languages 
Solidity also supports Exponential Operators
    uint x = 5 ** 2 // equal to 5^2 = 25

_______
Structs allow you to create more complicated data types that have mutiple properties.

    //example
    struct Person{
        uint age;
        string name;
    }
_______
Arrays

Arrays come in two types:
    
    // Fixed:
        uint[2] fixedArray with 2 elements
        string[5] fixedArray with 5 elements
    //Dynamic:
        uint[]  dynamicArray

    //Array of structs:
        Person[] people; // dynamicArray, can keep adding to it

    //create a new person
        Person satoshi = Person(172, "Satoshi");

    //add that person to the Array
        people.push(satoshi);

    //same but in one line of code
        people.push(Person(16, "Vitalik"));

    **Remember that state variables are stored permanently in the blockchain. 
    Creating a dynamic array of structs can be useful for storing structured data in a contract- 
    kind of like a database. **

    Public Arrays:
        When array is declared as public - Solidity will automatically create a getter.
        Other contracts would then be able to read - not write- to this array.
        Useful for storing public data in contract
            Person[] public people;



_______
Functions

    //example
    function eatHamburgers(string _name, uint _amount){
        //
    }
    //To call:
    eatHamburgers("vitalik, 100);

It's convention to start function parameter variable names with an underscore 
in order to differeiate them from global variables

A)  Public/Private Functions
    
    Functions are public by default
        Make habit of marking functions as private by default then only make public the functions you want exposed to the world.

        It's convention to **start private function names** with an underscore
    
    //example
        uint[] numbers;

        function _addToArray(uint _number) private {
            numbers.push(_number);
        }

B)  Return Values
    The function declaration contans the type of the return value.

    The example doesnt change the state - doesnt change any values or write anything
    In this case - declare it as a view function - meaning only viewing the data but not modifying it

    //example  
        string greeting = "Whats up dog";
        
        function sayHello() public view returns (string) {
            return greeting;
        }

    Solidity also contains pure functions - means youre not accessing any data in the app. 
        This function does not read from the state of the app, its return value depends only on its function parameters.

    //example
        function _multiply(uint a, uint b) private pure returns (uint) {
            return a * b;
        }

_______
Keccak256 & Typecasting

A) Keccak256

    Ethereum has the hash function keccak256 built in - version of SHA3
        Maps input into a random 256-bit hexidecimal numbers
        expects a single parameter of type 'bytes' - means we have to 'pack' any parameters before calling

    //example
        keccak256(abi.encodePacked("aaaab"));
            //6e91ec6b618bb462a4a6ee5aa2cb0e9cf30f7a052bb467b0ba58b8748c00d2e5
        
        keccak256(abi.encodePacked("aaaac"));
            //b1f078126895a1424524de5321b339ab00408010b7cf0e6ed451514981e58aa9

B) Typecasting

    Converts between data types
        a * b returns a uint, but need to store it as a uint8
        By casting it as a uint8 - it works and the compiler won't throw an error

    //example
        uint8 a = 5;
        uint b = 6;
        uint8 c = a * b; 
            // throws an error because a * b returns a uint, not uint8

        uint8 c = a * uint8(b); 
            // we have to typecast b as a uint8 to make it work


_______
Events

    Are a way for your contract to communicate that something happened on the blockchain to your app front-end
    Which can be listening for certain events and take action when they happen.

    //example
        // declare the event
        event IntegersAdded(uint x, uint y, uint result);

        function add(uint _x, uint _y) public {
            uint result = _x + _y;
        // fire an event to let the app know the function was called:
            emit IntegersAdded(_x, _y, result);
            return result;
        }

    Your app front-end could then listen for the event - A javascipt implementation would look like:
    
    //example
        YourContract.IntegersAdded(function(error, result) { 
            // do something with result
        }

_______
Mappings
    A mapping is a key-value store for storing and looking up data.

    //example 
        //For a financial app, storing a uint that holds the user's account balance:
        mapping (address => uint) public accountBalance;

    //example
        // Or could be used to store / lookup usernames based on userId
        mapping (uint => string) userIdToName;


msg.sender
    Global variable which refers to the address of the person (or smart contract) who the current function.
    
    In Solidity, function execution always needs to start with an external caller.
    A contract will sit on the blockchain doing nothing until one of it's functions is called.

    USing msg.sender gives blockchain security. Only way someone could modify data is to have the private key.

    //example - using msg.sender and updating mapping:
    
        mapping (address => uint) favoriteNumber;

        function setMyNumber(uint _myNumber) public {   // Update our `favoriteNumber` mapping to store `_myNumber` under `msg.sender`
        favoriteNumber[msg.sender] = _myNumber;         //  The syntax for storing data in a mapping is just like with arrays
        }

        function whatIsMyNumber() public view returns (uint) {      // Retrieve the value stored in the sender's address
        return favoriteNumber[msg.sender];                          // Will be `0` if the sender hasn't called `setMyNumber` yet
        }

_______
Require
    Will throw and error and exit function if condition is not met

    Solidity does not have native string comparison 
        Use hashes to see if stings are equal
    
    //example
        function sayHiToVitalik(string _name) public returns (string) {
            require(keccak256(_name) == keccak256("Vitalik"));
            return "Hi!";
        }

_______
Inheritance

    Split code logic across multiple contracts to organize the code.

    //example
        contract Doge {
            function catchphrase() public returns (string) {
            return "So Wow CryptoDoge";
            }
        }

        contract BabyDoge is Doge {
            function anotherCatchphrase() public returns (string) {
            return "Such Moon BabyDoge";
            }
        }

    BabyDode inherits from Doge. MEans if you compile and deploy BabyDoge, 
        it will have access to both catchphrase() and anotherCatchphrase()
            and any other public functions we may define on Doge.

    This can be used for logical inheritance - such as with a subclass:
        Cat is an Animal
    But it can also be used to organize your code by grouping similar logic together into different contracts.


B) Import

    //example   
        import "./someothercontract.sol";

        contract newContract is SomeOtherContract {

        }

    If a filed named someothercontract.sol was in the same directory as this contract
        thats what ./ means
            it would get imported by the compiler


_______
Storage vs Memory
    Two places you can store variables - in Storage and in Memory

    Storage refers to variables stored permanently on the blockchain.

    Memory variables are temporary, and are erased beteen external function calls to your contract.

        Like a computer's hard disk vs RAM

    Most of the time you dont need keywords because Solidity handles them by default.

    State variables(variables declared outside of functions) are by default storage and written permanently to the blockchain
    while variable declared inside functions are memory and will disappear when the function call ends. 

    There are times when you need to use the keywords, mainly when dealing with structs and arrays within functions.
    