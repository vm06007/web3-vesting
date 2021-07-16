# Create a contract with following functionality:

## TOPIC: Vesting Tokens (Covertable to ETH)
### Requirements:

a. Contract should manage one specific Vesting token.

	 -Vesting token is defined during contract creation.
	 -Vesting token cannot be changed after contract deployment.
	 -Vesting token balance should be pre-loaded in the contract.

b. Contract should manage Vesting distributions periods for Recipients.

	 -Recipient can only have one Vesting distribution assigned per address.
	 -Vesting distribution can be marked as Immutable not allowing to be destroyed.
	 -Vesting distribution should be done linearly according to following parameters:

 - Vesting Time (specified in seconds to determine lock time)
 - Vesting Amount (amount of tokens locked for vesting)
 - Available Amount (amount of tokens withdrawable right away)

c. Contract should allow Recipient to withdraw available amount and vest remaining amount linearly according to the examples below.

> Example A: Distribution is assigned to Recipient with following parameters:
0 tokens opened, 300 tokens locked for 600 seconds. (total 300 / 600 seconds)
Recipient will be able to withdraw 1 token every 2 seconds.
Available amount in this case will keep growing linearly.
Recipient can withdraw at any time and continue vest remaining amount.

> Example B: Distribution is assigned to Recipient with following parameters:
100 tokens opened, 300 tokens locked for 300 seconds. (total 400 / 300 seconds,
Meaning 100 tokens will be available to recipient instantly + 300 tokens vested.
Recipient can withdraw available amount at any time or total amount (400 tokens) after 300 seconds.

Each second Recipient is granted one more token in this example.
Recipient could continue withdrawing availale tokens each second as they become unlocked.


c) Contract should be managed by two separate addresses each having specific role:

	 - contract Master can create new Vesting distributions one for each Recipeint
	 - contract Keeper can destory existing Vesting distributions that are not marked as "Immutable"



Additional Requirements:

a) In case a distribution is not marked as Immutable it can be destroyed by contract Keeper.

b) In case contract Keeper destroys ongoing distribution all unlocked tokens must go to Recipient.

c) In case contract Keeper destroys ongoing distribution all locked tokens must remain in the contract and can be re-distributed to other Recipient.
_______
**IMPORTANT: When a new distribution for Recipient is created contract MUST CHECK and ensure that:**

	 -Vesting contract holds enough tokens to create this distribution!
	 -Recipient address is not a contract!


### DeFi aspect:
assuming there is a UNISWAP pair for the token, allow Recipient to choose withdrawal of token converted to ETH, create function that allows to:

	 functionA withdrawTokens() - withdraws Recipient tokens directly from contract.
	 functionB withdrawAsETH() - converts Recipient tokens to ETH (through UNISWAP) and sends ETH to Recipient.


###### BONUS POINTS: Try to solve this without (not using):
a) SafeMath library
b) ERC20 interface
c) If/Else branching

# UI

UI - create a simple UI showing to a visitor (if connected with MetaMask) following info:
* visitor has any distributions, e.g. visitor is a Recipient or not.
*  if visitor is a Recipient, show available token balance and ability to Withdraw()
* show previous Withdrawals that Recipeint already made before.

UI for deployer of the contract - allow to create vesting distributions for Recipients.

