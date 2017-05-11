# Developer notes for Manager release 2.6

## Notable Changes:

* AppStack Grouping - Volume files (vmdk/vhd) on different storage locations that share a path and filename are
  considered to be copies of each other. They can be managed as a single AppStack in the UI.

* Automatic AppStack Import - Volumes files (vmdk/vhd) located at the default appstack path on storage locations in
  enabled StorageGroup's are automatically imported if the setting is enabled when creating the storage group.

* Automatic AppStack Replication - Volumes files (vmdk) located at the default appstack path on storage locations in
  enabled StorageGroup's are automatically replicated to all datastores in the storage group if the setting is enabled
  when creating the storage group.

* Improve internationalization support - Non-ascii text characters are now handled properly in the UI and database.
  Strings containing UTF-8 characters from previous versions may not appear correctly.

* Users can now receive volumes when they login to computers running a server version of Windows.
  Previously, only volumes were only attached to standard desktop versions.

## Fixes:

* Users, Computers, Groups, and Organizational Units can be unassigned even when they are not found in AD.

* There might have been casees in v2.5.2 where the option to overwrite the database checkbox did not work properly

* When uninstalling the App Volumes Manager, attached volumes are no longer automically detached. There is now a checkbox 
  during uninstallation that can be set if this a permanent uninstall. If the intent is to temporarily uninstall the
  App Volumes Manager or upgrade to a newer version, this checkbox should be left unchecked.
  

## AppStack Grouping

Files available at the same path and filename on different datastores are considered to be copies of each other.
Each AppStack now has a Locations count and pop-up display to show the child files. 
When mounting, only 1 file for each AppStack will be attached.
This is determined by whether or not the target VM can access the storage.
When multiple files are eligible, the file with the least amount of active attachments (by other users) is used.
When viewing attachments, the storage location of the attached file is shown in brackets: "AppStack-Name[datastore1]".

## Automatic AppStack Import

There is a new checkbox available when creating a StorageGroup called "Automatically Import AppStacks". 
When enabled, all storage locations in the group are checked for new volumes every 15 minutes.
This is intended to be combined with AppStack Grouping so that new volume files are automatically imported and associated with their primary AppStack for scale-out use-cases.

## Automatic AppStack Import

There is a new checkbox available when creating a StorageGroup called "Automatically Replicate AppStacks". 
When enabled, any AppStacks on one StorageGroup are replicated to the other datastores in the StorageGroup.
This is intended to be combined with AppStack Grouping so that new volume files are automatically imported and replicated for scale-out use-cases.

## Improve internationalization support

Due to a configuration error, strings containing UTF-8 input in previous versions were stored as single-byte ASCII characters in the database. This problem has been fixed going forward, but old strings may appear garbled.

## Server VDI

Users can now receive volumes when they login to computers running a server version of Windows