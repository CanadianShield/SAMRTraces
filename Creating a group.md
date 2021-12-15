# Creating a user account

This is taken from [\[MS-SAMR\]](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/3d8e23d8-d9df-481f-83b3-9175f980294c).

The following sequence of methods and parameters creates a group on a domain controller "secdc02", a domain name of "piesec", and a group name of "TestGroup". Here is the command used to trigger this flow: `net group TestGroup /add /domain`.

<details><summary>1. ➡️ Send SamrConnect5</summary>

Details [SamrConnect5](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/c842a897-0a42-4ca5-a607-2afd05271dae).
    
|Parameter field|Parameter value|
|--|--|
|ServerName|msdc-1|
|DesiredAccess|0x31|
|InVersion|1|
|InRevisionInfo|[SAMPR_REVISION_INFO_V1](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/963e60b5-9233-4669-b8a8-85bf4f0806dc) structure|
</details>

<details><summary>2. ⬅️ Receive SamrConnect5</summary>

|Parameter field|Parameter value|
|--|--|
|OutVersion|1|
|OutRevisionInfo|3|
|ServerHandle|\[implementation-specific value\] serverHandle|
</details>

<details><summary>3. ➡️ Send SamrEnumerateDomainsInSamServer</summary>

Details [SamrEnumerateDomainsInSamServer](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/2142fd2d-0854-42c1-a9fb-2fe964e381ce).
    
|Parameter field|Parameter value|
|--|--|
|ServerHandle|serverHandle|
|EnumerationContext|0x0|
|PreferedMaximumLength|0x2000|
</details>
