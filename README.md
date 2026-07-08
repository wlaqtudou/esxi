ESXi 8.0 U3 Realtek 网卡驱动注入制作说明

多合1网卡驱动脚本https://github.com/itiligent/ESXi-Custom-ISO
注释掉
$manualUpdate1 = "VMware-ESXi-8.0U3i-25205845-depot.zip" 
$manualUpdateUrl1 = "https://itiligent-my.sharepoint.com/personal/david_itiligent_com_au/_layouts/15/guestaccess.aspx?share=IQCF4Hyn6mTeQa_4ZDCjKEHTAQnKFHxTOAsUDSFDn6m7W00&e=Sm7Cyf&download=1"

修改为
$manualUpdate1 = "VMware-VMvisor-Installer-8.0.0.update03-24280767.x86_64-Dell_Customized-A02.zip"

运行
.\esxi8.ps1 -izip ".\VMware-VMvisor-Installer-8.0.0.update03-24280767.x86_64-Dell_Customized-A02.zip" -nsc

esxi8.0-24280767下载地址:
https://dl.dell.com/FOLDER12885639M/1/VMware-VMvisor-Installer-8.0.0.update03-24280767.x86_64-Dell_Customized-A02.iso

驱动下载地址：
https://github.com/dRumata/linux-notes/blob/main/net55-r8168-8.039.01-napi.x86_64.vib

Python 3.11 下载地址：
https://www.python.org/ftp/python/3.11.0/python-3.11.0-amd64.exe


一、准备文件

在 C 盘创建文件夹：

C:\ESXI


下载并放入：

- ESXi-Customizer-PS.ps1
- VMware-VMvisor-Installer-8.0.0.update03-24280767.x86_64-Dell_Customized-A02.zip
- net55-r8168-8.039.01-napi.x86_64.vib


创建驱动目录：

mkdir C:\ESXI\driver


将网卡驱动复制到：

C:\ESXI\driver\net55-r8168-8.039.01-napi.x86_64.vib


最终目录结构：

C:\ESXI
│
├── ESXi-Customizer-PS.ps1
├── VMware-VMvisor-Installer-8.0.0.update03-24280767.x86_64-Dell_Customized-A02.zip
│
└── driver
    └── net55-r8168-8.039.01-napi.x86_64.vib



二、设置 PowerShell 执行权限

以管理员身份运行 Windows PowerShell：

Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned



三、安装 Python 3.11

安装完成后检查：

python --version


安装依赖：

python -m pip install six lxml psutil pyopenssl



四、安装 VMware PowerCLI

运行：

Install-Module VMware.PowerCLI -Scope CurrentUser



五、开始制作 ESXi 自定义 ISO

进入目录：

cd C:\ESXI

临时解除脚本执行限制
Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process

运行：

.\ESXi-Customizer-PS.ps1 -izip ".\VMware-VMvisor-Installer-8.0.0.update03-24280767.x86_64-Dell_Customized-A02.zip" -pkgDir ".\driver" -nsc
.\esxi8.ps1 -izip ".\VMware-VMvisor-Installer-8.0.0.update03-24280767.x86_64-Dell_Customized-A02.zip" -nsc



完成后会生成：

DEL-ESXi_803.24280767-A02-customized.iso


使用 Rufus 将生成的 ISO 写入 U 盘，然后安装 ESXi。

安装时 Realtek PCIe GbE Family Controller 网卡应该可以被识别。
