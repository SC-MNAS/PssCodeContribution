# Setup Environment for PSS Direct Code Contribution
This repository is meant to pave the way for PSS engineers and provide them with collaboration on the required steps in order to setup the local environment to start PSS Direct Code Contribution.

The following steps are supposed to provide a smooth setup process. Your help in pointing out the issues and improving the steps are highly appreciated.

# Steps to work through 
### 1. PSS Direct Code Contribution
Have a quick look at [PSS Direct Code Contribution](http://thecore.dk.sitecore.net/Enablement/PSS/PSS-PD-Collaboration/PSS-Direct-Code-Contribution.aspx) article on the core to get acquaintance with the process in general.

### 2. Legoland
Review [Sitecore Nuget (Legoland, ps.ads)](http://thecore.dk.sitecore.net/Building-our-products/How%20we%20develop%20our%20products/Development%20guides/Sitecore%20Nuget%20(Legoland,%20ps,-d-,ads).aspx) 
to get some insight into the subject. 
> No need to follow any steps in the article.

### 3. Install Resharper
[Resharper](https://www.jetbrains.com/resharper/) is a Visual Studio Extension and is an essential tool for [Sitecore Code Quality Model](http://thecore.dk.sitecore.net/Building-our-products/How%20we%20develop%20our%20products/Development%20guides/Code%20style%20and%20configuration.aspx).

### 4. Install Sitecore.CodeQuality
Install [Sitecore.CodeQuality 1.5.0](http://nuget1dk1:8181/feeds/tools/Sitecore.CodeQuality/1.5.0) which is a set of project tools to assist with employing the Sitecore Code Quality Model.
### 5. Install GhostDoc
[GhostDoc](http://submain.com/products/ghostdoc.aspx) is a Visual Studio extension that automatically generates XML documentation comments for methods and properties based on their type, parameters, name, and other contextual information.

### 6. Install Chocolatey
We use [Chocolatey](https://chocolatey.org/) a package manager for Windows to easily manage installation, configuration, upgrade, and uninstallation of Softwares and modules.

### 7. Install nuget.commandline Nuget Package 
install the package via executing the following command in command prompt as an administrator.
```ps1
choco install nuget.commandline -version 2.8.6
```
> **Note** If you have already installed a later version of the package it is necessary to remove it and use this verion.
```ps1
choco uninstall nuget.commandline
```    
### 8. Install ps.ads
Execute the following command in command prompt as an administrator
```ps1
choco install ps.ads -Source http://nuget1dk1:8181/nuget/Packages -y
```
The [Installing ps.ads](https://sitecore1.sharepoint.com/sites/DevOps/_layouts/15/WopiFrame.aspx?sourcedoc={2b6af397-1218-4118-b152-8051ee2083db}&action=view&wd=target%28%2F%2FKnowledge%20base.one%7C5fe94501-41a6-4d1f-b475-9c2c8c7381e0%2FInstalling%20ps.ads%7Ca16ea03f-4aa4-42a0-8046-cfd2105c8174%2F%29) article provides more details on the installation process should the need arise.

### 9. Clone the Source code 
Source code is located [here](http://tfs4dk1/tfs/pd-products-01/Products/_git/Foundation.Content) 
You can use Visual studio or SourceTree as per your convenience to clone the source code to your local machine.

### 10. Check out the /release/8.2/update4 branch
The operation again can be executed using either Visual Studio or SourceTree.

### 11. Edit the environment-specific data
#### 11.1. Open the buildscript_tc/tg.init.ps1 file and set the following parameters according to your local environment setup:
```ps1
  [Parameter(Mandatory=$false)][string] $instanceName = "sc82",
  [Parameter(Mandatory=$false)][string] $sqlServerName = ".",
  [Parameter(Mandatory=$false)][string] $dbUser = "sa",
  [Parameter(Mandatory=$false)][string] $dbPassword = "sitecore",
```
#### 11.2. Edit the following properties in buildscript_tc/environment.include file 
```ps1
  <property name="env.sql2008.server" value="."/>
  <property name="env.sql2008.security" value="integrated security=true"/>
```
#### 11.3. Also buildscript_tc/localbuild.ps1 file needs proper configuration
```ps1
  [Parameter(Mandatory=$false)][string] $instanceName = "sc82",
  [Parameter(Mandatory=$false)][string] $sqlServerName = ".",
```
### 12. Running SQL Server Service

   Make sure the **SQL Server (MSSQLSERVER)** service is up and running in the services.msc.

### 13. Manual Build 
Open Sitecore solution located in \code\Sitecore\Sitecore.sln and build the **Sitecore.Client** project.
This manual build is a workaround to avoid some issues when you do not have Visual Studio 2012 Update 2 installed.  
Alternatively, you can install Visual Studio 2012 Update2 which I believe the consumed resources does not worth the one-time usage.

### 14. initialize_sandbox.bat
Open a command prompt window **as an administrator** and navigate to the cloned repository local folder and execute the initialize_sandbox.bat file.

This is the main step and the majority of the previous steps were meant to make this step get executed faultlessly.
- Some warnings with yellow color will be logged that mostly can be ignored for instance those related to _approved verbs_.  
- Some errors with red color related to the project target and not found files can also be ignored due to performing the manual build in step 13. 
- If errors related to IIS website such as _Cannot find the drive. The drive called 'IIS' does not exist._ logged it can be attributed to non-administrator shell.
- If errors related to install-sitecore-package such as _Unknown command: install-sitecore-package_ logged most probably it is due to using the wrong Nuget.Commandline version.
- If in the final step (Deserialize-Database) some errors logged it is mostly because of not performing the project build in step 13. 
### 15. Verification 
Rebuild the entire sitecore solution to make sure everything is alright.


> Have fun developing and reduce the future issues effectively.
