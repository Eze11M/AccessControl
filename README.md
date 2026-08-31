# Access Control simple solidity contract

This repo features a simple solidity contract that lets assign and manage different roles with different authorization levels.

## This contract features:

   1. A public mapping(bytes32 => mapping(address => bool)) "roles" state variable that stores a hashmap from the role to the accounts, and from the accounts to its assignation.
   2. A private bytes32 constant "ADMIN" and "USER" state values that stores the keccak256 that identifies the role on bytes32 fixed length.
   3. A "onlyRole(bytes32)" modifier that checks if the sender has the provided necessary authorization level to call the function.
   4. A contructor() that sets as "ADMIN" role the contract deployer using "_grantRole(bytes32, address)" function.
   5. An internal "_grantRole(bytes32, address)" function that assigns the given role to the given account.
   6. An external "grantRole(bytes32, address)" function that wraps "_granRole(bytes32, address)" to be used in pair of "onlyRole(bytes32)" modifier.
   7. An external "revokeRole(bytes32, address)" function that unassigns the given role to the given account, utilizing "onlyRole(bytes32)" modifier.
   8. Two external pure "getAdminRole()" and "getUserRole()" functions returning bytes32 role keccak256 that serve as test helpers for the contract.
   9. A customized event handling for granting and revoking roles.

## Advise:

The mapping `roles[_role][_account]` should be read as "from this role for the account X its granted".

We use keccak256 to encode roles so we ensure the length of the role identifier is always the same no matter what length the seed identifier string was.

The functions "getAdminRole()" and "getUserRole()" are intended for testing, and are pure because "ADMIN" and "USER" state values are constant; meaning its values are compiled into the contract code itself, so checking them does not require to lookup on a blockchain slot.