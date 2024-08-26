
# SecureToken ERC-20 Smart Contract

## Overview
The `SecureToken` contract is an implementation of the ERC-20 standard on the Ethereum blockchain. It includes several additional features for enhanced security and functionality, including the ability to pause the contract, burn tokens, transfer ownership, and recover mistakenly sent ERC-20 tokens.

## Contract Features

### Basic Information
- **Token Name**: `SecureToken`
- **Token Symbol**: `STKN`
- **Decimals**: `18`
- **Total Supply**: Defined during contract deployment

### State Variables
- **`name`**: The name of the token.
- **`symbol`**: The symbol representing the token.
- **`decimals`**: The number of decimals the token uses.
- **`totalSupply`**: The total supply of tokens in circulation.
- **`balanceOf`**: A mapping that stores the balance of each address.
- **`allowance`**: A mapping that stores the amount an address is allowed to spend on behalf of another address.
- **`owner`**: The address of the current owner of the contract.
- **`newOwner`**: The address to which ownership will be transferred.
- **`paused`**: A boolean that indicates whether the contract is paused.

### Events
- **`Transfer`**: Emitted when tokens are transferred from one address to another.
- **`Approval`**: Emitted when an owner approves a spender to transfer tokens on their behalf.
- **`Paused`**: Emitted when the contract is paused by the owner.
- **`Unpaused`**: Emitted when the contract is unpaused by the owner.
- **`Burn`**: Emitted when tokens are burned.
- **`OwnershipTransferred`**: Emitted when ownership is transferred to a new owner.

### Modifiers
- **`onlyOwner`**: Restricts the execution of a function to the owner of the contract.
- **`whenNotPaused`**: Restricts the execution of a function to when the contract is not paused.
- **`whenPaused`**: Restricts the execution of a function to when the contract is paused.

### Constructor
The constructor initializes the contract with an initial supply of tokens, assigns all tokens to the creator of the contract, and sets the creator as the owner.

```solidity
constructor(uint256 _initialSupply) {
    owner = msg.sender;
    totalSupply = _initialSupply * (10 ** uint256(decimals));
    balanceOf[msg.sender] = totalSupply;
}
```

## Functions

### Transfer
Transfers tokens from the caller's address to another address.

```solidity
function transfer(address _to, uint256 _value) public whenNotPaused returns (bool success)
```

**Parameters**:
- `_to`: The recipient's address.
- `_value`: The amount of tokens to transfer.

**Returns**: `true` on success.

### Approve
Approves a spender to transfer tokens on behalf of the caller.

```solidity
function approve(address _spender, uint256 _value) public whenNotPaused returns (bool success)
```

**Parameters**:
- `_spender`: The address authorized to spend the tokens.
- `_value`: The amount of tokens the spender is authorized to spend.

**Returns**: `true` on success.

### TransferFrom
Transfers tokens from one address to another using the allowance mechanism.

```solidity
function transferFrom(address _from, address _to, uint256 _value) public whenNotPaused returns (bool success)
```

**Parameters**:
- `_from`: The address from which tokens are transferred.
- `_to`: The recipient's address.
- `_value`: The amount of tokens to transfer.

**Returns**: `true` on success.

### Burn
Burns tokens, reducing the total supply.

```solidity
function burn(uint256 _value) public whenNotPaused
```

**Parameters**:
- `_value`: The amount of tokens to burn.

### Pause
Pauses the contract, disabling all transfers and approvals.

```solidity
function pause() public onlyOwner whenNotPaused
```

### Unpause
Unpauses the contract, re-enabling all transfers and approvals.

```solidity
function unpause() public onlyOwner whenPaused
```

### Transfer Ownership
Transfers ownership of the contract to a new address.

```solidity
function transferOwnership(address _newOwner) public onlyOwner
```

**Parameters**:
- `_newOwner`: The address of the new owner.

### Accept Ownership
Accepts ownership of the contract. This must be called by the new owner to finalize the transfer.

```solidity
function acceptOwnership() public
```

### Recover ERC-20 Tokens
Recovers ERC-20 tokens that were mistakenly sent to this contract.

```solidity
function recoverERC20(address tokenAddress, uint256 tokenAmount) public onlyOwner
```

**Parameters**:
- `tokenAddress`: The address of the ERC-20 token contract.
- `tokenAmount`: The amount of tokens to recover.

## Usage Instructions

### Deploying the Contract
Deploy the contract by specifying the initial supply of tokens. All tokens will be assigned to the deployer's address.

### Transferring Tokens
Use the `transfer` function to send tokens from your address to another. Ensure the contract is not paused.

### Approving Spenders
Use the `approve` function to allow another address to spend tokens on your behalf.

### Transferring Tokens via Spender
Use the `transferFrom` function to transfer tokens on behalf of another address, provided you have been approved to do so.

### Burning Tokens
To reduce the total supply, call the `burn` function with the number of tokens you wish to burn.

### Pausing the Contract
The contract owner can pause all token transfers and approvals by calling the `pause` function.

### Unpausing the Contract
The contract owner can re-enable token transfers and approvals by calling the `unpause` function.

### Transferring Ownership
The current owner can initiate an ownership transfer using `transferOwnership`. The new owner must call `acceptOwnership` to finalize the transfer.

### Recovering Mistakenly Sent Tokens
The contract owner can recover any ERC-20 tokens that were mistakenly sent to this contract by calling `recoverERC20`.

## Security Considerations
- The contract includes a `pause` feature that allows the owner to halt all token transfers in case of an emergency.
- Ownership transfer requires the new owner to accept the ownership, adding an additional layer of security.
- The contract is secured using the `onlyOwner` modifier for critical functions, ensuring that only the contract owner can execute these functions.

## License
This contract is released under the MIT License. See the `LICENSE` file for more details.
