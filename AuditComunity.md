# AuditCommunity Smart Contract

The `AuditCommunity` contract is designed to manage an audit community with specific roles, including seekers and hunters. It inherits from an abstract base contract `BASE` which defines essential addresses and governor functionalities.

## Contract Overview

### Events

- **NewHunter**: Emitted when a new hunter is approved.
- **HunterBanned**: Emitted when a hunter is banned.
- **HunterVoted**: Emitted when a hunter votes.
- **NewSeeker**: Emitted when a new seeker joins the community.
- **SeekerLeftCommunity**: Emitted when a seeker leaves the community.
- **VoteForBan**: Emitted when a vote to ban a seeker is cast.
- **SeekerBanned**: Emitted when a seeker is banned.
- **NewStakeForSeekers**: Emitted when a new stake amount for seekers is set.
- **NewStakeForHunters**: Emitted when a new stake amount for hunters is set.
- **ExSeekerGotStake**: Emitted when an ex-seeker retrieves their stake.
- **NewHunterRequest**: Emitted when a new hunter requests to join.
- **HunterLeftCommunity**: Emitted when a hunter leaves the community.

### Modifiers

- **onlyHunter**: Restricts function execution to active hunters.
- **onlyAuditContract**: Restricts function execution to the audit contract.
- **onlyInit**: Ensures function can only be executed if the contract is initialized.

### Enums

- **Status**: Represents the status of a seeker or hunter with values:
  - Absent
  - Pending
  - Active
  - Paused
  - Left
  - Banned

### Structs

- **Seeker**: Represents a seeker with:

  - `name` (string)
  - `addr` (address)
  - `index` (uint256)
  - `status` (Status)
  - `registredOn` (uint256)
  - `stake` (uint256)
  - `stakeWithdrawOn` (uint256)
  - `banVotes` (uint256)
  - `banVoters` (address[])

- **KickData**: Represents ban vote data for seekers:

  - `voted` (bool)
  - `index` (uint256)
  - `date` (uint256)

- **Hunter**: Represents a hunter with:

  - `name` (string)
  - `addr` (address)
  - `index` (uint256)
  - `status` (Status)
  - `registredOn` (uint256)
  - `balance` (Balance)
  - `lastQuarterBytesAudited` (uint256)
  - `totalBytesAudited` (uint256)
  - `contractsAssigned` (uint256[])
  - `auditing` (bool)
  - `isNameSet` (bool)

- **Balance**: Represents a hunter's balance with:
  - `stakeAndBalance` (uint256)
  - `refundedFees` (uint256)
  - `allTimeEarn` (uint256)
  - `stakeWithdrawOn` (uint256)

### Mappings

- **banVoteData**: Mapping of seekers to hunters to kick data.
- **Seekers**: Mapping of seeker addresses to seeker structs.
- **Hunters**: Mapping of hunter addresses to hunter structs.
- **PositionInAvailableHunters**: Mapping of hunter addresses to their position in available hunters array.
- **isNameUsed**: Mapping to check if a name is used.
- **AddressByName**: Mapping of names to addresses.

### Public Variables

- **SeekersList**: Array of seeker addresses.
- **HuntersList**: Array of hunter addresses.
- **AvailableHunters**: Array of available hunters for auditing.
- **applicantsList**: Array of addresses of applicants requesting to become hunters.
- **seekerStake**: Minimum stake required for seekers.
- **hunterStake**: Minimum stake required for hunters.
- **votesForBanSeeker**: Number of votes required to ban a seeker.
- **inited**: Boolean indicating if the contract is initialized.

### Functions

#### Initialization

- **initContract**: Initializes the contract with stakes and votes for banning seekers.

#### View Functions

- **getSeekerData**: Returns data of a specific seeker.
- **getSeekerWithdrawDate**: Returns the withdraw date of a specific seeker.
- **getBanVoteIndex**: Returns ban vote data for a specific seeker and hunter.
- **getTotVotes**: Returns the total ban votes for a specific seeker.
- **isSeeker**: Checks the status of a specific seeker.
- **getSeekerName**: Returns the name of a specific seeker.
- **getSeekerIndex**: Returns the index of a specific seeker.
- **getSeekersCount**: Returns the count of seekers.
- **getSeekersList**: Returns the list of seeker addresses.
- **getSeekersLast**: Returns the last 25 seekers or all if less than 25.
- **get25SeekersFromIndex**: Returns 25 seekers starting from a specific index.
- **getHunterData**: Returns data of a specific hunter.
- **isHunter**: Checks the status of a specific hunter.
- **getHunterName**: Returns the name of a specific hunter.
- **getHuntersNames**: Returns the names of multiple hunters.
- **getIndex**: Returns the index of a specific hunter.
- **getCheckedInfo**: Returns the last quarter and total bytes audited by a specific hunter.
- **getCodesReviewed**: Returns the contracts assigned to a specific hunter.
- **getHuntersCount**: Returns the count of hunters.
- **getHuntersList**: Returns the list of hunter addresses.
- **getHuntersLast**: Returns the last 25 hunters or all if less than 25.
- **get25HuntersFromIndex**: Returns 25 hunters starting from a specific index.

#### Audit Contract Functions

- **incremetCodeAuditAndBalance**: Increments code audit data and balance for a specific hunter.
- **engageHunters**: Engages hunters for an audit.
- **dismissHunters**: Dismisses hunters from an audit.

#### Governor Functions

- **nominateHunter**: Nominates a hunter.
- **pauseHunter**: Pauses a hunter.
- **unpauseHunter**: Unpauses a hunter.
- **banHunter**: Bans a hunter.
- **setNewStakeForSeekers**: Sets a new stake amount for seekers.
- **setNewStakeForHunters**: Sets a new stake amount for hunters.

#### Hunter Functions

- **requestToBecomeHunter**: Requests to become a hunter.
- **withdraw**: Withdraws excess balance.
- **leaveCommunityForHunter**: Leaves the community for hunters.
- **getStakeHunter**: Retrieves the stake for a hunter.
- **voteForBanSeeker**: Votes to ban a seeker.

#### Seeker Functions

- **becomeSeeker**: Becomes a seeker.
- **leaveCommunityForSeeker**: Leaves the community for seekers.
- **getStakeSeeker**: Retrieves the stake for a seeker.

#### Internal Functions

- **\_sendCoinsFromTreasuryTo**: Sends coins from the treasury to a specific address.
- **\_sendCoinsToTreasury**: Sends coins to the treasury.
- **\_sendCoinsTo**: Sends coins to a specific address.
