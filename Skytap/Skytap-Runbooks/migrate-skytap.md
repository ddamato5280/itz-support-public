# Migrate to VMware in Techzone from Aspera

This will guide you through the steps to migrate your Skytap environments to the VMware on IBM Cloud infrastructure.

1. Request to access to private channel #skytap-migration-support for  migration inquiries via Slack #ITZ-TECHZONE-SUPPORT or https://techzone.ibm.com/help

2. In Techzone, make reservations for Template Builder, and IBM Cloud Object Storage from https://techzone.ibm.com/collection/skytap-migration
- If your Template Builder reservation expires you lose everything except what you upload to COS!!!
- If you don't have access to this Collection, request it from Support.

![migrate5](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate5.png)

- Your reservation details will be something like:
![migrate7](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate6.png)
- Use the URL at top of Template Builder reservation to connect to the Template Builder VM. 

3. From within Template Builder VM

- The vSphere console login information is at the bottom of the reservation details.
![migrate7](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate7.png)

`Open Chrome and use credentials for vSphere found at bottom of Template Builder reservation.
Best to use Chrome for clipboard access, or if you are using Firefox and unable to copy/paste in Remote Desktop Web Client, please enable clipboard See https://sudoedit.com/firefox-async-clipboard/.` 

4. Download exported VM
- Use the link provided by support and paste it into Filezilla within the Template Builder VM.
![migrate11b](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate11b.png)
- Download the VM to the /home/techzone folder.
- Once downloaded you can extract the using 
- '''yum -y install p7zip'''
- and 
- '''7za e <archive name>'''
- Wait 10 min or over an hour, until the .vmx file extracts.

5. Edit the .vmx, remove floppy, and set power options to “soft”
![migrate14b](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate14b.png)

![migrate13](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate13.png)

![migrate12](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate12.png)

6. Convert VM to OVA.  
- Run a Terminal window
```
cd ~
ovftool session.vmx mynewvm.ova
```
![migrate14](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate14.png)

7. Backup your environment.   Upload to Cloud Object Storage

`Assuming you made a reservation for Cloud Object Storage, and accepted the invite to the ITZ-TECH account.`

  Log into cloud.ibm.com, navigate to Storage under resources and open the dte-cos instance.

![migrate15](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate15.png)
Put images in the skytap-exports bucket.
![migrate16](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate16.png)

8. Open Chrome, log into vSphere using the credentials at the bottom of your Template Builder reservation.  Create folder in vSphere under templates-shared.  

9. Right click to Deploy OVF.
![migrate17](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate17.png)
![migrate19](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate19.png)
- Select your default compute resource:
![migrate20](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate20.png)
- Select the storage "datastore-shared":
![migrate21](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate21.png)
- Select the network "gym-segment-shared":
![migrate22](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate22.png)
- Finish
![migrate23](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate23.png)
![migrate24](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate24.png)

10. Upgrade vSphere compatibility
![migrate25](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate25.png)
![migrate26](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate26.png)


11. Clone to Template
![migrate27](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate27.png)
![migrate28](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate28.png)

12. Create Techzone environment in https://techzone.ibm.com/collection/skytap-test

![migrate36](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate36.png)


Variables required
- vm_template_folder
- vm_template_id
- vm_map_string
- vm_domain
- vm_router_ip
- vm_subnet

13. Test reservation

14. Production rollout
- Support request to template VM to production
Infrastructure admin to template and provide env details


