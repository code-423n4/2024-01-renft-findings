Missing `NATSPEC` comment in `PaymentEscrow.MODULE_PROXY_INSTANTIATION(..)`
```
// * @auditmissing NATSPEC comment 
    // * @audit SUGGESTION: @dev Instantiate this contract as a module. When using a proxy, the kernel address
    ...
    function MODULE_PROXY_INSTANTIATION(
        Kernel kernel_
    ) external onlyByProxy onlyUninitialized {
        kernel = kernel_;
        initialized = true;
    }
```

## SUGGESTION
Add the comment 
```
//  @dev Instantiate this contract as a module. When using a proxy, the kernel address
```
at the top of the comments as modified below
```
//  @dev Instantiate this contract as a module. When using a proxy, the kernel address
//  *      should be set to address(0).
//  *      should be set to address(0). @audit-info
    /**
     * @notice Instantiates this contract as a module via a proxy.
     *
     * @param kernel_ Address of the kernel contract.
     */
    function MODULE_PROXY_INSTANTIATION(
        Kernel kernel_
    ) external onlyByProxy onlyUninitialized {
        kernel = kernel_;
        initialized = true;
    }
```
