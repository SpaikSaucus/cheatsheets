[![en](https://img.shields.io/badge/lang-en-red.svg):ballot_box_with_check:](#) [![es](https://img.shields.io/badge/lang-es-yellow.svg):black_large_square:](https://github.com/SpaikSaucus/cheatsheets/blob/main/BackEnd/Net/README.es.md)

# CheatSheets Net

## Table of Contents
- [NET 500.19 URLHandler error](#net-50019-urlhandler-error)
- [Execute exe from C# with CMD.exe](#execute-exe-from-c-with-cmdexe)
- [Remove BOM](#remove-bom)
- [Convert Selection Multiple to integer value](#convert-selection-multiple-to-integer-value)
- [Check if it is integer](#check-if-it-is-integer)
- [MSBUILD Execute](#msbuild-execute)
- [Check installed .Net Framework version](#check-installed-net-framework-version)
- [WCF Check if the service is up](#wcf-check-if-the-service-is-up)


## NET 500.19 URLHandler error

### PROBLEM
I have a simple webAPI build by Visual Studio 2013. It works well when I run it from VS13 but when I copy the project in local IIS it gives me the following error.

HTTP Error 500.19 - Internal Server Error The requested page cannot be accessed because the related configuration data for the page is invalid.

Detailed Error Information:

    Module IIS Web Core
    Notification BeginRequest
    Handler Not yet determined
    Error Code 0x80070021
    Config Error This configuration section cannot be used at this path. This happens when the section is locked at a parent level. 
    Locking is either by default (overrideModeDefault="Deny"), or set explicitly by a location tag with overrideMode="Deny" or the legacy allowOverride="false".
    Config File \?\C:\inetpub\wwwroot\APITeslin\web.config
    
Config Source:

```xml
36:   <system.webServer>  
37:     <handlers>  
38:       <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
```

### SOLUTION
Windows Server 2012, IIS 8.5. Should work for other versions too.

Go to server manager, click add roles and features
In the roles section choose: 
* Web Server
    * Under Security sub-section choose everything (I excluded digest, IP restrictions and URL authorization as we don't use them)
    * Under Application Development choose .NET Extensibility 4.5, ASP.NET 4.5 and both ISAPI entries
    * In the features section choose: NET 3.5, .NET 4.5, ASP.NET 4.5
    * In the web server section choose: Web Server (all), Management Tools (IIS Management Console and Management Service), Windows Authentication - if you are using any of it.


## Execute exe from C# with CMD.exe

```csharp
[HttpPost]
[Route("api/maintenance/test")]
public HttpResponseMessage Test()
{
    var startInfo = new ProcessStartInfo();
    startInfo.WindowStyle = ProcessWindowStyle.Hidden;
    startInfo.Verb = "runas";
    startInfo.FileName = "cmd.exe";
    //startInfo.Arguments = @"/C C:\..\..\Baseline-branch\Demon\bin\Debug\demon.exe test& exit";
    //startInfo.Arguments = @"/C C:\..\..\Baseline-branch\Demon\bin\Debug\demon.exe test&ping 127.0.0.1 -n 10 > nul";
    startInfo.Arguments = @"/C C:\app\myApp-9080\WS\demon.exe test";

    try
    {
        if (!File.Exists(@"C:\app\myApp-9080\WS\demon.exe"))
            throw new Exception("The file not exists");

        using (var exeProcess = Process.Start(startInfo))
        {
            exeProcess.WaitForExit(1000 * 60 * 2);
        }
    }
    catch (Exception ex)
    {
        return Request.CreateResponse(HttpStatusCode.InternalServerError, new { Error = ex.Message });
    }

    return Request.CreateResponse(HttpStatusCode.OK);
}
```

## Remove BOM
```csharp
//Byte order mark or BOM
public static string RemoveBom(this string value) 
{
	try
	{
		return Regex.Replace(value, @"[\uFEFF]+g", string.Empty, RegexOptions.None, TimeSpan.FromSeconds(1.5));
	}
	catch (RegexMatchTimeoutException)
	{
		throw new Exception("The Regex timeout");
	}
}
```
References:
* https://www.freecodecamp.org/news/a-quick-tale-about-feff-the-invisible-character-cd25cd4630e7/ 
* https://hexed.it/


## Convert Selection Multiple to integer value

We are going to convert the multiple selection of values to an integer that can be used and then with that integer value, we can obtain the selection made. We are going to do this with the use of binaries.

Example, types of login available at my App:
* Auth0
* Google
* Facebook

Combining:
* No login configured: 000 (binary) 0 (integer)
* __Auth0__ configured unique: 001 (binary) 1 (integer)
* __Google__ configured unique: 010 (binary) 2 (integer)
* __Google__ + __Auth0__ configured: 011 (binary) 3 (integer)
* __Facebook__ configured unique: 100 (binary) 4 (integer)
* __Facebook__ + __Auth0__ configured: 101 (binary) 5 (integer)
* __Facebook__ + __Google__ configured: 110 (binary) 6 (integer)
* __Facebook__ + __Google__ + __Auth0__ configured: 111 (binary) 7 (integer)


```csharp
using System;
using System.Collections.Generic;
					
public class Program
{
	public static void Main()
	{
		var lst = new List<LoginType>();
		lst.Add(LoginType.Auth0);
		lst.Add(LoginType.Google);
		lst.Add(LoginType.Facebook);
		
		var result = ConvertListEnumToIntWithBinary(lst);
		
		Console.WriteLine(result.ToString());
		
		var result2 = ConvertIntToListEnumWithBinary<LoginType>(result);
		foreach(var loginT in result2) {
			Console.WriteLine(loginT.ToString());
		}
	}

	public static int ConvertListEnumToIntWithBinary<T>(List<T> listEnum)
	{
		if (!typeof(T).IsEnum) throw new Exception("ConvertListEnumToIntWithBinary: T is not enum");

		var result = 0;
		foreach (var l in listEnum) {
			result |= 1 << Convert.ToInt32(l);
		}
		return result;
	}

	public static List<T> ConvertIntToListEnumWithBinary<T>(int intListEnum)
	{
		if (!typeof(T).IsEnum) throw new Exception("ConvertIntToListEnumWithBinary: T is not enum");

		var result = new List<T>();
		foreach (var val in Enum.GetValues(typeof(T))) {
			if (HasBit(intListEnum, (int)val)) result.Add((T)val);
		}
		return result;
	}

	private static bool HasBit(int value, int bitNumber)
	{
		return (value & (1 << bitNumber)) != 0;
	}

	public enum LoginType {
		Auth0,
		Google,
		Facebook
	}
}
```


## Check if it is integer

```csharp
using System;
using System.Collections.Generic;

public class Program
{
	public static void Main()
	{
		var numbers = new List<double>(){ 
			1.034923, 
			2.0,
			3.141615478, 
			4,
			5.000000000001,
		};

		foreach (var number in numbers) {
			var remainder = number % 1;
			if (remainder != 0) 
				Console.WriteLine(number + " is not integer.");
			else
				Console.WriteLine(number + " is integer.");
		}
	}
}
```


## MSBUILD Execute

Using CMD console in the following path:
* Visual Studio 2015
    * Visual Studio 2015 :arrow_right: Visual Studio Tools :arrow_right: MSBuild Command Prompt for VS2015
* Visual Studio 2022
    * Microsoft Visual Studio :arrow_right: 2022 :arrow_right: Community :arrow_right: MSBuild :arrow_right: Current :arrow_right: Bin

Command:
* _MSBuild MyApp.sln /t:Rebuild /p:Configuration=Release_
* _MSBuild MyApp.csproj /t:Clean /p:Configuration=Debug;TargetFrameworkVersion=v3.5_

Example:
* _C:\..\..>MSBuild.exe MyApp.Api.sln -t:rebuild_


## Check installed .Net Framework version

You can check the version of the .NET Framework installed on a computer by opening a command prompt, navigating to:
* cd %windir%\Microsoft.NET\FrameWork 
and then navigating to the directory with the latest version number.

Once in the directory with the latest version number, execute command: 
* .\MSBuild.exe -version

The above command will output the latest installed version in the following format:
* _C:\Windows\Microsoft.NET\Framework\v4.0.30319>.\MSBuild.exe -version_

When the command is entered, the following information will appear:

    Microsoft (R) Build Engine version 4.6.1038.0
    [Microsoft .NET Framework, version 4.0.30319.42000]
    Copyright (C) Microsoft Corporation. All rights reserved.

The last line, after the copyright, will show the most recent version of the .NET Framework on your computer, for example, 4.6.1038.0.


## WCF Check if the service is up
```csharp
bool isServiceUp = true;

try
{
	// The address should be obtained from client.config so that the environment service is consumed.
	var address = "http://xxxxxx:2100/WCFMyCompanyServiceHost/IXXXXXXService.svc";
	var mexClient = new MetadataExchangeClient(new Uri(address), MetadataExchangeClientMode.HttpGet);
	mexClient.GetMetadata();
}
catch (Exception ex)
{
	isServiceUp = false;
} 
```