# Creating a user account

This is taken from [\[MS-SAMR\]](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/3d8e23d8-d9df-481f-83b3-9175f980294c).

The following sequence of methods and parameters creates a user account given a network address of "msdc-1", a domain name of "ms", and a user name of "testuser".

<details><summary>1. ➡️ Send SamrConnect</summary>

Details [SamrConnect](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/defe2091-0a61-4dfa-be9a-2c1206d53a1f).
    
|Parameter field|Parameter value|
|--|--|
|ServerName|msdc-1|
|DesiredAccess|0x31|
</details>

<details><summary>2. ⬅️ Receive SamrConnect</summary>

|Parameter field|Parameter value|
|--|--|
|Status|0|
|ServerHandle|\[implementation-specific value\] serverHandle|
</details>

<details><summary>3. ➡️ Send SamrLookupDomainInSamServer</summary>

Details [SamrLookupDomainInSamServer](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/47492d59-e095-4398-b03e-8a062b989123).

|Parameter field|Parameter value|
|--|--|
|ServerHandle|serverHandle|
|Name.Length|4|
|Name.MaximumLength|4|
|Name.Buffer|ms|
</details>

<details><summary>4. ⬅️ Receive SamrLookupDomainInSamServer</summary>

|Parameter field|Parameter value|
|--|--|
|Status|0|
|DomainId|\[implementation-specific SID\]. For example: S-1-5-21-3448151421-356457007-600757626|
</details>

<details><summary>5. ➡️ Send SamrOpenDomain</summary>

Details [SamrOpenDomain](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/ba710c90-5b12-42f8-9e5a-d4aacc1329fa).

|Parameter field|Parameter value|
|--|--|
|ServerHandle|serverHandle|
|DesiredAccess|0x00000010|
|DomainId|S-1-5-21-3448151421-356457007-600757626|
</details>

<details><summary>6. ⬅️ Receive SamrOpenDomain</summary>

|Parameter field|Parameter value|
|--|--|
|Status|0|
|DomainHandle|\[implementation-specific value\] domainHandle|
</details>

<details><summary>7. ➡️ Send SamrCreateUser2InDomain</summary>

Details [SamrCreateUser2InDomain](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/a98d7fbb-1735-4fbf-b41a-ef363c899002).

|Parameter field|Parameter value|
|--|--|
|DomainHandle|domainHandle|
|Name.Length|16|
|Name.MaximumLength|16|
|Name.Buffer|testuser|
|AccountType|0x00000080|
|DesiredAccess|0x02000000|
</details>

<details><summary>8. ⬅️ Receive SamrCreateUser2InDomain</summary>

|Parameter field|Parameter value|
|--|--|
|Status|0|
|UserHandle|\[implementation-specific value\] userHandle|
|GrantedAccess|0xf07ff|
|RelativeId|2810|
</details>

<details><summary>9. ➡️ Send SamrCloseHandle</summary>

Details [SamrCloseHandle](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/55d134df-e257-48ad-8afa-cb2ca45cd3cc).

|Parameter field|Parameter value|
|--|--|
|Handle|userHandle|
</details>

<details><summary>10. ⬅️ Receive SamrCloseHandle</summary>
    
|Parameter field|Parameter value|
|--|--|
|Status|0|
|Handle|0|
</details>

<details><summary>11. ➡️ Send SamrCloseHandle</summary>

|Parameter field|Parameter value|
|--|--|
|Handle|domainHandle|
</details>

<details><summary>12. ⬅️ Receive SamrCloseHandle</summary>

|Parameter field|Parameter value|
|--|--|
|Status|0|
|Handle|0|
</details>

<details><summary>13. ➡️ Send SamrCloseHandle</summary>

|Parameter field|Parameter value|
|--|--|
|Handle|serverHandle|
</details>

<details><summary>14. ⬅️ Receive SamrCloseHandle</summary>

|Parameter field|Parameter value|
|--|--|
|Status|0|
|Handle|0|
</details>
