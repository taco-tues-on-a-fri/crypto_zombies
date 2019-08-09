https://solidity.readthedocs.io/en/latest/introduction-to-smart-contracts.html

Introduction to Smart Contracts

    //example
        pragma solidity >=0.4.0 <0.6.0;

        contract SimpleStorage {
            uint storedData;

            function set(uint x) public {
                storedData = x;
            }

            function get() public view returns (uint) {
                return storedData;
            }
        }

A contract in the sense of Solidy is a: 
    collection of code - its functions 
    data - its state
 Which reside at a specific address on the blockchain


    uint storedData declares a state variable called storedData 
    of type uint (unsignedinteger of 256 bits)

    Think of it as a single slot in a database that can be queried and altered
    by calling functions of the code that manages the database.

    Integers
int / uint: Signed and unsigned integers of various sizes. 
Keywords uint8 to uint256 in steps of 8 (unsigned of 8 up to 256 bits) 
and int8 to int256. 
uint and int are aliases for uint256 and int256, respectively.

To access a state variable, you do not need the prefix 'this.' as is common in other languages

All identifiers (contract names, function names and variable names) are restricted to the ASCII character set.
    It is possible to store UTF-8 encoded data in string variables. 


https://en.wikipedia.org/wiki/ASCII
ASCII (/ˈæskiː/  ASS-kee)
abbreviated from American Standard Code for Information Interchange, 
is a character encoding standard for electronic communication. 

ASCII codes represent text in computers, telecommunications equipment, and other devices.

Most modern character-encoding schemes are based on ASCII, 
although they support many additional characters.

https://en.wikipedia.org/wiki/UTF-8
UTF-8 is a variable width character encoding capable of encoding all 1,112,064 valid code points in Unicode using one to four 8-bit bytes.
The encoding is defined by the Unicode Standard, and was originally designed by Ken Thompson and Rob Pike
The name is derived from Unicode (or Universal Coded Character Set) Transformation Format – 8-bit.
