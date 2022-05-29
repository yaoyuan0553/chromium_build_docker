# escape=`

# Use the latest Windows Server Core 2022 image.
FROM mcr.microsoft.com/windows/servercore:ltsc2022

# Restore the default Windows shell for correct batch processing.
SHELL ["cmd", "/S", "/C"]

RUN `
    # Download the Visual Studio 2019 bootstrapper.
    curl -SL --output vs_community.exe https://aka.ms/vs/16/release/vs_community.exe `
    `
    # Download Windows SDK 10.0.20348
    && curl -SL --output winsdksetup.exe https://go.microsoft.com/fwlink/?linkid=2164145 `
    `
    # Download Chromium depot_tools
    && curl -SL --output depot_tools.zip https://storage.googleapis.com/chrome-infra/depot_tools.zip `
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
    # install Windows SDK 10.0.20348
    && (start /w winsdksetup.exe /features + /q /norestart) `
    `
    # Cleanup
    && del /q vs_community.exe winsdksetup.exe `
    `
    # unzip depot_tools for chromium
    && mkdir "%userprofile%\depot_tools" && tar -xf depot_tools.zip -C "%userprofile%\depot_tools" && del /q depot_tools.zip `
    `
    # add depot_tools to path
    && setx /M path "%userprofile%\depot_tools;%path%" `
    `
    # update depot tools
    && cd "%userprofile%\depot_tools" && update_depot_tools.bat

# Define the entry point for the docker container.
# This entry point starts the developer command prompt and launches the PowerShell shell.
ENTRYPOINT ["C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Community\\Common7\\Tools\\VsDevCmd.bat", "&&", "powershell.exe", "-NoLogo", "-ExecutionPolicy", "Bypass"]
