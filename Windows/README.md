# CheatSheets Windows

## Table of Contents
- [More useful commands for Windows](#more-useful-commands-for-windows)
- [All commands](#all-commands)
- [Environment variables](#environment-variables)

## Close session in server

```bash
qwinsta /server:myServer
```
Esto nos trae como resultado, las sesiones que en este momento están corriendo en el Servidor y con que usuarios. Es importante prestar atención a este detalle: cada sesión esta identificada con un numero, el cual se vera a la derecha del usuario.

```bash
rwinsta /server:asodev02 X
# X= Id de usuario.
```

## More useful commands for Windows.

```bash
> cmd 
# Command Prompt 
> ipconfig 
# IP Configuration 
> eventvwr.msc 
# Event Viewer 
> inetmgr 
# Internet Information Services Manager  
> net user /domain MY_USER
# Ver detalles del dominio de un usuario, como sus grupos y si se encuentra activo o no.
> nslookup 
# Ver detalles de como resuelve un dns.
> tracert XX.234.XX.XX 
# Para ver los nodos por los que se viaja la solicitud a una ip.
> subst d: c:\discod
# Generara el disco D basado en una carpeta existente en el disco C.
> certmgr.msc 
# Certificate Manager 
> cleanmgr 
# Disk Cleanup Utility 
> diskpart 
# Disk Partition Manager 
> lusrmgr.msc
# Local User and Group - Check Never Expired Password
# -> Users -> Properties -> [check] Password Never Expires
> powercfg.cpl
# Power Configuration 
> regedit
# Registry Editor 
> mstsc
# Remote Desktop 
> services.msc
# Services 
> shutdown
# Shut Down Windows 
> msconfig 
# System Configuration Utility 
> taskmgr 
# Task Manager 
> telnet 
# Telnet Client 
> notepad 
# Notepad 
> calc 
# Calculator 
> winver
# Windows Version (About Windows) 
```

## All commands

```bash
appwiz.cpl # Add/Remove Programs 
azman.msc # Authorization Manager (Vista/Win7) 
c:\Windows\System32\iexpress.exe # IExpress - Turn a cmd/vbs script into an installer .exe file  (example)
c:\windows\sysWOW64\odbcad32.exe # 32-bit ODBC driver under 64-bit platform
c:\windows\system32\odbcad32.exe # 64 bit ODBC driver under 64-bit platform
calc # Calculator 
certmgr.msc # Certificate Manager 
charmap # Character Map 
chkdsk # Check Disk Utility (XP) 
ciadv.msc # Indexing Service 
cleanmgr # Disk Cleanup Utility 
cliconfg # SQL Client Configuration 
clipbrd # Clipboard Viewer 
cmd # Command Prompt 
colorcpl # Color Management  
compMgmtLauncher # Computer Management (Vista/Win7) 
compmgmt.msc # Computer Management (XP) 
credwiz # Credential (passwords) Backup and Restore Wizard (Vista/Win7) 
dcomcnfg # Component Services 
defrag # Disk Defragmenter 
desk.cpl # Display Properties 
devmgmt.msc # Device Manager 
dfrg.msc # Disk Defragmenter (XP) 
dfrgui # Disk Defragmenter (Vista) 
dialer # Phone Dialer 
directx.cpl # Direct X Control Panel* 
diskmgmt.msc # Disk Management 
diskpart # Disk Partition Manager 
dpinst # Driver Package Installer (Vista/Win7)  
drwtsn32 # Dr. Watson System Troubleshooting Utility 
dvdplay # DVD Player 
dxdiag # Direct X Troubleshooter 
eudcedit # Private Character Editor 
eventvwr.msc # Event Viewer 
excel # Microsoft Excel 
findfast.cpl # Findfast 
firewall.cpl # Windows Firewall 
firewallControlPanel # Firewall Control Panel (Vista/Win7) 
firewallSettings # Firewall Settings (Vista/Win7)  
folders # Folders Properties control 
fonts # Fonts control 
fsmgmt.msc # Shared Folders 
fsquirt # Bluetooth Transfer Wizard 
gpedit.msc # Group Policy Editor (XP Prof) 
hdwwiz.cpl # Add Hardware Wizard 
inetcpl.cpl # Internet Properties 
inetmgr # Internet Information Services Manager  
intl.cpl # Regional Settings 
ipconfig # IP Configuration 
iscsicpl # iSCSI Initiator (Vista/Win7) 
joy.cpl # Game Controllers 
keyboard # Keyboard Properties control 
logoff # Log out 
lpksetup # Language Pack Installer (Vista/Win7)  
lusrmgr.msc # Local Users and Groups (XP) 
magnify # Windows Magnifier 
main.cpl # Mouse Properties 
mblctr # Windows Mobility Center (Mobile PCs only)(Vista/Win7) 
migwiz # Files and Settings Transfer Tool 
mmsys.cpl # Sounds and Audio 
mobsync # Syncronization Tool 
mouse # Mouse Properties control 
mrt # Microsoft Malicious Software Removal Tool 
msaccess # Microsoft Access 
msconfig # System Configuration Utility 
msdt # Microsoft Support Diagnostic Tool (Vista/Win7) 
msinfo32 # System Information 
msnmsgr # MSN Messenger
mspaint # Microsoft Paint 
msra # Remote Assistance(Vista/Win7) 
mstsc # Remote Desktop 
ncpa.cpl # Network Connections 
net user /domain MY_USER # Ver detalles del dominio de un usuario, como sus grupos y si se encuentra activo o no.
netconnections # Network Connections control 
netplwiz # Advanced User Accounts Control Panel (Vista/Win7)  
netsetup.cpl # Network Setup Wizard 
nltest # Realiza tareas administrativas de red. https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc731935(v=ws.11)
notepad # Notepad 
nslookup # Ver detalles de como resuelve un dns.
ntmsmgr.msc # Removable Storage 
ntmsoprq.msc # Removable Storage Operator Requests 
nusrmgr.cpl # User Account Management 
odbccp32.cpl # ODBC Data Source Administrator 
optionalfeatures # Windows Features (Vista/Win7) 
osk # On Screen Keyboard 
password.cpl # Password Properties 
pbrush # Paint 
perfmon.msc # Performance Monitor 
perfmon.msc # Reliability and Performance Monitor 
powercfg.cpl # Power Configuration 
powerpnt # Microsoft Powerpoint 
printers # Printers Folder 
quicktimeplayer # Quicktime Player 
regedit # Registry Editor 
regedit32 # Registry Editor 
rsop.msc # Resultant Set of Policy (XP Prof) 
schedtasks # Scheduled Tasks control 
sdclt # Backup Status and Utility (Vista/Win7) 
secpol.msc # Local Security Policy 
services.msc # Services 
sfc # System File Checker Utility (Scan/Purge)  
shrpubw # Shared Creation Wizard 
shutdown # Shut Down Windows 
sigverif # File Signature Verification Tool 
slui # Software Licensing/Activation (Vista/Win7) 
sndvol # Sound Volume (Vista/Win7) 
soundrecorder # Sound Recorder (Vista/Win7) 
sticpl.cpl # Scanners and Cameras 
subst d: c:\discod # Generara el disco D basado en una carpeta existente en el disco C.
sysdm.cpl # System Properties 
sysedit # System Configuration Editor 
syskey # Windows System Security Tool 
taskmgr # Task Manager 
telephon.cpl # Phone and Modem Options 
telnet # Telnet Client 
timedate.cpl # Date and Time Properties 
tnsping MY_DATABASE # Te informa que archivo de parametros sqlnet.ora esta utilizando y que adaptador de LDAP esta utilizando.
tourstart # Windows XP Tour Wizard 
tpmInit # Trusted Platform Module Initialization Wizard (Vista/Win7) 
tracert XX.234.XX.XX # Para ver los nodos por los que se viaja la solicitud a una ip.
tweakui # Tweak UI
userpasswords2 # User Accounts (Autologon) control 
utilman # Utility Manager 
verifier # Driver Verifier Utility 
wercon # Windows Error Reports  
wf.msc # Windows Firewall with Advanced Security (Vista/Win7) 
wiaacmgr # Windows Image Acquisition (scanner)(Vista/Win7) 
winver # Windows Version (About Windows) 
winword # Microsoft Word 
wmimgmt.msc # Windows Management Infrastructure 
write # Wordpad
wscui.cpl # Security Center 
wscui.cpl # Windows Security Center 
wuapp # Windows Update (Vista/Win7) 
wuaucpl.cpl # Automatic Updates 
wupdmgr # Windows Update 
wusa # Windows Update Standalone Installer(Vista/Win7) 
```

## Environment variables
%AppData%
%LocalAppData%
%ProgramFiles%