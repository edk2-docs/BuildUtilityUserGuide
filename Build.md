EDK II Build.exe
================

Name
--------

```Build.exe``` – The master command that provides developers with a single 
command for selecting various options of a build

Synopsis
------------

``` ini
  Build.exe [Options] [Targets]
     Options:
            [-v | -q | -d] [-a <Arch>] [-p <DscFile>] [-m <InfFile>] [-b <Target>] [-t <TagName>] 
            [-f FdfFile] [-r <RomImageName>] [-i <FvImageName>] [-C <CapsuleImageName>] 
            [-n ThreadNum] [-x <SkuId>] [-u] [-e] [-w] [-j <LogFile>] [-s] [-D <MACROS>]
            [-y <ReportFile>] [-Y <ReportType>] [-F <Flags>] [--ignore-sources] [--check-usage]
            
     Targets:
            [All | GenC | GenMake | Fds | Libraries | Modules | Clean | CleanAll | CleanLib | run]

  Build.exe –h

  Build.exe --version
  
```

Description
---------------

Build.exe is the master command line (CLI) tool that provides a single command 
for selecting various build options. In general, it checks the environment 
variables, gets the user’s configuration from either the CLI or target.txt, 
parses the dsc, dec, inf, *target.txt*, *tools_def.txt*, generates .C and .H 
files and the Makefiles for one or more modules, calls a make (NMake or make) 
program to process these Makefiles, then optionally calls GenFds to generate an 
fd file.

The build tool supports two kinds of path specifications on command line - an 
absolutely path or a relative (to the WORKSPACE environment variable) path – in 
command line.

Options
-----------

There are no required options. If no options are specified, it uses options 
specified in target.txt.

```ini
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -a TARGETARCH, --arch=TARGETARCH
                        ARCHS is one of list: IA32, X64, IPF, ARM, AARCH64 or
                        EBC, which overrides target.txt's TARGET_ARCH
                        definition. To specify more archs, please repeat this
                        option.
  -p PLATFORMFILE, --platform=PLATFORMFILE
                        Build the platform specified by the DSC file name
                        argument, overriding target.txt's ACTIVE_PLATFORM
                        definition.
  -m MODULEFILE, --module=MODULEFILE
                        Build the module specified by the INF file name
                        argument.
  -b BUILDTARGET, --buildtarget=BUILDTARGET
                        Using the TARGET to build the platform, overriding
                        target.txt's TARGET definition.
  -t TOOLCHAIN, --tagname=TOOLCHAIN
                        Using the Tool Chain Tagname to build the platform,
                        overriding target.txt's TOOL_CHAIN_TAG definition.
  -x SKUID, --sku-id=SKUID
                        Using this name of SKU ID to build the platform,
                        overriding SKUID_IDENTIFIER in DSC file.
  -n THREADNUMBER       Build the platform using multi-threaded compiler. The
                        value overrides target.txt's
                        MAX_CONCURRENT_THREAD_NUMBER. Less than 2 will disable
                        multi-thread builds.
  -f FDFFILE, --fdf=FDFFILE
                        The name of the FDF file to use, which overrides the
                        setting in the DSC file.
  -r ROMIMAGE, --rom-image=ROMIMAGE
                        The name of FD to be generated. The name must be from
                        [FD] section in FDF file.
  -i FVIMAGE, --fv-image=FVIMAGE
                        The name of FV to be generated. The name must be from
                        [FV] section in FDF file.
  -C CAPNAME, --capsule-image=CAPNAME
                        The name of Capsule to be generated. The name must be
                        from [Capsule] section in FDF file.
  -u, --skip-autogen    Skip AutoGen step.
  -e, --re-parse        Re-parse all meta-data files.
  -c, --case-insensitive
                        Don't check case of file name.
  -w, --warning-as-error
                        Treat warning in tools as error.
  -j LOGFILE, --log=LOGFILE
                        Put log in specified file as well as on console.
  -s, --silent          Make use of silent mode of (n)make.
  -q, --quiet           Disable all messages except FATAL ERRORS.
  -v, --verbose         Turn on verbose output with informational messages
                        printed, including library instances selected, final
                        dependency expression, and warning messages, etc.
  -d DEBUG, --debug=DEBUG
                        Enable debug messages at specified level.
  -D MACROS, --define=MACROS
                        Macro: "Name [= Value]".
  -y REPORTFILE, --report-file=REPORTFILE
                        Create/overwrite the report to the specified filename.
  -Y REPORTTYPE, --report-type=REPORTTYPE
                        Flags that control the type of build report to
                        generate.  Must be one of: [PCD, LIBRARY, FLASH,
                        DEPEX, BUILD_FLAGS, FIXED_ADDRESS, EXECUTION_ORDER].
                        To specify more than one flag, repeat this option on
                        the command line and the default flag set is [PCD,
                        LIBRARY, FLASH, DEPEX, BUILD_FLAGS, FIXED_ADDRESS]
  -F FLAG, --flag=FLAG  Specify the specific option to parse EDK UNI file.
                        Must be one of: [-c, -s]. -c is for EDK framework UNI
                        file, and -s is for EDK UEFI UNI file. This option can
                        also be specified by setting *_*_*_BUILD_FLAGS in
                        [BuildOptions] section of platform DSC. If they are
                        both specified, this value will override the setting
                        in [BuildOptions] section of platform DSC.
  -N, --no-cache        Disable build cache mechanism
  --conf=CONFDIRECTORY  Specify the customized Conf directory.
  --check-usage         Check usage content of entries listed in INF file.
  --ignore-sources      Focus to a binary build and ignore all source files

```

