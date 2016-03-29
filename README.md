VstoAddinInstaller
===================

[InnoSetup][] script to install and activate Visual Studio Tools for 
Office&reg; (VSTO) addins. VstoAddinInstaller makes heavy use of the 
InnoSetup Preprocessor (ISPP).

Features
--------

- Installs Word and Excel add-ins. (Can be expanded to deal with 
  add-ins for other Office application fairly easily, but I haven't 
  had time to do it and test it yet.)
- Downloads and installs .NET and VSTO runtimes if needed.
- Checks if Excel or Word is running and can automatically shut it 
  down before proceeding with the installation process.
- Can be used with an `/UPDATE` switch to silently shut down and 
  restart Excel or Word after the installation.
- Modular structure makes it easy to keep custom configuration 
  separate from the core functionality.

The script is based on the installer used by [Daniel's XL Toolbox][].


Requirements
------------

- [InnoSetup][] with InnoSetup Preprocessor; ideally you would obtain 
  the InnoSetup QuickStart Pack.
- [InnoSetup Download Plugin][isdp] (takes care of downloading the 
  .NET and VSTO runtime installers on the target system, if needed)
- Visual Studio with Visual Studio Tools for Office (VSTO) (included 
  in the 'Professional' editions).

Some of these requirements should be fairly obvious...


Instructions for use
--------------------

VstoAddinInstaller is designed to keep itself out of your way: After 
downloading and extracting the [archive][zip] (or cloning the [Git][] 
repository), you should have a dedicated folder 'VstoAddinInstaller' 
in your solution directory. You won't have to edit anything inside the 
VstoAddinInstaller directory. A configuration file can be copied from 
an example file `make-installer.dist.iss` from the `config-dist` 
subdirectory to your solution's directory, where you may want to 
include it in your own version control system.

A recommended folder structure is as follows:

        YourSolutionFolder
        ├── deploy                  <-- custom folder, name it whatever you like
        │   ├── make-installer.iss  <-- your custom config file for VstoAddinInstaller
        │   ├── VstoAddinInstaller  <-- VstoAddinInstaller, e.g. as Git submodule
        │   │   ├── config-dist
        │   │   │   ├── make-installer.dist.iss  <-- template for your make-installer.iss
        │   │   │   └── ...
        │   │   └── ...
        │   ├── setup-files         <-- additional files, such as a license file
        │   │   └── ...
        │   └── releases            <-- where the installer will be written to
        │       └── ...
        ├── YourProject
        │   ├── bin
        │   │   ├── Debug
        │   │   │   ├── YourProject.vsto   <-- generated by Visual Studio
        │   │   │   └── ...
        │   │   └── Release
        │   │       ├── YourProject.vsto   <-- generated by Visual Studio
        │   │       └── ...
        │   ├── YourProject.csproj
        │   └── ...
        ├── VERSION.TXT             <-- text file containing the version number (optional)
        └── ...


### Cloning as Git submodule

If you use [Git][] as your version control, you can clone the 
VstoAddinInstaller respository into a submodule of your project 
repository:

    git submodule add git@github.com:bovender/VstoAddinInstaller

You can then easily update the VstoAddinInstaller module by changing 
into the directory and issuing

    git pull

Because the entire VstoAddinInstaller repository is contained in the 
submodule, you can switch between branches and use the `develop` 
branch rather than the `master` branch if you wish to do so.


### Configuring

You need to make a copy of `config-dist\make-installer.dist.iss` and 
edit the contents. Ideally you would keep your copy of this file 
outside the `VstoAddinInstaller` directory, as shown above, so it does 
not get overwritten when you update VstoAddinInstaller.

You can rename the file to `make-installer.iss` or whatever you wish; 
the `.dist` part in the original file only serves to indicate that 
this is the distributed example file.

Follow along the instructions in your copy of 
`make-installer.dist.iss`.

**Important**: Each installer produced with InnoSetup must have its 
own global unique identifier (GUID). Please insert your own GUID where 
indicated. You can generate a GUID using the corresponding command 
from the `Tools` menu in InnoSetup Studio, for example.


### Compiling the installer

To compile the installer, simply run your copy of `make-installer.iss` 
through InnoSetup.


### Extending

If you wish to add additional capabilities to your installer (e.g. 
InnoSetup tasks), have a look in the `config-dist` directory where you 
will find some example files.

Currently it is not possible to run additonal code during 
initialization of setup (`InitializeSetup`) or initialization of the 
wizard (`InitializeWizard`). I have been thinking about how to provide 
this capability, but have not yet have the time to implement it.


Demos
-----

Two demo projects are contained in this repository in the `demo` 
directory, one for an Excel add-in and one for a Word add-in (2010 or 
newer).

In order to try out these demo installers, you must first build the 
projects in Visual Studio. The binaries are not contained in this 
repository.


Further information
-------------------

For background information on the prerequisites of a VSTO add-in, see
<http://xltoolbox.net/blog/2015-01-30-net-vsto-add-ins-getting-prerequisites-right.html>.


Notice
------

This script is based on the related [ExcelAddinInstaller][].


License
-------

Published under the [Apache License, Version 2.0](LICENSE).

        Copyright (C) 2016 Daniel Kraus (bovender) <http://github.com/bovender>

        Licensed under the Apache License, Version 2.0 (the "License");
        you may not use this file except in compliance with the License.
        You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

        Unless required by applicable law or agreed to in writing, software
        distributed under the License is distributed on an "AS IS" BASIS,
        WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
        See the License for the specific language governing permissions and
        limitations under the License.

Microsoft®, Windows®, Office, and Excel® are either registered
trademarks or trademarks of Microsoft Corporation in the United States
and/or other countries.


[InnoSetup]: http://www.jrsoftware.org/isinfo.php
[isdp]: http://mitrichsoftware.wordpress.com 
[Daniel's XL Toolbox]: http://xltoolbox.net
[ZIP]: https://github.com/bovender/VstoAddinInstaller/archive/master.zip
[Git]: http://git-scm.com/downloads

<!-- vim: set tw=72 ts=4 :-->
