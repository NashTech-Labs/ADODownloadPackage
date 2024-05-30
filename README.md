# Azure_Container_Build_Task.ado

Use this Azure template to download a package from a package management feed in Azure Artifacts.

## Parameters

The pipeline requires the following parameters to be defined:


| Name  | Displayname | type | Default | Values | Opional/Required | Comments |
| ------------- | ------------- | :-------------: | :-------------: | ------------- | :-------------: | ------------- |
| packageType | Package type | string | nuget |'maven'/'npm' / 'nuget'/ 'pypi'/'upack'/'cargo' | Required | Allowed values: maven, npm, nuget, pypi (Python), upack (Universal), cargo|
| feed | Artifact feed on Azure Artifactory | string | | | Required | For project-scoped feeds, the format is `projectID/feedID`.  |
| view  | view or type of feed| string | Local | Local/Prerelease/Release | OPtional | Specifies a view that only uses versions promoted to that specific view. |
| definition | defined id of package  | string |  |  | Required | Can also be package name |
| version  | pecifies the version of the package. | string |  | | Required |  Use `latest` to download the latest version of the package at runtime. |
| files | Specifies which files to download using [file matching patterns](https://go.microsoft.com/fwlink/?linkid=2086953). | string | `**` | | Optional | when `packageType = maven / pypi / upack |
| extract | Extracts the package contents and contains the package archive in the artifact folder. | boolean |true| true/false | Optional | when `packageType = nuget / npm |

--------------------------------------------------------------------------------------------------------------------------------------------------

These parameters provide multiple use case options for the template, enable/disable flags for the utilization of different templates as per the requirements.

## Use Cases

You can directly call a particular template as per the requirement. for example: 

  ```yaml
  # azure-pipeline.yml
  resources:
  repositories:
    - repository: Template
      type: github
      name: your_username/Azure_Container_Build_Task.ado
      ref: <respective branch name>
      endpoint: 'githubServiceConnectioNname'

  steps:
  # passing the parameters
  - template: ADO_Download_Package.yml@Template
    parameters:
      packageType: 'nuget'      
      feed: '<Azure Artifact Feed>'     
      view: 'Local'    
      definition: com.test:testpackage 
      downloadPath: '$(System.ArtifactsDirectory)'     
      version: '${{parameters.version}}'   
        
  
Make sure to adjust the repository name, branch name, and parameter values according to your project's requirements.