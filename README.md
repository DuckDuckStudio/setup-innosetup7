# Setup Inno Setup 7

Install Inno Setup 7 in a Windows GitHub Actions workflow.

In the Windows Runner ([windows-2025](https://github.com/actions/runner-images/blob/main/images/windows/Windows2025-VS2026-Readme.md#tools) / [windows-2022](https://github.com/actions/runner-images/blob/main/images/windows/Windows2022-Readme.md#tools)), Inno Setup 6 is installed by default, but that's not what we want.  
This Action uses [Chocolatey](https://chocolatey.org/) to uninstall the existing Inno Setup 6, then uses [WinGet](https://learn.microsoft.com/en-us/windows/package-manager/winget/) to install Inno Setup 7, and adds the Inno Setup 7 directory to the workflow's `PATH`, allowing future steps to directly run commands such as `iscc`.

## Q&A

### Why not just use a single package manager to handle everything?

[The software installed by default on Windows is installed via Chocolatey](https://github.com/actions/runner-images#package-managers-usage), [Chocolatey will install Inno Setup to `C:\ProgramData\Chocolatey\bin`](https://docs.chocolatey.org/en-us/default-chocolatey-install-reasoning/).

If you use WinGet to uninstall Inno Setup 6 that was installed via Chocolatey, WinGet won't bother with any of that — it'll just run the uninstaller and be done with it.  
Then the `iscc` command (similar for other commands) still points to `C:\ProgramData\Chocolatey\bin\iscc.exe`, and future calls will result in a "file not found" error.

If you want to use Chocolatey to install Inno Setup 7, you'll realize that [Chocolatey only has Inno Setup 6](https://community.chocolatey.org/packages?q=innosetup).

### Why doesn't it work on Ubuntu or macOS?

I guess Chocolatey and WinGet aren't available on those systems, right?

## How to Use

Just like you would with any other action:

```yaml
- name: Setup Inno Setup 7
  uses: DuckDuckStudio/setup-innosetup7@1.0.0
```

## License

This project is licensed under the [MIT](LICENSE.txt) license.
