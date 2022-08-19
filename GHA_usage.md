# Power Platform Lifecycle management using Github actions

the goals are 
 - to check in the the source / Power FX code to the solution in order to 
 - detect and manage changes in the code 
 - allow testing across multiple environments
 - test the appl by deploying it to a test invironment

in order to this we will use the [PowerPlatform Github actions](https://github.com/microsoft/powerplatform-actions) to automate the process of 
 - retrieving the solution form a development environment 
 - extracting the code from the solution(s), 
   - including the PowerApp definition
 - checking in the code to the repository ( Dev branch by default)
 - building managed solution packages 
 - deploying the packages to the test environment

possible future steps:
 - testing the code for best practices and coding style
 - scan for and remove secrets from the code


## Environments (dev, test)
------------------------

Naming convention:
    Dev-xxxx    development environments 
    Test-xxxx   test environments


The Power Platform environments are created in one or multiple AAD tenants
These Power Platform environments are used to to develop and to test the application in different environments by different people

Each PP_environemnts needs to be defined in GH as an environnment ( repo > settings > environment) and per environment the environment specific infomration is provided using environment secrets.
In order to automatically deploy the application to/from an environment the environment needs to have a CDS database configured.
The type of database does not matter ( sandbox, developer, trial , productions, pay as you go ) 

Also permission need to be granted to the AAD Application user to access the database 
- aka.ms/ppac > environments > [select environment] > settings > users and permissions > Application users 
  - + Setup app user
  - select App : TACO PowerApp ALM
  - org : select the org 
  - Role : select either `System Customizer`  or `System Administrator`


FOr each environment, it must be created and the following secret must be defined 
   * CSD_ENVIRONMENT_URL    : the url of the CDS environment (aka.ms/ppac > ) https://taco-test.crm4.dynamics.com/

On the repo level the the definint of the AAD appliaction used for the deployment must be defined, using 4 sectets :

 * APP_ID           : the application GUID of the AAD application used for the deployment 
 * APP_SECRET       : the application secret of the AAD application used for the deployment
 * APP_TENANTID     : the tenant id of the AAD application used for the deployment

in order to be able to create commits, and have these commits trigger other GH worflows, a Persona Access token must be provided.

* MY_GITHUB_TOKEN : the github token of the persona used to check in the code to the repo


# Workflows 

## Commit.yml (Manual) 
  Check in the code for a solution to the repo
  - Inputs 
    - source environment ( select from environments)
    - destination branch ( )
    - Solution name ( dropdown)
    - type of sulution to export ( Unmanaged, or Both managed and unmanaged)
  
  - Steps 
    - test connection to the environment
    - export the solution from the environment
    - unpacked the solution
    - check it in to the `Dev` branch

## Ping.yml (Manual) 
  Check if it is possible to access a PP environment
  - Inputs 
    -  environment ( select from environments)

 - Steps 
    - test connection to the environment




TODO:
=====

 how to hande deployment in a multi-tenant environment ?

TODO Replace default values 

 -  "defaultValue": "4611111L0sFAGiIY3qMb5aAehPGpIujhc3Kjx51AzFuh4-uwQ==", --> remove 
 -    "defaultValue": "https://m365x14050590.sharepoint.com/sites/AACQMgt", --> contaco.sharepoint.com
