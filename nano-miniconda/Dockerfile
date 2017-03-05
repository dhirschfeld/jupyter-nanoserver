FROM microsoft/nanoserver

SHELL ["powershell", "-Command", "$ErrorActionPreference='Stop'; $ProgressPreference='SilentlyContinue';"]

# Use local Miniconda install to deploy
# https://github.com/ContinuumIO/anaconda-issues/issues/1219
ADD Miniconda3 C:/Miniconda3

RUN $path = $env:path + ';C:\Miniconda3;C:\Miniconda3\Scripts;C:\Miniconda3\Library\bin'; \
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment' -Name Path -Value $path; \
    New-Item -Path 'HKLM:\SOFTWARE\Python\PythonCore\3.6\PythonPath' -Value 'C:\Miniconda3\Lib;C:\Miniconda3\DLLs' -Force; \
    New-Item -Path 'HKLM:\SOFTWARE\Python\PythonCore\3.6\InstallPath' -Value 'C:\Miniconda3' -Force; \
    New-Item -Path 'HKLM:\SOFTWARE\Python\PythonCore\3.6\InstallPath\InstallGroup' -Value 'Python 3.6' -Force; \

# Work around broken ruamel_yaml build
# https://github.com/ContinuumIO/anaconda-issues/issues/1469
RUN Remove-Item -Force -Recurse C:/Miniconda3/lib/site-packages/ruamel_yaml; 
ADD ruamel_yaml-0.11.14-py3.6.egg-info C:/Miniconda3/lib/site-packages
ADD ruamel_yaml C:/Miniconda3/lib/site-packages/ruamel_yaml

# Deploy unreleased conda with required menuinst patch
# https://github.com/conda/conda/pull/4709
RUN Start-Process conda -ArgumentList 'update','--yes','-c','dhirschfeld','conda' -NoNewWindow -Wait; \
    Start-Process conda -ArgumentList 'clean','-tips' -NoNewWindow -Wait;

CMD ["cmd.exe"]
