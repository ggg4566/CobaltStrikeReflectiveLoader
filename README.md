# Cobalt Strike User-Defined Reflective Loader
Cobalt Strike User-Defined Reflective Loader written in Assembly & C for advanced evasion capabilities.

![](/images/bobsBeacon.png)

+ Based on Stephen Fewer's incredible Reflective Loader project: 
  + https://github.com/stephenfewer/ReflectiveDLLInjection
+ Created while working through Renz0h's Reflective DLL videos from the [Sektor7 Malware Developer Intermediate (MDI) Course](https://institute.sektor7.net/courses/rto-maldev-intermediate/) 

## Initial Project Goals
+ Learn how Reflective Loader works.
+ Write a Reflective Loader in Assembly.
+ Compatible with Cobalt Strike.
+ Cross compile from macOS/Linux.
+ Implement Inline-Assembly into a C project.

## Future Project Goals
+ Use the initial project as a template for more advanced evasion techniques leveraging the flexibility of Assembly.
+ Implement Cobalt Strike options such as no RWX, stompPE, module stomping, changing the MZ header, etc.
+ Write a decent Aggressor script.
+ Support x86.
+ Have different versions of reflective loader to choose from.
+ Implement HellsGate/HalosGate for the initial calls that reflective loader uses (pNtFlushInstructionCache, VirtualAlloc, GetProcAddress, LoadLibraryA, etc).
+ Optimize the assembly code.
+ Hash/obfuscate strings.
+ Some kind of template language overlay that can modify/randomize the registers/methods.

## Usage
1. Start your Cobalt Strike Team Server with or without a profile
  + At the moment I've only tested without a profile and with a few profiles generated from [Tylous's epic SourcePoint project](https://github.com/Tylous/SourcePoint)
```bash
#### This profile stuff below is optional, but this is the profile I tested this Reflective Loader with ####
# Install Go on Kali if you need it
sudo apt install golang-go -y
# Creating a Team Server Cobalt Strike profile with SourcePoint
## Clone the SourcePoint project
git clone https://github.com/Tylous/SourcePoint.git
## Build SourcePoint Go project
cd SourcePoint
go build SourcePoint.go
## Run it with some cool flags (look at the help menu for more info)
### This is the settings I have tested UD Reflective Loader with
./SourcePoint -PE_Clone 18 -PostEX_Name 13 -Sleep 3 -Profile 4 -Outfile myprofile.profile -Host <TeamServer> -Injector NtMapViewOfSection
## Start Team Server
cd ../
sudo ./teamserver  <TeamServer> 'T3@Ms3Rv3Rp@$$w0RD' SourcePoint/myprofile.profile
```
2. Go to your Cobalt Strike GUI and import the rdll_loader.cna Agressor script
![](/images/loadRdllScriptMenu.png)
3. Generate your x64 payload (Attacks -> Packages -> Windows Executable (S))
![](/images/CreateBeaconStageless.png)

## Build (Only tested from macOS at the moment)
1. Run the compile-x64.sh shell script after installling required dependencies
```bash
# Install brew on macOS if you need it (https://brew.sh/)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
# Install Ming using Brew
brew install mingw-w64
# Clone this Reflective DLL project from this github repo
git clone https://github.com/boku7/CobaltStrikeReflectiveLoader.git
# Compile the ReflectiveLoader Object file
cd CobaltStrikeReflectiveLoader/
cat compile-x64.sh
x86_64-w64-mingw32-gcc -c ReflectiveLoader.c -o ./bin/ReflectiveLoader.x64.o -shared -masm=intel
bash compile-x64.sh
```
2. Follow "Usage" instructions

## Credits / References
### Reflective Loader
+ https://github.com/stephenfewer/ReflectiveDLLInjection
+ 100% recommend these videos if you're interested in Reflective DLL:  
  + [Dancing with Import Address Table (IAT) - Sektor 7 MDI Course](https://institute.sektor7.net/courses/rto-maldev-intermediate/463262-pe-madness/1435207-dancing-with-iat)
  + [Walking through Export Address Table - Sektor 7 MDI Course](https://institute.sektor7.net/courses/rto-maldev-intermediate/463262-pe-madness/1435189-walking-through-export-address-table)
  + [Reflective Injection Explained - Sektor 7 MDI Course](https://institute.sektor7.net/courses/rto-maldev-intermediate/463258-reflective-dlls/1435355-reflective-injection-explained)
  + [ReflectiveLoader source review - Sektor 7 MDI Course](https://institute.sektor7.net/courses/rto-maldev-intermediate/463258-reflective-dlls/1435383-reflectiveloader-source-review)
### Cobalt Strike User Defined Reflective Loader
+ https://www.cobaltstrike.com/help-user-defined-reflective-loader
### Great Resource for learning Intel ASM
+ [Pentester Academy - SLAE64](https://www.pentesteracademy.com/course?id=7)
### Implementing ASM in C Code with GCC
+ https://outflank.nl/blog/2020/12/26/direct-syscalls-in-beacon-object-files/
+ https://www.cs.uaf.edu/2011/fall/cs301/lecture/10_12_asm_c.html
+ http://gcc.gnu.org/onlinedocs/gcc-4.0.2/gcc/Extended-Asm.html#Extended-Asm
