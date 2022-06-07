# escape=`

# Use the latest Windows Server Core 2019 image.
FROM mcr.microsoft.com/windows/servercore:ltsc2019

# Restore the default Windows shell for correct batch processing.
SHELL ["cmd", "/S", "/C"]

RUN `
    cd "%userprofile%" `
    `
    # Download the Visual Studio 2019 bootstrapper.
    && curl -SL --output vs_community.exe https://aka.ms/vs/16/release/vs_community.exe `
    `
    # Download Windows SDK 10.0.19041
    && curl -SL --output winsdksetup.exe https://go.microsoft.com/fwlink/?linkid=2120843 `
    `
    # Install Build Tools with the specifications provided chromium Windows build guide, excluding workloads and components with known issues.
    && (start /w vs_community.exe --quiet --wait --norestart --nocache `
        --installPath "%ProgramFiles(x86)%\Microsoft Visual Studio\2019\Community" `
        --add Microsoft.VisualStudio.Workload.NativeDesktop `
        --add Microsoft.VisualStudio.Component.VC.ATLMFC `
        --add Microsoft.VisualStudio.Component.VC.Tools.ARM64 `
        --add Microsoft.VisualStudio.Component.VC.MFC.ARM64 `
        --includeRecommended `
        --remove Microsoft.VisualStudio.Component.Windows10SDK.10240 `
        --remove Microsoft.VisualStudio.Component.Windows10SDK.10586 `
        --remove Microsoft.VisualStudio.Component.Windows10SDK.14393 `
        --remove Microsoft.VisualStudio.Component.Windows81SDK `
        || IF "%ERRORLEVEL%"=="3010" EXIT 0) `
    `
    # install Windows SDK 10.0.19041
    && (start /w winsdksetup.exe /features + /q /norestart) `
    `
    # Cleanup
    && del /q vs_community.exe winsdksetup.exe

# Define the entry point for the docker container.
# This entry point starts the developer command prompt and launches the PowerShell shell.
ENTRYPOINT ["C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Community\\Common7\\Tools\\VsDevCmd.bat", "&&", "powershell.exe", "-NoLogo", "-ExecutionPolicy", "Bypass"]
