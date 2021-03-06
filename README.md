# Disasmo
[VS2019 Add-in.](https://marketplace.visualstudio.com/items?itemName=EgorBogatov.Disasmo)
Click on any method or class to see what .NET Core's JIT generates for them (ASM).

![demo](images/screenshot.gif)


The Add-in targets .NET Core contributors so it assumes you already have CoreCLR local repo.
If you don't have it, the steps to obtain and configure it are:
```bash
git clone git@github.com:dotnet/coreclr.git
cd coreclr
build release skiptests
build debug skiptests
```
We have to build it twice because we need mostly release files and a debug version of `clrjit.dll`.
For more details visit [viewing-jit-dumps.md](https://github.com/dotnet/coreclr/blob/master/Documentation/building/viewing-jit-dumps.md).
The Add-in basically follows steps mentioned in the doc above:
```
dotnet restore
dotnet publish -r win-x64 -c Release
set COMPlus_JitDisasm=%method%
ConsoleApp123.exe
```
In order to be able to disasm any method (even unused) the add-in injects a small line to the app's `Main()`:
```csharp
System.Runtime.CompilerServices.RuntimeHelpers.PrepareMethod(%methodHandle%);
```
**However**, you can use BenchmarkDotNet-style disassembler without any local CoreCLR, 
just enable it in "Settings/Use BDN disasm".

## Known Issues
* Only .NET Core Console applications are supported
* I only tested it for .NET Core 3.0 apps
* Multi-target projects are not supported
* Generic methods are not supported
* **Resharper** hides Roslyn actions by default (Uncheck "Do not show Visual Studio Light Bulb").

## 3rd party dependencies
* [MvvmLight](https://github.com/lbugnion/mvvmlight) (MIT)
* [AvalonEdit](https://github.com/icsharpcode/AvalonEdit) (MIT)
* [BenchmarkDotNet](https://github.com/dotnet/BenchmarkDotNet) (MIT)
* [ObjectLayoutInspector](https://github.com/SergeyTeplyakov/ObjectLayoutInspector) (MIT)