Targets
----------

If no target is given, then default target is ALL.

**ALL**

  Build everything for either the platform or module.

**GenC**

  Auto-generate all C files for either the platform or module.

**GenMake**

  Generate the Makefiles – if auto-generated files are missing,
  then auto-generate all C files first for either the platform or module.

**Fds**

  Create the FD Image files.

**Libraries**

  Build all EDK Libraries and EDK II Library Instances which are specified.

**Modules**

  Build all EDK components and EDK II modules which are specified.

**Clean**

  Remove intermediate files generated by the NMAKE command (leaving the 
  auto-generated C format, FD image files, PE32 output files, PCH files and 
  LIB files).

**CleanAll**

  Remove all intermediate, auto-generated, FV and FD image files – state of 
  the tree should be exactly as if a fresh checkout or install has occurred.

**CleanLib** 

  Remove intermediate files generated by the NMAKE command AND LIB files 
  (leaving the auto-generated, FV and FD image files, PE32 output files and 
  PCH files)

**run** 

  Run platform (for emulator platform only)


Status codes returned
---------------------


| Value         | Description                            |
|---------------|----------------------------------------|
| ```0```       | The action was completed as requested. |
| ```Nonzero``` | The action failed.                     |



Examples
------------

Assume that $(WORKSPACE) is C:\\MyWorkspace

1. Build platform: Nt32Pkg.dsc

   Specify the platform description file on the command line.

  ```
    C:\\MyWorkspace> build -p Nt32Pkg\\Nt32Pkg.dsc –a IA32
  ```

2. Build the platform in the current working directory if it contains a platform description file.

  ```
    C:\\MyWorkspace\\Nt32Pkg> build –a IA32
  ```

3. Build the active platform specified in the target.txt file.

  ```
    C:\\ MyWorkspace> build –a Ia32
  ```

4. Build Module: HelloWorld.inf

  Specify the platform and Module on the command line.

  ```
    C:\\MyWorkspace> build -p Nt32Pkg\\Nt32Pkg.dsc –a IA32 –m MdeModulePkg/Application/HelloWorld/HelloWorld.inf
  ```

5. Specify the Module on the command line and use the active platform specified in the target.txt file.

  ```
    C:\\MyWorkspace> build –a IA32 –m MdeModulePkg/Application/HelloWorld/HelloWorld.inf
  ```

6. Build the module in the current working directory if it contains a module description file and
   specify the platform on the command line.

  ```
    C:\\ MyWorkspace\\MdeModulePkg\\Application\\HelloWorld> build –a Ia32 –p Nt32Pkg\\Nt32Pkg.dsc
  ```

7. Build the module in the current working directory and use the active platform specified in the
   target.txt file.

  ```
    C:\\ MyWorkspace\\MdeModulePkg\\Application\\HelloWorld> build –a Ia32
  ```

Bugs
---------

No known issues.

Report bugs to [edk2-devel@lists.01.org](mailto://edk2-devel@lists.01.org)

Files
----------

*target.txt*, *tools_def.txt*, platform.dsc, flashmap.fdf, package.dec and module.inf.

See also
------------

GenFds.exe*

License
-----------

Copyright (c) 1999 - 2016, Intel Corporation. All rights reserved.

This program and the accompanying materials are licensed and made available under the terms and
conditions of the BSD License which accompanies this distribution. The full text of the license may be
found at:

  http://opensource.org/licenses/bsd-license.php

THE PROGRAM IS DISTRIBUTED UNDER THE BSD LICENSE ON AN "AS IS" BASIS, WITHOUT WARRANTIES
OR REPRESENTATIONS OF ANY KIND, EITHER EXPRESS OR IMPLIED.
