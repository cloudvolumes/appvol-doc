# Developer notes for Manager release 2.10

## Notable Changes:

* AVM_PROTECT_VOLUMES="1" operating system environment variable can be used to enable vMotion and to provide volumes
accidental deletion protection when all ESX hosts are version 6u1 (ESX 6u1 requires vCenter 6 or later). vMotion of
VMDK files is not supported.

* All templates are now created with "attributes volume set NODEFAULTDRIVELETTER" to ensure they are not automatically assigned a letter by a Windows Mountvol

* Computer identity is now verified against AD, so computer that were formerly trusted by Active Directory that have lost their trust relationship will no longer get volumes. This can happen due to default maximum machine password age of 90 days in Windows when reverting virtual machines to a snapshot after its machine password changed. See "Domain member: Disable machine account password changes" (https://technet.microsoft.com/en-us/library/jj852191.aspx) for instructions on how to disable this.

* Due to significant significant improvemsnts in the App Volumes Manager in v2.10, App Volumes Broker Integration Agent is no longer needed and is also not recommended.


## No attachment to Low Performance Storage

Now storage can be marked as "Not Attachable" making manager skip it when using it for mounting volumes.  This allows IT
admins to better manage storage capabilities.

For example, an IT admin can setup two vCenters each with a local storage and one shared storage.  The slower performing
shared storage can be marked "Not Attachable" and be used solely for replication.


## Expand the size of a writable volume

Admin can specify a new size for a Writable Volume (must be at least >1 gig larger than current size). The manager will
grow the volume file to the new size.  Next time the user logs into a VM with App Volumes Agent, it will automatically
grow the file system of the volume.