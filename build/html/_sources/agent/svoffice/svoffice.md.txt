# SVOFFICE

svoffice.exe is a program that lives in the template.  It was introduced around AV 2.10.  It runs at post-provisioning to groom some Office-related settings on the AppStack.  Prior to 2.12, post-provisioning ran the program only if the AppStack contained an Office product.  Starting in 2.12.1, post-provisioning runs the program unconditionally, for obscure technical reasons.  For AppStacks containing an Office product, svoffice also runs during volume attachment to groom the system volume and writable volume.

## Errors
 
Has anyone seen the error of 'Svoffice failed. Please collect log files for VMware support.'? This dialog is presented to the customer whenever they assign a VDI desktop or NON-VDI (VM) the Office 2016 AppStack. Once you clear the error (Select 'OK') the dialog goes away and Office works great. 

This is Office 2016 O365 with App Volumes 2.12.1

Art Rothstein:
It's new in 2.12.1. I got tired of asking customers with problems to check their cv_startup_postsvc.log file for an error.  Now when svoffice writes an error message to that file, it also displays that message box.

Please check %SVAgent%\cv_startup_postsvc.log.  It's a small text file.  Office may appear to be working great, but the presence of an error in that log means there's a problem lurking.


