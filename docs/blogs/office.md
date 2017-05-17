https://blogs.vmware.com/euc/2015/06/vmware-app-volumes-microsoft-office-license-install.html


By Denis Gundarev, Application Solutions Architect, App Volumes R&D, VMware
This article is the first in a series of blog posts about using Microsoft Office with VMware App Volumes.
Microsoft Office and VMware App Volumes
Microsoft Office is one of the most popular application suites deployed on Windows enterprise desktops. Microsoft Office applications are often implemented first in VDI installations, both in proofs of concept and in production environments.
With dynamic application delivery technologies such as App Volumes and VMware ThinApp, Microsoft Office deployment and compatibility are very important.
In this blog series, we will explain various scenarios using Microsoft Office with VDI, as well as use cases and best practices for deploying Microsoft Office with App Volumes. We will discuss compatibility and interoperability for different Microsoft Office versions and editions, Office activation, and support for locally installed and AppStack-provisioned Office components, as well as troubleshooting and fine-tuning AppStacks with Office components.
This information applies to both View virtual desktops in VMware Horizon 6, and to Citrix XenDesktop virtual desktops.
But we should start at the beginning. Before provisioning any application to an AppStack, we need to understand which scenarios are supported or allowed by the application vendor. This can be a very tricky question, and it becomes even more complicated when you start thinking about Microsoft licensing.
Licensing

In my consulting practice, many customers have asked if application virtualization could reduce software licensing costs, and my answer has always been simple—ask your application vendor. For Microsoft Office, the answer is no. None of the application virtualization solutions for Microsoft Office will reduce Office licensing costs. All Microsoft Office applications purchased as a software product are licensed per device, where “device” is a physical or virtual system that runs Microsoft Office applications or a system that is used to access Office applications remotely. Each license permits only one user to access the software at a time. The only exception to this rule is the subscription-based model that is licensed per user. Also important to note is that neither of the following purchase scenarios permits network use:
Microsoft Office retail purchase (the fully packaged product)
Acquisition of Office with hardware that includes the original equipment manufacturer (OEM) licensing
In other words, for most VDI scenarios you can use only Microsoft Office editions that are available with Microsoft volume licensing. These include Office Professional Plus 2013, Office Standard 2013, and Office 365 ProPlus. You cannot use Office Home & Student or Office Personal editions on VDI.
Licensing Compared to Activation

Do not confuse licensing with activation.
When you license software, you acquire the right to use the software according to either the end-user-licensing agreement (EULA) or the Product Usage Rights (PUR), or both. Currently, Microsoft does not enforce license validation for Office on endpoint devices, but instead relies on compliance with the EULA. However, Microsoft requires that you activate the product on the device where it is running, although not necessarily where it is installed.
When you activate the Microsoft Office product, you submit the product key for validation and, if the product key is valid and not compromised, the activation subsystem enables licensed features of the product.
Activation Methods

Now that we know the difference between licensing and activation, let us talk about activation methods.
Microsoft Office products acquired through Microsoft volume licensing support two activation methods: Multiple Activation Key (MAK) activation and Key Management Service (KMS) host activation.
Multiple Activation Key (MAK) activation can be used to activate software on multiple devices. Using this method, information about every activated device is sent automatically to Microsoft. MAK keys must be entered on every device where Microsoft Office is installed. Each MAK has a predetermined number of allowed activations, depending on your agreement. Every time you activate the software using an MAK, the number of allowed activations for this license key decrements. If the activation limit is reached, you need to contact Microsoft to request a new license key or extend the activation limit. MAK activation status is stored on the computer where the software is installed. This means that if you are using a nonpersistent VDI deployment, a copy of Microsoft Office is activated after every reboot, and you will reach the activation limit very quickly, even in a small environment.
Key Management Service (KMS) host activation works differently. You do not need to enter the product key on every device because a preinstalled Generic Volume License Key (GVLK) is used instead. When a user launches the application, the application tries to discover a Key Management Service (KMS) host. If discovery is successful and the KMS host has a KMS key for that product, the software is activated. The KMS host collects a unique client machine identification (CMID) from the client computer and saves each CMID in a KMS host table. If a client computer renews its activation within 30 days, the cached record is removed from the table, a new record is created, and the 30-day period begins again. If a KMS client computer does not renew its activation within 30 days, the KMS host removes the corresponding CMID from the table and reduces the activation count by one.
If you plan to use Windows 8 with Office 2013, you can also evaluate Active Directory–Based Activation (ADBA) as a type of KMS. Active Directory–Based Activation works similarly to KMS activation but uses standard Active Directory interfaces to communicate with the KMS host. ADBA can be used for Office 2013 volume license activation using the standard KMS product key. However, neither Windows 7 nor Office 2010 is supported, which limits the use case for the ADBA activation method at the present time.
VMware recommends KMS host–based activation for Microsoft Office in a nonpersistent VDI deployment because it enables a fully automated activation that does not require interaction with the user. Moreover, if you use View virtual desktops in Horizon 6, KMS is the only officially supported activation method.
Office 365 ProPlus

