# Dockerfile for Windows Servercore + PHP 5.6x86 + IIS

FROM mcr.microsoft.com/windows/servercore:20H2

MAINTAINER caioaraujo.immv@gmail.com

# Download section
ADD 'https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x64.exe' C://vc11.exe
ADD 'https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x86.exe' C://vc1186.exe
ADD 'https://aka.ms/vs/16/release/vc_redist.x64.exe' C://vc_redist.x64.exe
ADD 'https://aka.ms/vs/16/release/vc_redist.x86.exe' C://vc_redist.x86.exe
ADD https://windows.php.net/downloads/releases/archives/php-5.6.40-Win32-VC11-x86.zip php.zip
ADD https://download.microsoft.com/download/5/7/2/57249A3A-19D6-4901-ACCE-80924ABEB267/ENU/x64/msodbcsql.msi msodbcsql.msi
# End section

# Unzip archives
RUN powershell -command Expand-Archive -Path c:\php.zip -DestinationPath C:\php

# Set environment variables
RUN setx PATH /M %PATH%;C:\php

# Install programs
RUN C:\vc11.exe /install /passive /norestart
RUN C:\vc1186.exe /install /passive /norestart
RUN MSIEXEC.EXE /I "msodbcsql.msi" /L*v "C:\msodbcsql.log" /qb IACCEPTMSODBCSQLLICENSETERMS=YES ALLUSERS=1
RUN C:\vc_redist.x64.exe /install /passive /norestart
RUN C:\vc_redist.x86.exe /install /passive /norestart

# Enable WindowsFeature for IIS
RUN powershell -command Add-WindowsFeature Web-Server
RUN powershell -command Add-WindowsFeature Web-CGI

# Delete downloads archives
RUN powershell -command Remove-Item C://vc11.exe,C://vc1186.exe,C://vc_redist.x64.exe,C://vc_redist.x86.exe,msodbcsql.msi -Force

# Setup php config in IIS
ADD 'https://raw.githubusercontent.com/caio-brendo/docker-iis-php5.6/v1.0.0/src/config/applicationHost.config' C://Windows//System32//inetsrv//applicationHost.config
ADD 'https://raw.githubusercontent.com/caio-brendo/docker-iis-php5.6/v1.0.0/src/config/php.ini' C://php//php.ini
ADD 'https://raw.githubusercontent.com/caio-brendo/docker-iis-php5.6/v1.0.0/src/config/index.php' C://inetpub//wwwroot//default//index.php
ADD 'https://raw.githubusercontent.com/caio-brendo/docker-iis-php5.6/v1.0.0/src/config/web.config' C://inetpub//wwwroot//default//web.config