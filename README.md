# SAM-R Traces

This repository is meant as a continuation of the [Protocol Examples](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/eed8ab3c-839c-49e9-a524-703ed733f949) section of \[MS-SAMR\]. The intention is to give the examples of the protocol flow we would see in network traces or some other advanced debugging when common SAM-R operations are performed **against a domain controller**.

**Security Account Manager Remote Protocol** is used by the operating system during many harmless operations. However it can also be used by a malicious actors to perform reconaissance tasks. The repository gives examples of different operations and their associated sequences when SAM-R is used by the OS or potentially by a malicious actor.

## Actions known to use SAM-R

SAM-R on the network can just be the result of a benign script or application call.
- System.DirectoryServices.AccountManagement wraps ADSI and can lead to SAM-R call to a domain controller.
- ADSI and the WinNT provider also can lead to SAM-R calls (example: in PowerShell `[ADSI]"WinNT://contoso.com/Bob,user"` will generate multiples SAM-R calls).
- The `net.exe` (and its friend `net1.exe`) will use SAM-R against a domain controller (example: `net users /domain`).

## Examples of SAM-R flows

🔎 [Creating a user account](/Creating%20a%20user%20account.md) (example from [\[MS-SAMR\]](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/3d8e23d8-d9df-481f-83b3-9175f980294c) [^1] other example available in [\[MS-ADOD\]](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-adod/5535175f-bf6a-44f0-979d-93ff6db7c65c)). 

🔎 [Enabling a user account](/Enabling%20a%20user%20account.md) (example from [\[MS-SAMR\]](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/bf8cfb76-24f7-42de-a95f-e5b9ec7435d0)) [^1]

🔎 [Creating a group](/Creating%20a%20group.md)

🔎 [Changing an account's password](/Changing%20a%20password.md)

🔎 Querying a user account's information

🔎 Querying a group's details and its members

🔎 Querying the domain's account policy

🔎 [Enumerating all user accounts](/Enumerating%20all%20user%20accounts.md)

🔎 [Enumerating all groups](/Enumerating%20all%20groups.md)

[^1]: These examples are not the only possible ways to interact with a SamServer to perform those actions.