If you are using Microsoft Office 365 ProPlus as part of your Microsoft Volume Licensing Agreement, you may use the Office 365 Click-To-Run installation package for persistent VDI desktops. Office 365 applications, in this case, will use per user activation with Microsoft Online servers.
However, Office Online activation requires that the user enter credentials on every new computer or virtual machine they are using to run Microsoft Office. This means that in a nonpersistent VDI scenario, or when Office is delivered using application virtualization with App Volumes, the activation limit will be reached very quickly and Office will enter the limited functionality mode. Because of this, you should use Microsoft Office 2013 volume licensing media with traditional KMS host–based activation on nonpersistent VDI desktops and when implementing App Volumes.
Note that you need to have access to the Microsoft Volume Licensing Service Center (VLSC) in order to download Microsoft Office 2013 volume licensing media. If you do not have such access, consult your Microsoft volume licensing partner.
Office 32-Bit Compared to 64-Bit Versions

Microsoft Office is available in both 64-bit and 32-bit versions. The Office 64-bit version will not run on 32-bit Windows, but is it a good idea to run a 64-bit version of Office on 64-bit Windows? Do this only if your users have specific needs such as editing large Microsoft Excel or Microsoft Project files. 64-bit versions of Microsoft Office have limited compatibility with third-party products and even have limitations for built-in functionality. Microsoft recommends the use of 32-bit versions of Microsoft Office for most users.
Another essential point to consider is that Office does not support running side-by-side installations of 64-bit and 32-bit versions of Office or Office components. As an example, you cannot run a 32-bit version of Microsoft Visio on a computer that has a 64-bit version of Microsoft Office installed, and vice versa.
This support limitation also affects Office applications delivered with App Volumes. For example, you cannot install 32-bit Office into the base image and deliver a 64-bit version of other Office applications through an AppStack.
Where to Get the Software and Product Keys

You need to purchase licenses for Microsoft Office directly from Microsoft or from a Microsoft partner. And for VDI scenarios you can use only Office editions available through Microsoft volume licensing.
However, enough about licensing…let us talk about the actual software bits that you need to use for deployment. As I mentioned before, for VDI scenarios, you need the Office Professional Plus 2013 or Office Standard 2013 installation source, even if you are using Office 365 licenses. Installation files downloaded only from a Microsoft Volume Licensing Support Center (VLSC) should be used. Do not try to use installation media that you downloaded from the TechNet Evaluation Center or from your MSDN subscription because these ISO files contain retail versions of Microsoft Office that are not allowed for use in most VDI scenarios. If you do not have access to VLSC, contact your volume licensing agreement administrator or Microsoft licensing partner to request access.
If ISO files have already been downloaded for you, verify them for use with App Volumes before proceeding with deployment or AppStack provisioning.
You must have a folder named Admin on your ISO; if there is no such folder, this is not a suitable ISO.
Check that the name of the folder that has a .WW extension (ProPlus.WW, Visio.WW, PrjPro.WW, Standard.WW, and so on). If there is a lowercase “r” before the dot, which means retail (ProPlusr.WW, Visior.WW), then this is not a suitable ISO.
The same is true for KMS product keys. They are available at VLSC, or they can be requested by phone. MSDN subscriptions come with retail keys that cannot be used for KMS activation.
Important: Volume-licensed Microsoft Office comes with the Generic Volume License Key (GVLK), which is used for KMS activation. Install the KMS key only on a KMS server, and do so before you implement App Volumes. Do not enter the KMS key during or after App Volumes installation or when you provision the AppStack.
Summary

