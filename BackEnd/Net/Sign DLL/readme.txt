sn.exe is a tools from Net Framework
-----------------------------------------

sn -Vl (Para ver la lista de los assemblys)
sn -Vx (para eliminar todos los assemblys en la lista)
sn -Vr *,f2ef049c21182d8e 	(Para agregar a la lista de assembly)



C:\..\..\..> sn -Vr ARQ_ServiceLocator.dll

Microsoft (R) .NET Framework Strong Name Utility  Version 1.1.4322.573
Copyright (C) Microsoft Corporation 1998-2002. All rights reserved.

Verification entry added for assembly 'ARQ_ServiceLocator,623C0F45FF9C2099'


C:\..\..\..> sn -Vl

Microsoft (R) .NET Framework Strong Name Utility  Version 1.1.4322.573
Copyright (C) Microsoft Corporation 1998-2002. All rights reserved.

Assembly/Strong Name                  Users
===========================================
ARQ_ServiceLocator,623C0F45FF9C2099   All users
ARQ_Web2008,623C0F45FF9C2099          All users



C:\..\..\..> makecert -sv myapp.pvk -n "CN=MYCOMPANY" mycompany.cer -b 01/01/2015 -e 01/01/2025 -r
C:\..\..\..> sn -R ARQ_ServiceLocator.dll C:\..\..\myapp.snk

.snk -> public key
.pvk -> private Key
.cer -> Certificate

/////makecert generate the .pvk and .cer files.

C:\..\..\..> certmgr -> certificate manager
