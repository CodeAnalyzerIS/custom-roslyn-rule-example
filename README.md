# Custom Roslyn Rule Example

In this repository you can find an example for a custom Roslyn Rule which can be added to the Code Analyzer Tool.  

## How to create your own custom rule?

To create your own custom rule, you have to install the following nuget package to your project:  
https://www.nuget.org/packages/CodeAnalyzerTool.RoslynPlugin.API  

This nuget package contains an API with an abstract class called `RoslynRule` which should be inherited from in your custom rule class.  
The abstract `RoslynRule` class inherits from the `DiagnosticAnalyzer` abstract class that is created by the Roslyn API.  
We added 3 new properties to the `RoslynRule` class that need to be implemented to be compatible with our tool:
- DiagnosticId
- Options
- Severity

All these properties are added for the customization through the config file.  
The DiagnosticId property indicates which ruleName should be used in the config file to configure and enable the rule.  

To initialise the Options, Severity, and SupportedDiagnostic properties, we suggest creating a default constructor which initialises these properties.  
Other than our custom properties, you can follow the DiagnosticAnalyzer guidelines

## How to use/install the custom plugin

The first important step is to edit the `.csproj` of the project to allow for the dependencies to be generated as `.dll`'s as well.  
This can be accomplished by doing the following:  
Add the following line to the PropertyGroup  
```
<EnableDynamicLoading>true</EnableDynamicLoading>
```
The PropertyGroup could look something like this:  
```
<PropertyGroup>
    <TargetFramework>net7.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
    <EnableDynamicLoading>true</EnableDynamicLoading>
</PropertyGroup>
```
If you don't want the project to create unnessecary dependencies (which are already implemented in the source code), 
you can check the source code dependencies and add the following line to the ones that are already available and you don't want to be generated again:  
```
<ExcludeAssets>runtime</ExcludeAssets>
```
Your package reference could look like this:  
```
<PackageReference Include="CodeAnalyzerTool.RoslynPlugin.API" Version="0.0.5" >
    <ExcludeAssets>runtime</ExcludeAssets>
</PackageReference>
```
  
After you have done these configurations, you can build the project but make sure the `.csproj` changes were saved.  

Once the files were generated, go to the directory where they were generated and copy them all.  
Then paste the files in the plugin folder which is specified by the configuration file (more information on that can be found here: https://github.com/CodeAnalyzerIS/code-analyzer-tool/wiki/Configuration)  
Our example for the directory structure was:  
rootOfRepository/CAT/Roslyn/rules/LicenseCheck/
  
IMPORTANT:  
Make sure to use the 'rules' folder in the Roslyn plugin folder and then use the name specified in the rule as `DiagnosticId` as foldername for the rule files.  

If you run the tool now, the custom rule should be implemented by the tool.
