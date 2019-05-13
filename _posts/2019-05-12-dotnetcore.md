---
layout: post
title:  ".Net Core"
description: "A brief introduction to .Net Core"
date:   2019-05-12
tags: [.Net,C#]
comments: true
references: [
   "MSDN:https://msdn.microsoft.com/en-us/library/z1zx9t92.aspx",
   "C# Corner:http://www.c-sharpcorner.com/article/assembly-in-net/",
   
]

excerpt: "This article gives a brief introduction to .Net framework and its components.
We will discuss the three key terms, when we refer .Net framework (CTS, CLI and CLS).Then
we will dig deep into CLR and CLR execution model. We will end this post explaining JIT compilation process."
---

Managed code is distributed in Assemblies. Assemblies

* Acts as scoping boundary for types
* Assigns location independent names
* Provides versioning support

When you mark a type as public, its visible for a code outside this assembly. If you omit the access modifier
then its treated as internal. An internal type is visible only with in that assembly. A type name is qualified
by the assembly name too. That is why two types under same namespace in two different assemblies is treated as 
different type.

```csharp
foo.dll
namespace A 
{
    public class Calc
    {
        ...
    }
}
Type name : "A.Calc,foo"
```

```csharp
bar.dll
namespace A 
{
    public class Calc
    {
        ...
    }
}
Type name : "A.Calc,bar" 
```
We can use `GetType()` to see the fully qualfied name of a type as shown below.  

```csharp
 class Program
    {
        static void Main(string[] args)
        {
            string url = "http://www.google.com";
            Uri uri = new Uri(url);
            Console.WriteLine(url.GetType().AssemblyQualifiedName);
            Console.WriteLine(uri.GetType().AssemblyQualifiedName);
            Console.WriteLine(typeof(Program).AssemblyQualifiedName);
        }
    }

```
outputs :

```
System.String, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
System.Uri, System, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089
App.Program, App, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null

```
The output shows that 'string' and 'Uri' are types in same namespace but diffrent assembly. 'string' resides 
in `mscorlib` while latter in `system` assembly. In addition an Assembly name contains a version, culture and token. 
culture is mostly used for assemblies that hold localized strings.  

ILDASM is a handy tool to view assembly level information. For example you can see the dependent assemblies of a
particular assembly as shown below. In this case you can see this assembly depends on mscorlib, system and Lib assembly.

<img src='/images/2017-04-26-17-50-40.png' class='img-responsive'>  

### Assembly Names   
Assemblies are classified into two types.

* Signed Assembly (Strong Named)
* Non Signed Assembly  

Signed assemblies have a public key token associated with it, while other will have a null public key token as shown above.
In the example above, you can see mscorlib and system are signed assemblies while Lib is not. Signed assemblies are also 
called Strong Named assembly. Based on these types the CLR assembly resolution and loading varies.  

### Simple Assembly Resolution  

For unsigned assemblies, assembly resolution is straight forward and following is the order in which CLR searches for a 
particular assembly.  

* Check hosting application directory (APPBASE) where executable resides.
* Check subdirectory with same name as assembly to load. (check calc.dll in APPBASE\calc\calc.dll)
* Check additional subdirectories specified in application config file. (eg hello.exe's config file is hello.exe.config)

hello.exe.config is a copy of your "App.config" file that compiler generates when we build and copy to bin folder. Following
configuration in App.config will add notify compiler to consider subdirectory called 'libraries' for assembly probing.  

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
  <runtime>
    <gcConcurrent enabled="true"/>
    <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
      <publisherPolicy apply="yes" />
      <probing privatePath="libraries"/>
    </assemblyBinding>
  </runtime>
</configuration>

```
You can make following call to determine from which location, the Assembly got resolved. "Point" is a 
type defined in Lib assembly.  

```csharp
Console.WriteLine(typeof(Point).Assembly.CodeBase);

output
------
file:///D:/Git/Avial/C#/CS/clr-fundamentals/jit-compilation/App/bin/Debug/libraries/Lib.DLL

```
Note : For signed assemblies the Assembly resolution takes a diffrent path.



