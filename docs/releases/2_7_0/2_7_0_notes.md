# Developer notes for Manager release 2.7

## Notable Changes:
* Removed "Spillover" from the available Storage Group distribution options
  This strategy will fill up a storage because it uses actual (thin) size and not the full provisioned size

* An additional set of credentials can be provided to be used when connecting to any trusted ActiveDirectory domain
  This enables use of additional domains that have one-way trust relationships (see documentation for more info)

* The API route used to delete assignments has changed from POST /cv_api/assignments to DELETE /cv_api/assignments

* When configuring direct-to-host mounting using the vCenter, you will no longer be prevented from saving the
  ESX credentials if they are not valid (or not able to be validated) on all hosts visible to vCenter.
  This fixes a case when an ESX server may be down for maintenance.

* An error will be shown in System Messages when the Agent fails to attach a VHD when the Manager is configured
  to use InGuest (no hypervisor) mode. An error will also be recorded in the Windows Event Viewer.
  
* New feature for InGuest will ensure agent computers are only granted access to a VHD volume on the fly directly
  before the Agent can mount the file.

* Added the ability to edit a Storage Group. Look for the [Edit] button after expanding a storage group row.

* Added the ability to rescan a Storage Group. This operation contacts the hypervisor and updates storage information
  metadata such as free space and overall capacity. It also adds any new storage that makes the prefix (automatic-mode).

* Added the ability to manually synchronize all Users or all Computers with ActiveDirectory. New [Sync] buttons are
  available on the Directory->Users and Directory->Computers pages.

* Administrator can now delete AppStacks and Writables in "unreachable" and "creating" states.

* [API] Added POST /cv_api/sessions route for API login, takes "username" and "password" parameters. Once logged in
  this way, no CSRF-token is needed for subsequent requests.

* [API] Added DELETE /cv_api/sessions route for API logout

* [Security] Session token is re-create during UI and API login requests

* [Security] HTTP server no longer accepts RC4 or EXPORT ssl ciphers, see nginx/conf/nginx.conf for the full list

* Users can upload prepackaged VHD templates using the "Upload Prepackaged Volumes" button during storage configuration.

## Trusted Domain Credentials

AV Manager automatically connects to domains trusted by the primary domain specified during configuration.

It is expected that the credentials provided can be used to authenticate to each trusted domains. This normally works
because the most trusted domains have two-way trust. That means the DOMAIN1\av_manager can be used to authenticate and
query DOMAIN2. However, if when there is a one-way trust, DOMAIN1 can trusts DOMAIN2, but DOMAIN1 credentials can not be
used to authenticate on DOMAIN2.

You can now specify a second set of credentials that will be used whenever connecting to a trust during ActiveDirectory
configuration. These credentials will be used when connecting to any trust instead of the primary domains credentials.
A list of domains to use the new trust credentials can be provided. Instead of using the credentials on all trusted
domains, they are only used on the specified domains. The list should be separated by spaces: domain2.local domain3.com.

If the domain controller can not be automatically detected from DNS, you can add that to a domain in the list
using a semi-colon. For example: domain2.local;10.0.0.1 domain3.com;ldap.domain3.com. Such a configuration will even
allow you to use a domain that is not trusted by the primary domain.


## vCenter Direct-to-Host Mounting

Volume mounts are performed using vSphere's VM reconfigure API. The time it takes to run this command can is shorter
when it is performed directly on an ESX host. The AV Manager allow an administrator to enable this functionality
when configuring a vCenter hypervisor.

To use this feature, each ESX server being managed by a vCenter must have an identical system/service account. We
suggest creating an account on each ESX with limited permissions.

If enabled, the AV Manager will attempt to connect to every ESX server before saving the hypervisor configuration.
Prior to the v2.7 release, the configuration would not be considered valid unless every ESX server could be reached.
In v2.7 and beyond, the administrator will receive a warning message if any of the ESX servers can not be contacted
but the configuration settings will still be saved. The administrator may want some ESX servers not to be included or
some of the servers may be offline for maintenance.


## VHD: Dynamic File Permissions

Files located on SMB based file shares can be accessed from multiple computers.  Enabling this feature for InGuest will
ensure agent computers are only granted access to VHD file on the fly directly before the Agent can mount the file.
Upon logoff (User Assignments) and shutdown (Computer Assignments) the rights are then revoked from the agent computer.

To enable this feature, from the Configuration > Hypervisor tab select "Hypervisor: None [VHD] In-Guest Mounting" and
check the "Security: [ ] Dynamic File Permissions" checkbox.

Note: An administrator must also configure the minimal file permissions as follows:

manager_server$   |  Full Control
Domain Computers  |  List Folder Contents


## Synchronize ActiveDirectory Entities

The AppVolumes Manager keeps a database record for any ActiveDirectory that has been seen by the Agent or assigned
an AppStack or Writable Volume. A background job is run every hour to synchronize up to 1000 entities. If you have more
than 1000 items, the a new batch of 1000 will be synchronized the next hour and so on.

Synchronization mainly updates human-readable information like name. It also checks the entry's enabled or disabled
state.

You can force a manual synchronization in two ways. First you can use the [Sync] button on the detail view for
an individual user or computer. You can also use the Sync button on the Users or Computers index pages. These buttons
will schedule a background job to synchronize every User or Computer in the Manager database.


## Deletion of AppStacks and Writables in "Unreachable" and "Creating" states

AppStacks and Writables that exists on a datastore that can no longer be contacted have their state set to "unreachable".
Previously these volumes were unable to be deleted from the Manager interface. This was restricted so that volumes
were not removed from the Manager unless the underlying volume files could be deleted. When a datastore can not be
contacted, we are unable to delete the volume file. However, if a datastore is removed and is never going to return,
the administrator was left with stale records. Administrators can now remove an AppStack or Writable even when it is
"unreachable".

## New interface for managing VHD file shares

Previously users would blindly add UNC paths with no information regarding if the path would function.  Now an
administrator can enter in a UNC (or partial UNC) path to browse shares.  The administrator will also be given a
warning message if the App Volumes Manager is unable to write to the share alerting the administrator that further
action may need to be taken.  Also, as a convenience, the administrator can quickly view the file share permissions.
