# SAM-R Traces

This repository is meant as a continuation of the [Protocol Examples](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/eed8ab3c-839c-49e9-a524-703ed733f949) section of \[MS-SAMR\]. The intention is to give the examples of the protocol flow we would see in network traces or some other advanced debugging when common SAM-R operations are performed.

**Security Account Manager Remote Protocol** is used by the operating system during many harmless operations. However it can also be used by a malicious actors to perform reconaissance tasks. The repository gives examples of different operations and their associated sequences when SAM-R is used by the OS or potentially by a malicious actor.

🔎 [Creating a user account](/Creating%20a%20user%20account.md) (example from [\[MS-SAMR\]](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/3d8e23d8-d9df-481f-83b3-9175f980294c)) [^1]

🔎 [Enabling a user account](/Enabling%20a%20user%20account.md) (example from [\[MS-SAMR\]](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/bf8cfb76-24f7-42de-a95f-e5b9ec7435d0)) [^1]

🔎 [Creating a group](/Creating%20a%20group.md)

🔎 [Changing an account's password](/Changing%20a%20password.md)

🔎 Querying a user account's information

🔎 Querying a group's details and its members

🔎 Querying the domain's account policy

🔎 Enumerating all user accounts

🔎 Enumerating all groups


[^1]: These examples are not the only possible ways to interact with a SamServer to perform those actions.
