# Enabling a user account

This is taken from [\[MS-SAMR\]](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/bf8cfb76-24f7-42de-a95f-e5b9ec7435d0).

The following sequence of methods and parameters enables the user account created in the previous example. This is performed on the machine with the network address of "msdc-1", a domain name of "ms", and a user name of "testuser" with Relative ID = 2810.

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

<details><summary>7. ➡️ Send SamrOpenUser</summary>

Details [SamrOpenUser](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/0aee1c31-ec40-4633-bb56-0cf8429093c0).

|Parameter field|Parameter value|
|--|--|
|DomainHandle|domainHandle|
|DesiredAccess|0x02000000|
|UserId|2810|
</details>

<details><summary>8. ⬅️ Receive SamrOpenUser</summary>

|Parameter field|Parameter value|
|--|--|
|Status|0|
|UserHandle|\[implementation-specific value\] userHandle|
|GrantedAccess|0xf07ff|
|RelativeId|2810|
</details>

<details><summary>9. ➡️ Send SamrSetInformationUser2</summary>

Details [SamrSetInformationUser2](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/99ee9f39-43e8-4bba-ac3a-82e0c0e0699e).

|Parameter field|Parameter value|
|--|--|
|UserHandle|userHandle|
|UserInformationClass|16|
|Buffer|Control = { 0x00000010 }|
</details>

<details><summary>10. ⬅️ Receive SamrSetInformationUser2</summary>

|Parameter field|Parameter value|
|--|--|
|Status|0|
</details>

<details><summary>11. ➡️ Send SamrCloseHandle</summary>

Details [SamrCloseHandle](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/55d134df-e257-48ad-8afa-cb2ca45cd3cc).

|Parameter field|Parameter value|
|--|--|
|Handle|userHandle|
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
|Handle|domainHandle|
</details>

<details><summary>14. ⬅️ Receive SamrCloseHandle</summary>

|Parameter field|Parameter value|
|--|--|
|Status|0|
|Handle|0|
</details>

<details><summary>15. ➡️ Send SamrCloseHandle</summary>

|Parameter field|Parameter value|
|--|--|
|Handle|serverHandle|
</details>

<details><summary>16. ⬅️ Receive SamrCloseHandle</summary>

|Parameter field|Parameter value|
|--|--|
|Status|0|
|Handle|0|
</details>
