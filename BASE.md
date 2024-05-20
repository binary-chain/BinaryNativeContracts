## BASE Contract

The `BASE` contract provides a foundational layer with shared constants and a function to retrieve governor details.

### Constants

- **GOVERNOR_ADDRESS**: Address of the governor.
- **TREASURY_CONTRACT**: Address of the treasury contract.
- **AUDIT_COMMUNITY_CONTRACT**: Address of the audit community contract.
- **AUDIT_CONTRACT**: Address of the audit contract.
- **NAMES_CONTRACT**: Address of the names contract.
- **SFC_CONTRACT**: Address of the SFC contract.

### Modifiers

- **onlyGovernor**: Restricts function execution to the governor.

### Functions

- **getGovernor**: Retrieves the governor's admin and DAO addresses by calling the `WhoIsGovernor` function on the `GOVERNOR_ADDRESS`.
