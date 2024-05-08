# What is cw3?

CW3 is a spec for on-chain multisigs, similar to Gnosis Safes on Ethereum.

A CW3 flex multisig contract uses a CW4 group contract as the backing privileged member set. The CW4 group contract is responsible for maintaining membership of who can approve proposals, and the CW3 flex multisig contract handles creating, voting, and executing proposals. Proposals are essentially wrapped messages that execute against the AppChain’s msg server.



