# escape=`

# Use the prebuilt Windows Server Core 2019 image with 
# Visual Studio 2019 and Windows SDK 10.0.19041 installed.
FROM linuxez0553/servercore2019-vs2019-winsdk_10.0.19041

# Restore the default Windows shell for correct batch processing.
SHELL ["cmd", "/S", "/C"]

RUN `
    cd "%userprofile%" `
    `
    # Download Chromium depot_tools
    && curl -SL --output depot_tools.zip https://storage.googleapis.com/chrome-infra/depot_tools.zip `
    `
    # unzip depot_tools for chromium
    && mkdir depot_tools && tar -xf depot_tools.zip -C depot_tools && del /q depot_tools.zip `
    `
    # add depot_tools to path
    && setx /M path "%userprofile%\depot_tools;%path%" `
    `
    # update depot tools
    && cd depot_tools && update_depot_tools.bat

# Define the entry point for the docker container.
# This entry point starts the developer command prompt and launches the PowerShell shell.
ENTRYPOINT ["C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Community\\Common7\\Tools\\VsDevCmd.bat", "&&", "powershell.exe", "-NoLogo", "-ExecutionPolicy", "Bypass"]
