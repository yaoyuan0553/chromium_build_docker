# escape=`

# Use the latest Windows Server Core 2019 image.
FROM mcr.microsoft.com/windows/servercore:ltsc2019

# Restore the default Windows shell for correct batch processing.
SHELL ["cmd", "/S", "/C"]

RUN `
    # Download the Visual Studio 2017 bootstrapper.
    curl -SL --output vs_community.exe https://aka.ms/vs/15/release/vs_community.exe `
    `
    # Download Windows SDK 10.0.18362
    && curl -SL --output winsdksetup.exe https://go.microsoft.com/fwlink/?linkid=2083338 `
    `
    # Download Chromium depot_tools
    && curl -SL --output depot_tools.zip https://storage.googleapis.com/chrome-infra/depot_tools.zip `
    `
    # Install Visual Studio 2017 with the specifications provided chromium Windows build guide, excluding workloads and components with known issues.
    && (start /w vs_community.exe --quiet --wait --norestart --nocache `
        --installPath "%ProgramFiles(x86)%\Microsoft Visual Studio\2017\Community" `
        --add Microsoft.VisualStudio.Workload.NativeDesktop `
        --add Microsoft.VisualStudio.Component.VC.ATLMFC `
        --add Microsoft.VisualStudio.Component.VC.Tools.ARM64 `
        --add Microsoft.VisualStudio.Component.VC.MFC.ARM64 `
        --includeRecommended `
        --remove Microsoft.VisualStudio.Component.Windows10SDK.14393 `
        --remove Microsoft.VisualStudio.Component.Windows10SDK.15063 `
        --remove Microsoft.VisualStudio.Component.Windows10SDK.16299 `
        --remove Microsoft.VisualStudio.Component.Windows10SDK.17134 `
        --remove Microsoft.VisualStudio.Component.Windows10SDK.17763 `
        --remove Microsoft.VisualStudio.Component.Windows81SDK `
        || IF "%ERRORLEVEL%"=="3010" EXIT 0) `
    `
    # install Windows SDK 10.0.18362
    && (start /w winsdksetup.exe /features + /q /norestart) `
    `
    # Cleanup
    && del /q vs_community.exe winsdksetup.exe `
    `
    # unzip depot_tools
    && mkdir depot_tools && tar -xf depot_tools.zip -C depot_tools && del /q depot_tools.zip `
    `
    # add depot_tools to path
    && setx /M path "C:\depot_tools;%path%" `
    `
    # update depot tools
    && cd depot_tools && update_depot_tools.bat

    
RUN `
    # temporary fix for the chromium 78 bug of not installing pypiwin32
    setx /M PYTHONIOENCODING "UTF-8" && python -m pip install pypiwin32

# Define the entry point for the docker container.
# This entry point starts the developer command prompt and launches the PowerShell shell.
ENTRYPOINT ["C:\\Program Files (x86)\\Microsoft Visual Studio\\2017\\Community\\Common7\\Tools\\VsDevCmd.bat", "&&", "powershell.exe", "-NoLogo", "-ExecutionPolicy", "Bypass"]
