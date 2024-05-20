# Smart Contract Description: ContractAudit

The `ContractAudit` smart contract inherits from an abstract contract `BASE`. The `ContractAudit` contract provides functionality for managing the audit process for smart contracts within a blockchain system. It ensures that only authorized addresses "Hunters" can perform voting actions.

## Contract Overview

### Events

- `NewContractSubmited(uint256 _uuid, address indexed _deployer, bytes32 _hash, string _name, uint256 _deposit, uint256 _date)`: Emitted when a new contract is submitted for audit.
- `NewVoteForContract(uint256 _uuid, address indexed _hunter, bool _vote, uint256 date)`: Emitted when a new vote is cast for a contract.
- `ChangedVoteForContract(uint256 _uuid, address indexed _hunter, bool _vote, uint256 date)`: Emitted when a vote is changed for a contract.
- `ContractApproved(uint256 _uuid, address indexed _deployer, bytes32 _hash, string _name, uint256 _date)`: Emitted when a contract is approved.
- `ContractRejected(uint256 _uuid, address indexed _deployer, bytes32 _hash, string _name, uint256 _date)`: Emitted when a contract is rejected.
- `ContractApprovedByDAO(uint256 _uuid, address indexed _deployer, bytes32 _hash, string _name, uint256 _date)`: Emitted when a contract is approved by the DAO.

### Modifiers

- `onlyInited`: Ensures that the function can only be called if the contract is initialized.
- `onlyHunter`: Ensures that the function can only be called by a hunter.

### Enums

#### `Vote`

- `None`
- `Yea`
- `Nay`

#### `AuditStatus`

- `Absent`
- `OnAudit`
- `Rejected`
- `Approved`

### Structs

#### `Code`

- `uint256 auditUUID`: Audit UUID.
- `bytes32 contractHash`: Contract hash.
- `string contractName`: Contract name.
- `address deployerAddr`: Deployer address.
- `uint256 licenceCost`: License cost.
- `uint256 auditCost`: Audit cost.
- `uint256 submitedOn`: Submission date.
- `uint256 contractLen`: Contract length.
- `Votes votes`: Votes for the contract.
- `address[] hunters`: List of hunters.
- `AuditStatus status`: Audit status.
- `bool govApproved`: Indicates if the contract is approved by the governor.

#### `Votes`

- `uint256 yea`: Number of "yea" votes.
- `uint256 nay`: Number of "nay" votes.

### Mappings

- `mapping(uint256 => mapping(address => Vote)) public HunterVoteForContract`: Maps votes for contracts.
- `mapping(bytes32 => uint256) public uuidByHash`: Maps contract hashes to their UUIDs.
- `mapping(uint256 => Code) public codeByUuid`: Maps UUIDs to their code.
- `mapping(bytes32 => bool) public isHashOnVote`: Maps contract hashes to their vote status.
- `mapping(bytes32 => bool) public isHashRejected`: Maps contract hashes to their rejection status.

### State Variables

- `uint256 public uuid`: UUID counter.
- `uint256 public hundredBytesCost`: Cost per hundred bytes.
- `uint256 public discussionPeriod`: Discussion period duration.
- `bool public inited`: Indicates if the contract has been initialized.

### Functions

#### Initialization Functions

- `initContract() external onlyGovernor returns (bool)`: Initializes the contract. Reverts if the contract is already initialized.

#### View Functions

- `getContractsLast() external view returns (Code[] memory)`: Returns the last 25 contracts.
- `get25ContractsFromIndex(uint256 _idx) external view returns (Code[] memory)`: Returns 25 contracts from a specified index.
- `getCode(uint256 _auditUUID) external view returns (Code memory)`: Returns the code for a specified UUID.
- `getVotes(uint256 _auditUUID) external view returns (uint256, uint256)`: Returns the votes for a specified UUID.
- `getHuntersList(uint256 _auditUUID) external view returns (address[] memory)`: Returns the list of hunters for a specified UUID.
- `getVoterVote(uint256 _auditUUID, address _hunter) external view returns (string memory)`: Returns the vote of a hunter for a specified UUID.
- `getAuditStatus(uint256 _auditUUID) external view returns (AuditStatus, bool)`: Returns the audit status and governor approval status for a specified UUID.
- `getDiscussionPeriodEndDate(uint256 _auditUUID) external view returns (uint256)`: Returns the end date of the discussion period for a specified UUID.
- `getUUID(bytes32 _contractHash) external view returns (uint256)`: Returns the UUID for a specified contract hash.
- `calcAuditCost(bytes calldata _contractByteCode) public view returns (uint256 cost)`: Calculates the audit cost for a contract.

#### DAO Functions

- `ApproveByDAO(address _deployer, bytes32 _contractHash, string calldata _contractName) external onlyGovernor returns (bool)`: Approves a contract by the DAO.
- `setHundredBytesCost(uint256 _price) external onlyGovernor returns (bool)`: Sets the cost per hundred bytes.
- `setDiscussionPeriod(uint256 _time) external onlyGovernor returns (bool)`: Sets the discussion period duration.
- `submitToAudit(address _deployer, string calldata _contractName, bytes calldata _contractByteCode) external payable onlyInited returns (bool)`: Submits a contract for audit.

#### Voting Functions

- `vote(uint256 _auditUUID, bool _vote) external returns (bool)`: Casts a vote for a contract.

### Internal Functions

- `_calcVotes(Code storage _contractByteCode) internal`: Calculates the votes for a contract.
- `_setHunters(Code storage _contractByteCode, uint256 _auditUUID) internal`: Sets the hunters for a contract.
- `_freeHunters(address[] memory _hunters) internal`: Frees the hunters.
- `_approve(bytes32 _contractHash, string memory _contractName, address _deployer) internal`: Approves a contract.
- `_reject(string memory _contractName) internal`: Rejects a contract.
- `_isHunter(address _hunter) internal view returns (bool result)`: Checks if an address is a hunter.
- `_incremetCodeAuditAndBalance(address _hunter, uint256 _auditUUID, uint256 _gasUsed, uint256 _singleAuditCost, uint256 _contractLen) internal returns (bool)`: Increments the code audit and balance.
- `_isContractAbsent(bytes32 _contractHash) internal view returns (bool result)`: Checks if a contract is absent.
- `_validate(bytes32 _contractHash, string memory _contractName, address _deployer) internal returns (bool)`: Validates a contract.
- `_fullNameCheck(address _nameOwner, string memory _contractName) internal view returns (bool result)`: Checks the full name of a contract.
- `_getNameOwner(string memory _contractName) internal view returns (address result)`: Gets the name owner.
- `_isUsed(string memory _contractName) internal view returns (bool result)`: Checks if a name is used.
- `_setTook(string memory _contractName, address _nameOwner) internal returns (bool)`: Sets a name to "took" status.
- `_setOnReview(string memory _contractName) internal returns (bool)`: Sets a name to "on review" status.
- `_removeFromReview(string memory _contractName) internal returns (bool)`: Removes a name from review.
- `_getLicenceCost() internal view returns (uint256 result)`: Gets the license cost.