To summarize the information discussed here:
In most VDI deployments, you can use only Microsoft Office Professional Plus 2013, Office Standard 2013, or Office 365 ProPlus by subscription, acquired through a Volume Licensing Agreement.
Retail and OEM versions of Office cannot be used for VDI.
If you use nonpersistent VDI or any kind of application virtualization, or both, you need to use KMS activation.
With KMS activation, you must use installation media that was downloaded from Microsoft VLSC.
You should use 32-bit versions of Microsoft Office unless your users have a particular interest in editing large Excel or Project files, in which case you can use 64-bit versions.
Mixing 32-bit and 64-bit Office installations on the same machine is not supported.
I hope that you find this article useful. Come back for the next post of this series where I will cover supported scenarios for using Microsoft Office with App Volumes, KMS deployment and troubleshooting, procedures that you can follow to provision an AppStack with Microsoft Office, and troubleshooting Office applications delivered through App Volumes.


<ul>
<li><a href="https://www.microsoft.com/en-us/Licensing/learn-more/brief-Office-volume-licensing.aspx" target="_blank" name="&amp;lpos=apps_scodevmw : 49">Licensing brief: Licensing Microsoft Office software in Volume Licensing</a></li>
<li><a href="https://www.microsoft.com/en-us/Licensing/learn-more/brief-office-365.aspx" target="_blank" name="&amp;lpos=apps_scodevmw : 50">Licensing brief: Licensing Microsoft Office 365 ProPlus subscription service in Volume Licensing</a></li>
<li><a href="http://download.microsoft.com/download/1/1/4/114A45DD-A1F7-4910-81FD-6CAF401077D0/Microsoft%20VDI%20and%20VDA%20FAQ%20v3%200.pdf" target="_blank" name="&amp;lpos=apps_scodevmw : 51">Licensing the Windows Desktop for VDI Environments</a></li>
<li><a href="https://www.microsoft.com/en-us/licensing/learn-more/brief-remote-desktop-services.aspx" target="_blank" name="&amp;lpos=apps_scodevmw : 52">Licensing brief: Licensing of Microsoft desktop application software for use with Windows Server Remote Desktop Services</a></li>
<li><a href="https://www.microsoft.com/en-us/licensing/learn-more/brief-windows-server-2012-rds.aspx" target="_blank" name="&amp;lpos=apps_scodevmw : 53">Licensing Windows Server 2012 R2 Remote Desktop Services and Microsoft desktop applications for use with RDS</a></li>
<li><a href="http://blogs.technet.com/b/office_resource_kit/archive/2013/05/21/the-new-office-garage-series-managing-office-in-virtualized-environments.aspx" target="_blank" name="&amp;lpos=apps_scodevmw : 54">The new Office Garage Series: Managing Office in Virtualized Environments</a></li>
<li><a href="https://technet.microsoft.com/en-us/library/gg982959.aspx" target="_blank" name="&amp;lpos=apps_scodevmw : 55">Overview of licensing and activation in Office 365 ProPlus</a></li>
<li><a href="https://www.microsoft.com/en-us/download/details.aspx?id=3497" target="_blank" name="&amp;lpos=apps_scodevmw : 56">Microsoft Product Use Rights Explained</a></li>
<li><a href="https://www.microsoft.com/en-us/licensing/existing-customer/FAQ-product-activation.aspx" target="_blank" name="&amp;lpos=apps_scodevmw : 57">Frequently asked questions about Volume License keys</a></li>
<li><a href="https://technet.microsoft.com/en-us/library/ee705504(v=office.15)" target="_blank" name="&amp;lpos=apps_scodevmw : 58">Volume activation of Office 2013</a></li>
<li><a href="https://technet.microsoft.com/en-us/library/ee705504(v=office.14)" target="_blank" name="&amp;lpos=apps_scodevmw : 59">Plan for volume activation of Office 2010</a></li>
<li><a href="https://technet.microsoft.com/en-us/library/dn385361" target="_blank" name="&amp;lpos=apps_scodevmw : 60">Active Directory–based activation of Office 2013</a></li>
<li><a href="https://technet.microsoft.com/en-us/library/ee624357" target="_blank">KMS activation of Office 2013</a></li>
<li><a href="http://blogs.technet.com/b/uspartner_ts2team/archive/2013/08/31/clarification-of-office-365-pro-plus-support-in-vdi-environments.aspx" target="_blank" name="&amp;lpos=apps_scodevmw : 62">Clarification of Office 365 Pro Plus support in VDI environments</a></li>
</ul>