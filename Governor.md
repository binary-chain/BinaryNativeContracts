# Governor Smart Contract

The `Governor` smart contract is designed to store and share addresses within the Binary Chain ecosystem, enabling the management of native parameters. Initially, control is shared between the DAO and an admin address. However, future upgrades will transition governance power exclusively to the DAO.

## Contract Overview

### Events

- `AdminKicked(string msg)`: Emitted when the admin is kicked by the DAO.
- `WelcomeNewAdmin(address indexed addr)`: Emitted when a new admin is set.

### Modifiers

- `onlyDAO`: Ensures the function can only be called by the DAO.
- `onlyGovernor`: Ensures the function can only be called by the governor, either the admin or the DAO.

## State Variables

- `address public admin`: Holds the address of the current admin.
- `address public DAO`: Holds the address of the DAO.
- `uint256 public unlockDAO`: Timestamp indicating when the DAO can take certain actions.
- `bool public isKicked`: Indicates if the admin has been kicked by the DAO.
- `bool public inited`: Indicates if the contract has been initialized.

## Functions

### `initContract()`

- Initializes the contract by setting the `admin` and `DAO` addresses.
- Sets `inited` to `true` to prevent re-initialization.
- Reverts with `ContractAlreadyInitError` if the contract is already initialized.

### `WhoIsGovernor()`

- Returns the current admin and DAO addresses.

### `setAdmin(address _addr)`

- Can be called by the governor to set a new admin address.
- Emits `WelcomeNewAdmin` event.

### `setDAO(address _addr)`

- Can be called by the governor to set a new DAO address.
- Reverts with `WrongAddressError` if the provided address is zero.
- If the admin has been kicked, both `DAO` and `admin` are set to the new address.
- Otherwise, sets the `DAO` and updates `unlockDAO` to be six months from the current timestamp.

### `kickAdmin()`

- Can be called by the DAO to kick the admin if the `unlockDAO` timestamp has passed.
- Sets `admin` to the DAO address and marks `isKicked` as `true`.
- Emits `AdminKicked` event.
- Reverts with `DaoNotUnlockedError` if called before the `unlockDAO` timestamp.
