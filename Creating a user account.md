# Creating a user account

This is as-is taken from from [\[MS-SAMR\]](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/3d8e23d8-d9df-481f-83b3-9175f980294c).

The following sequence of methods and parameters creates a user account given a network address of "msdc-1", a domain name of "ms", and a user name of "testuser".

<details><summary>1. ➡️ Send SamrConnect</summary>
    
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
