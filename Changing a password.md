# Changing an account's password

The following sequence of methods and parameters changes the password of a user called "testuser" on a domain controller "secdc02", a domain name of "piesec.ca" (NetBIOS name "piesec"). The relative identifier of the user is 1604.
Here is the command used to trigger this flow: `net user TestUser Pa$$w0rd /domain`.
It actually triggers more operations than what is represented here but the operations priors that are only to gather informations such as the relative identifier (via [SamrQueryInformationUser](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/4ad8d54c-0d5a-4d5a-9e9a-1bc9ee008d47)), not to change the password. Therefore, only the operations with the handles changing the password are represented here.

Note that the field `Status` refers to the `ReturnValue` on a network trace. 

<details><summary>1. ➡️ Send SamrConnect5</summary>

Details [SamrConnect5](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/c842a897-0a42-4ca5-a607-2afd05271dae).
    
|Parameter field|Parameter value|
|--|--|
|ServerName|\\secdc02.piesec.ca|
|DesiredAccess|0x30|
|InVersion|1|
|InRevisionInfo|[SAMPR_REVISION_INFO_V1](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/963e60b5-9233-4669-b8a8-85bf4f0806dc) structure|
    
`DesiredAccess` mask corresponds to `SpecificRights: SamServerEnumerateDomains` and `SpecificRights: SamServerLookupDomain`.
</details>

<details><summary>2. ⬅️ Receive SamrConnect5</summary>

|Parameter field|Parameter value|
|--|--|
|OutVersion|1|
|OutRevisionInfo|3|
|ServerHandle|\[implementation-specific value\] serverHandle|
|Status|0|
</details>

<details><summary>3. ➡️ Send SamrLookupDomainInSamServer</summary>

Details [SamrLookupDomainInSamServer](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/47492d59-e095-4398-b03e-8a062b989123).
    
|Parameter field|Parameter value|
|--|--|
|ServerHandle|serverHandle|
|Name|PIESEC|
</details>

<details><summary>4. ⬅️ Receive SamrLookupDomainInSamServer</summary>

|Parameter field|Parameter value|
|--|--|
|DomainId|\[implementation-specific SID\]. For example: S-1-5-21-776355648-152374955-3729610662|
|Status|0|
    
The `Buffer` structure contains a sub structure `SamprEnumerationBuffer` listing the name of the domain as well as the container where the group will be created.
</details>

<details><summary>5. ➡️ Send SamrOpenDomain</summary>

Details [SamrOpenDomain](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/ba710c90-5b12-42f8-9e5a-d4aacc1329fa).
    
|Parameter field|Parameter value|
|--|--|
|ServerHandle|serverHandle|
|DesiredAccess|0x280|
|DomainId|S-1-5-21-776355648-152374955-3729610662|
    
`DesiredAccess` mask corresponds to `SpecificRights: DomainGetAliasMembership` and `SpecificRights: DomainLookup`.
</details>

<details><summary>6. ⬅️ Receive SamrOpenDomain</summary>

|Parameter field|Parameter value|
|--|--|
|DomainHandle|\[implementation-specific value\] domainHandle|
|Status|0|
</details>

<details><summary>7. ➡️ Send SamrLookupNamesInDomain</summary>

Details [SamrLookupNamesInDomain](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/d91271c6-7b2e-4194-9927-8fabfa429f90).
    
|Parameter field|Parameter value|
|--|--|
|DomainHandle|domainHandle|
|Count|1|
|Names|One entrie: testuser|
</details>

<details><summary>8. ⬅️ Receive SamrLookupNamesInDomain</summary>

|Parameter field|Parameter value|
|--|--|
|RelativeIds|RID Count: 1|
|Use|Count: 1|
|Status|0|
    
In the `Use` field, the element 0x1 corresponds to `SidTypeUser - User account`.
</details>

<details><summary>9. ➡️ Send SamrQueryInformationUser</summary>

Details [SamrQueryInformationUser](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/e66db19f-600a-481b-bc4e-23953433255d).
    
|Parameter field|Parameter value|
|--|--|
|UserHandle|\[implementation-specific value\] userHandle|
|UserInformationClass|0x10|

`UserInformationClass` enumeration is described here: [USER_INFORMATION_CLASS](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/6b0dff90-5ac0-429a-93aa-150334adabf6). `0x10` is `UserControlInformation`.
</details>

<details><summary>10. ⬅️ Receive SamrQueryInformationUser</summary>

|Parameter field|Parameter value|
|--|--|
|Buffer|[SAMPR_USER_INFO_BUFFER](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/9496c26e-490b-4e76-827f-2695fc216f35) structure|
|Status|0|
</details>
