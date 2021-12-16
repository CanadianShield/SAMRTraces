# Enumerating all groups

The following sequence of methods and parameters enumerates all groups on a domain controller "secdc02" in a domain name of "piesec". Here is the command used to trigger this flow: `net group /domain`.

Note that the field `Status` refers to the `ReturnValue` on a network trace. 

<details><summary>1. ➡️ Send SamrConnect5</summary>

Details [SamrConnect5](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/c842a897-0a42-4ca5-a607-2afd05271dae).
    
|Parameter field|Parameter value|
|--|--|
|ServerName|\\SECDC02.piesec.ca|
|DesiredAccess|0x301|
|InVersion|1|
|InRevisionInfo|[SAMPR_REVISION_INFO_V1](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/963e60b5-9233-4669-b8a8-85bf4f0806dc) structure|
    
`DesiredAccess` mask corresponds to `SpecificRights:SamServerEnumerateDomains`.
</details>

<details><summary>2. ⬅️ Receive SamrConnect5</summary>

|Parameter field|Parameter value|
|--|--|
|OutVersion|1|
|OutRevisionInfo|3|
|ServerHandle|\[implementation-specific value\] serverHandle|
|Status|0|
</details>

<details><summary>3. ➡️ Send SamrEnumerateDomainsInSamServer</summary>

Details [SamrEnumerateDomainsInSamServer](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/2142fd2d-0854-42c1-a9fb-2fe964e381ce).
    
|Parameter field|Parameter value|
|--|--|
|ServerHandle|serverHandle|
|EnumerationContext|0x0|
|PreferedMaximumLength|0x2000|
</details>

<details><summary>4. ⬅️ Receive SamrEnumerateDomainsInSamServer</summary>

|Parameter field|Parameter value|
|--|--|
|EnumerationContext|4|
|Buffer|[SAMPR_ENUMERATION_BUFFER](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/c53161a4-38e8-4a28-a33e-0d378fce03dd) structure|
|CountReturned|2|
|Status|0|
</details>

<details><summary>5. ➡️ Send SamrLookupDomainInSamServer</summary>

Details [SamrLookupDomainInSamServer](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/47492d59-e095-4398-b03e-8a062b989123).
    
|Parameter field|Parameter value|
|--|--|
|ServerHandle|serverHandle|
|Name|piesec|
</details>

<details><summary>6. ⬅️ Receive SamrLookupDomainInSamServer</summary>

|Parameter field|Parameter value|
|--|--|
|DomainId|\[implementation-specific SID\]. For example: S-1-5-21-776355648-152374955-3729610662|
|Status|0|
</details>

<details><summary>7. ➡️ Send SamrOpenDomain</summary>

Details [SamrOpenDomain](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/ba710c90-5b12-42f8-9e5a-d4aacc1329fa).
    
|Parameter field|Parameter value|
|--|--|
|ServerHandle|serverHandle|
|DesiredAccess|0x304|
|DomainId|S-1-5-21-776355648-152374955-3729610662|
    
`DesiredAccess` mask corresponds to `SpecificRights: DomainReadOther`, `SpecificRights: DomainListAccounts` and `SpecificRights: DomainLookup`.
</details>

<details><summary>8. ⬅️ Receive SamrOpenDomain</summary>

|Parameter field|Parameter value|
|--|--|
|DomainHandle|\[implementation-specific value\] domainHandle|
|Status|0|
    
The `Buffer` structure contains a sub structure `SamprEnumerationBuffer` listing the name of the domain as well as the container where the group will be created.
</details>

<details><summary>9. ➡️ Send SamrQueryInformationDomain</summary>

Details [SamrQueryInformationDomain](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/5d6a2817-caa9-41ca-a269-fd13ecbb4fa8).
    
|Parameter field|Parameter value|
|--|--|
|DomainHandle|domainHandle|
|DomainInformationClass|[DomainInformationClass](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/3e8738b2-5df6-499f-907d-ac2471bf0281) enumeration: 0x2|

`DomainInformationClass` mask is `DomainGeneralInformation`.
</details>

<details><summary>10. ⬅️ Receive SamrQueryInformationDomain</summary>

|Parameter field|Parameter value|
|--|--|
|Buffer|[SAMPR_DOMAIN_INFO_BUFFER](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/1adc2142-dbb8-4554-aa24-010c713698bf) structure|
|Status|0|
    
The `Buffer` structure contains a sub structure `SamprEnumerationBuffer` listing the name of the domain as well as the container where the group will be created.
</details>

<details><summary>11. ➡️ Send SamrQueryDisplayInformation2</summary>

Details [SamrQueryDisplayInformation2](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/9aa37aa4-f77e-49b1-9993-5c92ba97cd62).
    
|Parameter field|Parameter value|
|--|--|
|DomainHandle|domainHandle|
|DomainInformationClass| 0x0002|

`DomainInformationClass` 0x2 is `DomainGeneralInformation`.
</details>

<details><summary>12. ⬅️ Receive SamrQueryDisplayInformation2</summary>

|Parameter field|Parameter value|
|--|--|
|TotalAvailable|0x0|
|TotalReturned|0x200|
|Buffer|PSAMPR_DISPLAY_INFO_BUFFER strucutre|
    
The `Buffer` wtructure contains a sub structure `GroupInformation` listing all groups matching the request.
</details>

<details><summary>13. ➡️ Send SamrCloseHandle</summary>

Details [SamrCloseHandle](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/55d134df-e257-48ad-8afa-cb2ca45cd3cc).
    
|Parameter field|Parameter value|
|--|--|
|SamHandle|samHandle|
</details>

<details><summary>14. ⬅️ Receive SamrCloseHandle</summary>

|Parameter field|Parameter value|
|--|--|
|SamHandle|{00000000-00000000-0000-0000-0000-000000000000}|
|Status|0|
</details>
