# Azure Blob Storage for Dynamics NAV
This repository will provide a C/AL written .net-based access to Azure Blob Storages via Rest API Calls

The main functions are:
1. List containers for a storage account
2. Create containers
3. Delete containers
4. List files/blobs in a container
5. Download files/blobs
6. Upload files/blobs
7. Delete files/blob

# Setup

## .net Library
Copy the dll-File located in the lib-Folder into the Addins-Folder of the ServiceTier and RoleTrailored Client (preferably create a new Folder called AzureBlobStorage for the sake of order)
* Service Tier: C:\Program Files\Microsoft Dynamics 365 Business Central\140\RoleTailored Client\Add-ins\AzureBlobStorage
* RTC: C:\Program Files\Microsoft Dynamics 365 Business Central\140\Service\Add-ins\AzureBlobStorage

Source of Code: https://github.com/Azure-Samples/storage-dotnet-rest-api-with-auth
Including bugfixes for following issues:
* https://github.com/Azure-Samples/storage-dotnet-rest-api-with-auth/issues/16
* https://github.com/Azure-Samples/storage-dotnet-rest-api-with-auth/issues/21

## NAV-Objects
Check your license before importing the objects. If the number range is already occupied, the NAV object must be renumbered manually. In this case download the txt-Files and edit them via TextEditor.
Don't forget to update the references!
