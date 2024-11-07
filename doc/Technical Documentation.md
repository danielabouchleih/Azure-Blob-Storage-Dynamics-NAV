# Technical Documentation

## .net Library
Copy the dll-File located in the lib-Folder into the Addins-Folder of the ServiceTier and RoleTrailored Client (preferably create a new Folder called AzureBlobStorage for the sake of order)
* Service Tier: C:\Program Files\Microsoft Dynamics 365 Business Central\140\RoleTailored Client\Add-ins\AzureBlobStorage
* RTC: C:\Program Files\Microsoft Dynamics 365 Business Central\140\Service\Add-ins\AzureBlobStorage

Source of Code: https://github.com/Azure-Samples/storage-dotnet-rest-api-with-auth
Including bugfixes for following issues:
* https://github.com/Azure-Samples/storage-dotnet-rest-api-with-auth/issues/16
* https://github.com/Azure-Samples/storage-dotnet-rest-api-with-auth/issues/21


## Setup Azure Blob Storage Accounts
Check [Setup Azure Blob Storage Accounts](<Frontend Documentation.md>) for information regarding setup

## Codeunit Azure Blob Storage Mgmt

The Azure Blob Storage Mgmt Codeunit with ID 50000 is the core of the Azure Blob Storage connection and provides all the functions required to handle containers and BLOBs.
Therefore, all functions are parameterized to pass in all necessary information and receive the necessary return values as call-by-reference parameters. If data records are returned, they are always stored in temporary records.

# Examples

## Get all Containers for a storage account
Define the following variables. Make sure tempABSContainer is set to Temporary = Yes
```
Name	DataType	Subtype	Length
AzureBlobStorageAccount	Record	Azure Blob Storage Account	
tempABSContainer	Record	ABS Container	
```

```
AzureBlobStorageAccount.GET('TEST'); //TEST needs to be replaced with the code of your account setup
ACEAzureBlobStorageMgmt.GetContainers(AzureBlobStorageAccount, tempABSContainer);
//Now tempABSContainers holds all Containers for the selected storage account. 
//You can now use them as any usual Record in NAV. In my Example I show them as a list to the user
PAGE.RUN(0, tempABSContainer);
```

## Create a new container for a storage account

Define the following variables and text constants
```
Name	DataType	Subtype	Length
AzureBlobStorageAccount	Record	Azure Blob Storage Account	
NewContainerName	Text	

Name	ConstValue
ContainerCreatedInfo	Container %1 was created successfully
```

```
ACEAzureBlobStorageMgmt.GET('Test');
ACEAzureBlobStorageMgmt.CreateContainer(AzureBlobStorageAccount, NewContainerName);

IF GUIALLOWED THEN
  MESSAGE(ContainerCreatedInfo, NewContainerName);
```

## Get a list of BLOBs/Files within a container of a storage account
Define the following variables. Make sure tempABSContainerContent is set to Temporary = Yes
```
Name	DataType	Subtype	Length
AzureBlobStorageAccount	Record	Azure Blob Storage Account	
ContainerName	Text
tempABSContainerContent	Record	ABS Container Content	
```

In the following example the content is shown to end user in a list page.
```
AzureBlobStorageAccount.GET('TEST');
ContainerName := 'TestContainer';
ACEAzureBlobStorageMgmt.GetContent(AzureBlobStorageAccount, ContainerName, tempABSContainerContent, '', '');
PAGE.RUN(0, tempABSContainerContent);
```

## Download a specific BLOB/File
To download a specific or multiple BLOBs/Files the tempABSContainerContent from the above example needs to be used again.

```
IF tempABSContainerContent.FINDFIRST THEN
    ACEAzureBlobStorageMgmt.DownloadFile(AzureBlobStorageAccount, tempABSContainerContent);

//Now a Open or Save-Dialog will be shown
```

## Upload a file to a container
To finally upload a new file to the Azure Blob Storage Container the following code can be used:

```
Name	DataType	Subtype	Length
AzureBlobStorageAccount	Record	Azure Blob Storage Account
ContainerName	Text
LocalFilePath	Text		
FileManagement	Codeunit	File Management	
TempBlob	Record	TempBlob	
FileInStream	InStream		
ABSFilePath	Text		
FileNameWithExtension	Text		
```


```

AzureBlobStorageAccount.GET('TEST');
TempBlob.Blob.CREATEINSTREAM(FileInStream);
ABSFilePath := 'Directory\Subdirectory' //Leave empty to upload into root directory

LocalFilePath := FileManagement.UploadFile('Upload file', LocalFilePath);
FileManagement.BLOBImportFromServerFile(TempBlob, LocalFilePath);
FileNameWithExtension := FileManagement.GetFileName(LocalFilePath);

IF ABSFilePath <> '' THEN
  ABSFilePath := ABSFilePath + '\' + FileNameWithExtension
ELSE
  ABSFilePath := FileNameWithExtension;

ACEAzureBlobStorageMgmt.UploadFileInStream(Rec, ABSFilePath, FileInStream);
```