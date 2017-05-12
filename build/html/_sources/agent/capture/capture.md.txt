# Capture

Often referred to as "Provisioning". This is the process of recording an application install in an AppStack.

## Common Questions

### Large Application Failures

When they connect an AppStack (150GB) and install a large application and it fails because the application does a pre-installation validation to look at disk space and only the c:\ disk space (50GB) is being reported. Has anyone come across this?

Matt Conover: 
There is a setting to determine whether we return the size of the C: drive or appstack in provisioning mode. you can create a registry setting under HKLM\SYSTEM\CurrentControlSet\services\svdriver\parameters named ReportSystemFreeSpace type REG_DWORD and set value to 0 (on the system being used for provisioning) 
then instead of reporting C: drive we'll report size of writable volume (or the AppStack in provisioning mode which is functionally the same as a writable volume)