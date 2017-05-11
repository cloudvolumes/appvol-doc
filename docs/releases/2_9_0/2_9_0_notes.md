# Developer notes for Manager release 2.9

## Notable changes
- Hypervisor configuration is now called Machine Managers.
- Multiple vCenters can be used with the App Volumes Manager concurrently.
- When more than one vcenter is added, multiple datacenters can be used.
- All json responses returned from /cv_api/ always have the Content-Type of application/json
- /cv_api/appstacks is no longer nested under a "currentappstack" and "appstacks" objects - it returns a flat array
- Storage replication will not work across multiple vCenters and is unsupported.

## Machine Managers - Multiple vCenter

Three types of Machine Managers are available and only one type may be selected by the App Volumes Manager database.

The three types are:

 * VMware vCenter Server
 * VMware ESX Server
 * VHD In-guest Services

System Administrators can add more than one vCenter to the Machine Manager configuration.

Note: VMs must be unique across all vCenters. Datastore names must be unique across all vCenters.

## Deleting a Machine Manager - vCenter Server Only

Deleting a machine manager will result in removal of related records.  Detaching logging users off and powering down machines
managed on the machine manager being deleted will ensure Appstacks and Writable Volumes are not left attached.

The following records will be removed when a related Machine Manager is deleted:

   - machine references

   - file references

   - storage locations

   - members of storage groups

   - snapvol file references of multi-file snapvols

   - snapvols with no more files