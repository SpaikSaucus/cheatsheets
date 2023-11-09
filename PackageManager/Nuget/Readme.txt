TARGET:
	The relative path to the folder within the package where the source files are placed, which must begin with 
		lib, content, build, or tools.

WORK FLOW:
	1~ Create or Edit package.nuspec
	2~ Must complete the folders you need with your files.
	3~ Execute the command to create the file nupkg.
	4~ Execute the command to push the package in the repository manager.

COMMANDS: (Developer Command Prompt for VS):
	
	C:\..\..>nuget pack package.nuspec	
	C:\..\..>dotnet nuget push myapp-tools.1.0.0.nupkg -k XXXXXX-XXXX-XXXX-XXXX-XXXXXXX -s https://nexus.ar/repository/myapp/
	
REFERENCE:
	https://docs.microsoft.com/en-us/nuget/reference/nuspec

EXAMPLE:
	<?xml version="1.0" encoding="utf-8"?>
	<package>
		<metadata>
			<id>routedebugger</id>
			<version>1.0.0</version>
			<authors>Jay Hamlin</authors>
			<requireLicenseAcceptance>false</requireLicenseAcceptance>
			<description>Route Debugger is a little utility I wrote...</description>
			<dependencies>
			  <group targetFramework=".NETFramework4.0" />
			</dependencies>
		</metadata>
		<files>
			<file src="MyAssembly.dll" target="lib/net40" />
		</files>
	</package>

ERROR PACKAGE NO ACTUALIZA:
	NuGet Package Manager -> General -> Clear All NuGet Cache(s)
		De esta manera limpiaras el cache y podras volver a tratar de instalar
		el package generado que se encuentra en el repository.