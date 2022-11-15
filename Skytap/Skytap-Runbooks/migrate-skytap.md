# Migrate to VMware in Techzone from Aspera

This will guide you through the steps to migrate your Skytap environments to the VMware on IBM Cloud infrastructure.

### 1. Request access to private channel `#skytap-migration-support` for migration inquiries via Slack `#ITZ-TECHZONE-SUPPORT` or https://techzone.ibm.com/help

### 2. Make a reservations for Template Builder in Techzone and optionally IBM Cloud Object Storage from https://techzone.ibm.com/collection/skytap-migration
- If your Template Builder reservation expires you lose everything except what you upload to COS!!! You must confirm that your File location on Aspera is secure if you choose to skip this step. 
- If you don't have access to this Collection, request it from Support.
![migrate5](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate5.png)

- Your reservation details will be something like:
![migrate7](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate6.png) 

### 3. Use the URL at top of Template Builder reservation to connect to the Template Builder VM
- Use the vSphere console login information at the bottom of the reservation details from within Template Builder RHEL VM
![migrate7](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate7.png)

`Open Chrome and use credentials for vSphere found at bottom of Template Builder reservation.
Best to use Chrome for clipboard access, or if you are using Firefox and unable to copy/paste in Remote Desktop Web Client, please enable clipboard See https://sudoedit.com/firefox-async-clipboard/.` 

### 4. Download the target VM from your Aspera environment to your Template Builder
- Install Aspera in your Template Builder image if Aspera is not installed. You will see the following image when you visit the IBM Aspera site when it is not installed.
- ![install-aspera](https://user-images.githubusercontent.com/18425410/201855212-c4c8a934-4d37-4bdb-a722-cc96f2bc2c00.jpg)
- Download the VM to the /home/techzone folder.
- Once downloaded you can extract the using 
- '''yum -y install p7zip'''
- and 
- '''7za e <archive name>'''
- Wait 10 min or over an hour, until the .vmx file extracts.

### 5. Edit the .vmx, remove floppy, and set power options to “soft” (depending on the errors you get when running the ovftool, you may need to remove other references)
![migrate14b](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate14b.png)
![migrate13](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate13.png)
![migrate12](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate12.png)

### 6. Convert VM to OVA.  
- Run a Terminal window
'''
cd ~
ovftool session.vmx mynewvm.ova
'''
![migrate14](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate14.png)

### 7. **Optional** *assuming your Aspera files are not protected* backup your environment by uploading to Cloud Object Storage
- Using the Cloud Object Storage environment reserved in Step 1, log into cloud.ibm.com, navigate to Storage under resources and open the dte-cos instance.
![migrate15](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate15.png)
- Put images in the skytap-exports bucket.
![migrate16](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate16.png)

### 8. Log into vSphere in Chrome using the credentials at the bottom of the Template Builder reservation.  Find a relevant or create folder in vSphere under templates-shared.  

### 9. Upload the VM to TechZOne 
- Right click to Deploy OVF and select the *.ova file that you created in step #6
![migrate17](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate17.png)
- For "Select a name and folder" be consistent with the other resources you selected in under templates-shared
![migrate19](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate19.png)
- Select the gym member default compute resource:
![migrate20](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate20.png)
- Click next on Review Details
- Select the storage "datastore-shared":
![migrate21](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate21.png)
- Select the network "gym-segment-shared":
![migrate22](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate22.png)
- Click Finish
![migrate23](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate23.png)
![migrate24](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate24.png)
- Wait for recent tasks to complete before proceeding to the next step
![recent-tasks](https://user-images.githubusercontent.com/18425410/201935029-73647d6d-4554-4c5f-b6ca-da16feaa4d04.jpg)

### 10. Upgrade vSphere compatibility
![migrate25](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate25.png)
![migrate26](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate26.png)

### 11. Clone to Template
![migrate27](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate27.png)
![migrate28](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate28.png)

### 12. Create Techzone environment in https://techzone.ibm.com/collection/skytap-test
![migrate36](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate36.png)

Variables required
- vm_template_folder
- vm_template_id
- vm_map_string
- vm_domain
- vm_router_ip
- vm_subnet

### 13. Test reservation

### 14. Production rollout
- Support request to template VM to production
Infrastructure admin to template and provide env details


