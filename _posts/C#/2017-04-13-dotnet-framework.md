---
layout: post
title:  ".Net Framework Essentials"
description: "A brief introduction to .Net framework and its components"
date:   2017-04-14
tags: [C#,.Net]
comments: true
references: [
   "MSDN:https://msdn.microsoft.com/en-us/library/z1zx9t92.aspx",
   "C# Corner:http://www.c-sharpcorner.com/article/assembly-in-net/",
   
]

excerpt: "This article gives a brief introduction to .Net framework and its components.
We will discuss the three key terms, when we refer .Net framework (CTS, CLI and CLS).Then
we will dig deep into CLR and CLR execution model. We will end this post explaining JIT compilation process."
---

When ever we read about .Net Framework, three key terms that we encounter are CLI, CLS and CTS. These are actually standards that defines the .Net Framework. To put it another way the .Net framework is actually one implementation of a spec called as **CLI** (Common Language Infrastructure). These are some key information you will find in this spec.

*  This spec defines in detail the operations or features to be supported by the execution engine (EE).
*  File format. What is inside the Managed executable file in the exe or dll ? (Derivative of PE32 spec)
*  Class libraries and classes that must be implemented by implementors of the framework.

There is a portion of spec which is commonly referred as **CTS** (Common Type System).  CTS is the part of spec that defines following info. Languages targeting CLR should support CTS.

*  Actual Instruction set supported by the execution Engine.
*  Types to be implemented as part of Execution engine. (string, integer, etc.)

There is a third portion of spec referred as **CLS** (Common Language Specification). This specify minimum set of types that should be included as part of the Run time. In developer perspective this is the minimum set of features just about you can count on to always exist in all variation of the CLI (Desktop CLR, Mono, Phone CLR etc..). So its 
advertising developers that these features will be built in available on wide variety of CLI implementation.  

<img src="/images/cli.png" class="img-responsive">  

There are various CLI implementations and following are popular one's  

* Microsoft .NET Framework (CLR) retail platform for Windows Desktops.
* Microsoft .NET Compact Framework (CF) targets pda's, smart phones
* Mono targets Linux, Mac (By Ximian)  

#### Language Support

Programming languages targeting .Net framework should adhere to CTS specification. Languages do not have to support 100% of the CTS. Each languages can chose a different subset of CTS to support. Also languages does not have to limit themselves to CTS. For example CTS spec described in CLI does not support Multiple Inheritance. But a language like C++ supports Multiple Inheritance.So thats where idea of CLS spec in CLI which 
specifies minimum set of features language targeting this framework should support, so that developers targeting this framework can count on.  

<img src="/images/lang-cli.png" class="img-responsive">  

#### CLR Execution
As described earlier, CLR is the Microsoft's implementation of CLI or in general called as .Net Framework. Now we will get deep into how our source code gets executed by CLR.  Source Code gets compiled into an Assembly (DLL or EXE by a **compiler**. The compiler packages your application into an Assembly which contains 

 * IL (Intermediate Language) format of your source code 
 * Metadata regarding your types (like access level of class, inheritance level, external class used etc..) 
 * PE specification, where the PE header is referred by the Windows Operating System to run the executable.

IL or Intermediate code is instructions targeting a Virtual machine used by execution engine. So all IL instructions will be instructions targeting registers defined in Virtual Machine and all.  

<pre class="line-numbers" ><code class="language-csharp">
    stfd int32 Point::Y
    ldc.i4 0xc8
</code></pre>

The above instructions are targeting the virtual machine.So now when you double click or run the executable, Windows will refer the PE headers and based on the type of assembly it create a process address space for executing this assembly. Then Windows maps the image of executable you run into that address space.Then it starts a thread to start the execution. The entry point where the thread starts execution is defined in one of the PE headers in the executable file. This is defined by the Compiler and usually the initial set of instructions will be to load or bootstrap the CLR. Its this instruction that checks the operating system version, installed.Net version and read settings defined by developer and decides to load which version of CLR to be loaded into the address space. 

So once the CLR get loaded, it will creates its execution environment like heap, thread
pools and then starts loading your assembly into the address space. IL is not a processor specific code. So its the process called JIT compilation that converts the IL code to X86 code (processor specific). CLR will start this JIT compilation from the entry point code specified in the assembly (usually main)and switch control of main thread to start executing this x86 instruction. Over the course of this execution, main method might be calling other methods and they are also JIT compiled to x86 instruction and allocate fields into heap etc. This will cascade till the main() exits.

At this moment our code will be executing under supervisory control of CLR and working in a restrictive environment. CLR also starts other runtime services like GC(Garbage Collection), Security (CAS), reflection etc which controls our execution environment.  

<img src="/images/execution.png" class="img-responsive">

You can view the IL code of an assembly using tools like ILDASM or Red Gate Reflector. Reflector even decompile the IL and shows the source code that may have been used. This helps us to learn more about third party codes to learn more about its implementation.  

<img src="/images/reflector.png" class="img-responsive">

#### JIT Compilation

Figure below shows a sample JIT Compilation process.

<img src="/images/jit.png" class="img-responsive">







