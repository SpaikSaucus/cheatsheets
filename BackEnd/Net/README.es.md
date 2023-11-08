[![en](https://img.shields.io/badge/lang-en-red.svg):black_large_square:](https://github.com/SpaikSaucus/cheatsheets/blob/main/BackEnd/Net/README.md) [![es](https://img.shields.io/badge/lang-es-yellow.svg):ballot_box_with_check:](#)

# CheatSheets Net

## Tabla de Contenido
- [NET 500.19 URLHandler error](#net-50019-urlhandler-error)
- [Dispose GC (Garbage Collector)](#dispose-gc-garbage-collector)
- [Ejecutar exe desde C# con CMD.exe](#ejecutar-exe-desde-c-con-cmdexe)
- [Remover BOM](#remover-bom)
- [Convertir Selección Multiple a un valor entero](#convertir-selección-multiple-a-un-valor-entero)
- [Chequear si es entero](#chequear-si-es-entero)
- [MSBUILD Execute](#msbuild-execute)
- [Verificar versión .NetFramework instalada](#verificar-versión-netframework-instalada)
- [WCF Chequear si esta levantado el servicio](#wcf-chequear-si-esta-levantado-el-servicio)
- [Group Linq Ejemplo](#group-linq-ejemplo)
- [Capturar LINQ query](#capturar-linq-query)
- [Capturar Exception de la capa de CLR](#capturar-exception-de-la-capa-de-clr)


## NET 500.19 URLHandler error

### PROBLEMA
Tengo una versión webAPI simple de Visual Studio 2013. Funciona bien cuando la ejecuto desde VS13 pero cuando copio el proyecto en IIS local me da el siguiente error.

HTTP Error 500.19 - Error interno del servidor No se puede acceder a la página solicitada porque los datos de configuración relacionados para la página no son válidos.

Información de error detallada:

    Módulo IIS Web Core
    Notificación BeginRequest
    Handler Aún no determinado
    Código de error 0x80070021
    Error de configuración Esta sección de configuración no se puede usar en esta ruta. Esto sucede cuando la sección está bloqueada en un nivel principal. 
    El bloqueo se realiza de forma predeterminada (overrideModeDefault = "Deny"), 
    o se establece explícitamente mediante una etiqueta de ubicación con overrideMode = "Deny" 
    o la heredada allowOverride = "false".
    Archivo de configuración \?\C:\inetpub\wwwroot\APITeslin\web.config

Fuente de configuración:
```xml
36:   <system.webServer>  
37:     <handlers>  
38:       <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
```

### SOLUCIÓN
Windows Server 2012, IIS 8.5 . Debería funcionar para otras versiones también.

Vaya al administrador del servidor, haga clic en agregar roles y funciones
En la sección de roles, elija: 
* Servidor web
    * En la subsección de Seguridad, seleccione todo (excluí resumen, restricciones de IP y autorización de URL ya que no los utilizo)
    * En Desarrollo de aplicaciones, elija: .NET Extensibility 4.5, ASP.NET 4.5 y ambas entradas ISAPI
    * En la sección de características, elija: NET 3.5, .NET 4.5, ASP.NET 4.5
    * En la sección del servidor web, seleccione: Servidor web (todo), Herramientas de administración (Consola de administración IIS y Servicio de administración), Autenticación de Windows - si está utilizando alguno de ellos.


## Dispose GC (Garbage Collector)

```csharp
using System;

public class Program
{
	public static void Main(string[] args)
	{
		Console.WriteLine("Init Program");

		using (var claseA = new A())
		{
			//Aquí veremos que no se hará el dispose al final de todo el programa de la clase B
			// ya que no se encuentra dentro de un using.
			var claseB = new B();

			using (var claseAuxHija = new AuxHija())
			{
				//Aquí veremos que se hace el dispose de Clase AuxBase, que es la clase que se heredada en clase AuxHija;
				// sim importar que no se hizo el using con la Clase AuxBase.
			}

			Console.WriteLine("End Program");
		}

		Console.ReadKey();
	}
}

public class A : IDisposable
{
	public string MiNombre
	{
		get { return "I am Class A"; }
	}
	public void Dispose()
	{
		Console.WriteLine(MiNombre + " I was made Dispose");
		GC.SuppressFinalize(this);
	}
}

public class B : IDisposable
{
	public string MiNombre
	{
		get { return "I am Class B"; }
	}
	public void Dispose()
	{
		Console.WriteLine(MiNombre + " I was made Dispose");
		GC.SuppressFinalize(this);
	}
}

public class AuxBase : IDisposable
{
	public string MiNombre
	{
		get { return "I am Class AuxBase"; }
	}
	public void Dispose()
	{
		Console.WriteLine(MiNombre + " I was made Dispose");
		GC.SuppressFinalize(this);
	}
}

public class AuxHija : AuxBase {}
```


## Ejecutar exe desde C# con CMD.exe

```csharp
[HttpPost]
[Route("api/maintenance/test")]
public HttpResponseMessage Test()
{
    var startInfo = new ProcessStartInfo();
    startInfo.WindowStyle = ProcessWindowStyle.Hidden;
    startInfo.Verb = "runas";
    startInfo.FileName = "cmd.exe";
    //startInfo.Arguments = @"/C C:\..\..\Baseline-branch\Demonio\bin\Debug\demonio.exe test& exit";
    //startInfo.Arguments = @"/C C:\..\..\Baseline-branch\Demonio\bin\Debug\demonio.exe test&ping 127.0.0.1 -n 10 > nul";
    startInfo.Arguments = @"/C C:\app\myApp-9080\WS\demonio.exe test";

    try
    {
        if (!File.Exists(@"C:\app\myApp-9080\WS\demonio.exe"))
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

## Remover BOM
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
Referencias:
* https://www.freecodecamp.org/news/a-quick-tale-about-feff-the-invisible-character-cd25cd4630e7/ 
* https://hexed.it/


## Convertir Selección Multiple a un valor entero

Vamos a convertir la selección multiple de valores a un numero entero que pueda ser usada para luego con ese valor entero, poder obtener la selección realizada. Esto vamos a realizarlo con la utilización de binarios.

Ejemplo, tipos de login disponibles en mi App:
* Auth0
* Google
* Facebook

Combinatorias posibles:
* Ningún login configurado: 000 (binario) 0 (entero)
* __Auth0__ único configurado: 001 (binario) 1 (entero)
* __Google__ único configurado: 010 (binario) 2 (entero)
* __Google__ + __Auth0__ configurados: 011 (binario) 3 (entero)
* __Facebook__ único configurado: 100 (binario) 4 (entero)
* __Facebook__ + __Auth0__ configurados: 101 (binario) 5 (entero)
* __Facebook__ + __Google__ configurados: 110 (binario) 6 (entero)
* __Facebook__ + __Google__ + __Auth0__ configurados: 111 (binario) 7 (entero)


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

		Console.ReadKey();
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


## Chequear si es entero

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
				Console.WriteLine(number + " no es entero.");
			else
				Console.WriteLine(number + " es entero.");
		}

		Console.ReadKey();
	}
}
```


## MSBUILD Execute

Usando la consola CMD en la siguiente ruta:
* Visual Studio 2015
    * Visual Studio 2015 :arrow_right: Visual Studio Tools :arrow_right: MSBuild Command Prompt for VS2015
* Visual Studio 2022
    * Microsoft Visual Studio :arrow_right: 2022 :arrow_right: Community :arrow_right: MSBuild :arrow_right: Current :arrow_right: Bin

Comando:
* _MSBuild MyApp.sln /t:Rebuild /p:Configuration=Release_
* _MSBuild MyApp.csproj /t:Clean /p:Configuration=Debug;TargetFrameworkVersion=v3.5_

Ejemplo:
* _C:\..\..>MSBuild.exe MyApp.Api.sln -t:rebuild_

## Verificar versión .NetFramework instalada

Puede verificar la versión de .NET Framework instalada en una computadora abriendo un símbolo del sistema, navegando a:
* cd %windir%\Microsoft.NET\FrameWork 
y luego navegando al directorio con el último número de versión. 

Una vez en el directorio con el último número de versión, ejecute el comando: 
* .\MSBuild.exe -version

El comando anterior generará la última versión instalada en el siguiente formato:
* _C:\Windows\Microsoft.NET\Framework\v4.0.30319>.\MSBuild.exe -version_

Cuando se ingresa el comando, aparecerá la siguiente información:

    Microsoft (R) Build Engine versión 4.6.1038.0
    [Microsoft .NET Framework, versión 4.0.30319.42000]
    Derechos de autor (C) Microsoft Corporation. Todos los derechos reservados.

La última línea, después del copyright, mostrará la versión más reciente de .NET Framework en su computadora, por ejemplo, 4.6.1038.0.


## WCF Chequear si esta levantado el servicio
```csharp
bool isServiceUp = true;

try
{
	// El address se debería obtener del client.config, de manera que se consume el servicio del ambiente.
	var address = "http://xxxxxx:2100/WCFMyCompanyServiceHost/IXXXXXXService.svc";
	var mexClient = new MetadataExchangeClient(new Uri(address), MetadataExchangeClientMode.HttpGet);
	mexClient.GetMetadata();
}
catch (Exception ex)
{
	isServiceUp = false;
} 
```


## Group Linq ejemplo

```csharp
using System;
using System.Collections.Generic;
using System.Linq;


public class Program
{
    public static void Main(string[] args)
    {
        var lst = new List<EmployeeData>();

        lst.Add(new EmployeeData() {
            Code = 1,
            Email = "employee1@company.com",
            Data = "Error without permissions"
        });
        lst.Add(new EmployeeData()
        {
            Code = 1,
            Email = "employee1@company.com",
            Data = "Absence 1 May"
        });
        lst.Add(new EmployeeData()
        {
            Code = 2,
            Email = "employee2@company.com",
            Data = "In training"
        });

        var results = lst.GroupBy(
            p => new { p.Code, p.Email },
            p => p.Data,
            (key, g) => new
            {
                Code = key.Code,
                Email = key.Email,
                Data = String.Join("/t ", g.Distinct())
            }
        );

        foreach(var result in results){
            Console.WriteLine(result.Code);
            Console.WriteLine(result.Email);
            Console.WriteLine(result.Data);
            Console.WriteLine();
        }
        Console.ReadKey();
    }
}

public class EmployeeData
{
    public uint Code { get; set; }
    public string Email { get; set; }
    public string Data { get; set; }
}

```


## Capturar LINQ query

```csharp
using System.Linq;
using System.Data.Objects;

using (efEntities context = new efEntities())
{
	var query = (from p in context.pais
				select p);
	var objectQuery = query as ObjectQuery;
	string sqlQuery = objectQuery.ToTraceString();
}
```


## Capturar Exception de la capa de CLR

Hay veces que las excepciones surgen dentro de una DLL que no es accesible, por lo que no se pude ver la excepcione ya que surgen en otra instancia del código. Para poder ver el lugar exacto donde surge la excepción. Debemos agregar al IDE, que muestre estas excepciones.

Debug :arrow_right: Exceptions :arrow_right: Tildar Thrown en Common Language Runtime Exceptions