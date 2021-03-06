the following function is equal to ```<address>.code``` in plain solidty

```solidity
function at(address _addr) public view returns (bytes memory code) {
    assembly {
        // retrieve the size of the code, this needs assembly
        let size := extcodesize(_addr)
        // allocate output byte array - this could also be done without assembly
        // by using code = new bytes(size)
        code := mload(0x40)
        // new "memory end" including padding
        mstore(0x40, add(code, and(add(add(size, 0x20), 0x1f), not(0x1f))))
        // store length in memory
        mstore(code, size)
        // actually retrieve the code, this needs assembly
        extcodecopy(_addr, add(code, 0x20), 0, size)
    }
}
```