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

##⚠️ Current State
* Currently only english (ENU) and german (DEU) captions are set.
* The Shared Access Key is NOT stored in the isolated storage of NAV, but directly in the table.

# Setup
Check the following documentations for 
* [Frontend Documentation](<doc/Frontend Documentation.md>)
* [Technical Documentation](<doc/Technical Documentation.md>)

## NAV-Objects

 :warning:
 **Importing Textfiles will overwrite existing objects without confirmation!** :warning:

Check your license before importing the objects. If the number range is already occupied, the NAV object must be renumbered manually. In this case download the txt-Files and edit them via TextEditor.

**Don't forget to update the references!**