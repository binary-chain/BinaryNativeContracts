# Smart Contract Description: NamesList Contract

The `NamesList` smart contract inherits from an abstract contract `BASE`. The `NamesList` contract provides functionality for managing the reservation, purchase, and use of names within a Binary Chain. It ensures the uniqueness of names on the chain, preventing conflicts about namespace for a Dapps.

## Contract Overview

### Events

- `NewReservedName(string _name)`: Emitted when a new name is reserved.
- `NewReservedNames(string[] _names)`: Emitted when multiple names are reserved.
- `UserBoughtName(address indexed _addr, string _name)`: Emitted when a user buys a name.
- `NameUsed(string _name)`: Emitted when a name is used.

### Modifiers

- `onlyContracts`: Ensures that the function can only be called by specified contract addresses.
- `checkPriceAndReserved`: Checks if the name is lowercase, if the sender has enough funds, and if the name is free before allowing the function to proceed.
- `checkLower`: Ensures that the name is lowercase.

### Enums

#### `Status`

- `Free`: Name is available.
- `Reserved`: Name is reserved.
- `Used`: Name is used.
- `OnReview`: Name is under review.
- `Took`: Name is taken.

### Structs

#### `Name`

- `string name`: The name.
- `address nameOwner`: The owner of the name.
- `Status status`: The status of the name.

### Mappings

- `mapping(string => Name) public names`: Maps names to their details.

### State Variables

- `uint256 public charPrice`: Price per character for names.
- `bool public inited`: Indicates if the contract has been initialized.

### Functions

#### Initialization Functions

- `initContract() external onlyGovernor returns (bool)`: Initializes the contract with a set of reserved names. Reverts if the contract is already initialized.

#### Read Functions

- `isUsed(string calldata _contractName) external view returns (bool)`: Checks if a name is used.
- `getNameOwner(string calldata _contractName) public view returns (address)`: Returns the owner of a name.
- `isNameAvailable(string calldata _contractName) external view returns (bool)`: Checks if a name is available.
- `calcNamePrice(string memory _contractName) public view returns (uint256)`: Calculates the price of a name based on its length.

#### Contract Functions

- `fullNameCheck(address _nameOwner, string calldata _contractName) external view returns (bool)`: Checks if a name is fully available for the given owner.
- `setOnReview(string calldata _contractName) external returns (bool)`: Sets a name to be on review.
- `removeFromReview(string calldata _contractName) external returns (bool)`: Removes a name from review.
- `setTook(string calldata _contractName, address _nameOwner) external returns (bool)`: Sets a name to be taken by a specified owner.
- `setUsed(string calldata _contractName) external returns (bool)`: Sets a name to be used.

#### User Functions

- `buyName(string calldata _contractName) external payable returns (bool)`: Allows a user to buy a name if they have enough funds and the name is free.

#### Governor Functions

- `setPriceForChar(uint256 _price) public returns (bool)`: Sets the price per character for names.
- `addReservedName(string calldata _contractName) external returns (bool)`: Reserves a name.
- `addReservedNames(string[] calldata _contractsNames) external returns (bool, uint256)`: Reserves multiple names.
- `allowReservedNameToAddress(string calldata _contractName, address _nameOwner) external returns (bool)`: Allows a reserved name to be assigned to a specific address.

### Internal Functions

- `_isLower(string calldata str) internal pure returns (bool)`: Checks if a string is in lowercase.
