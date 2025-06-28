---
title: "Compiling C Files with MSVC"
datePublished: Sat Jun 28 2025 09:21:01 GMT+0000 (Coordinated Universal Time)
cuid: cmcg16yro001602jx4ab11jxy
slug: compiling-c-files-with-msvc
tags: bash, windows, powershell, ccpp, fishshell, msvc

---

In MSYS2, you can use the GNU toolchain to compile C code. However, using MSVC for comparison can be a bit more cumbersome.

In the Start menu, search for `Developer Command Prompt for VS` to find a shortcut like `Developer Command Prompt for VS 2022`. This shortcut runs a batch script to set up the compilation environment: `%comspec% /k "C:\Program Files\Microsoft Visual Studio\2022\Community\Common7\Tools\VsDevCmd.bat"`. However, this is only available as a batch file. In Fish shell, both `cmd /k` and `cmd /c` will exit Fish and switch to cmd, which is somewhat inconvenient.

The following PowerShell script runs the batch file to store environment variables in a temporary file and then imports them into PowerShell, indirectly setting up the compilation environment:

```powershell
# Define paths
$vsDevCmd = "C:\Program Files\Microsoft Visual Studio\2022\Community\Common7\Tools\VsDevCmd.bat"
$tempEnvFile = "$env:TEMP\env_vars.txt"

# Create output directory
New-Item -ItemType Directory -Force -Path .\dist

# Run VsDevCmd.bat and capture environment variables
cmd.exe /c "`"$vsDevCmd`" -arch=x86 && set > $tempEnvFile"

# Import environment variables into PowerShell
Get-Content $tempEnvFile | ForEach-Object {
    if ($_ -match "^(.*?)=(.*)$") {
        $name = $matches[1]
        $value = $matches[2]
        if ($name -eq "PATH") {
            # Append to PATH without overwriting
            $env:PATH = "$env:PATH;$value"
        } else {
            # Set other environment variables
            Set-Item -Path "env:$name" -Value $value
        }
    }
}

# Clean up temporary file
Remove-Item $tempEnvFile -ErrorAction SilentlyContinue

# Run cl.exe
cl /nologo .\c\main.c /O2 /Fo.\dist\ /Fe.\dist\msvc.exe /link /nologo /LTCG /OPT:REF /OPT:ICF
```

This allows you to use PowerShell to compile C projects with MSVC:

```bash
powershell -c "./build-msvc.ps1"
```