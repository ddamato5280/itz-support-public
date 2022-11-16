# Migrate to VMware in Techzone from Aspera

This page contains the steps to migrate a VMware image from Aspera to the VMware on IBM Cloud infrastructure.

### 1. Request access to private channel `#skytap-migration-support` for migration inquiries via Slack `#ITZ-TECHZONE-SUPPORT` or https://techzone.ibm.com/help

### 2. Make a reservations for Template Builder RHEL Bastion VM in Techzone and optionally IBM Cloud Object Storage from https://techzone.ibm.com/collection/skytap-migration
- If the Template Builder reservation expires, everything is lost except what is uploaded to COS!!! Confirm that the VMware Files on Aspera is secure when skipping this step. 
- Request access to [skytap-migration collection]([skytap-migratio](https://techzone.ibm.com/collection/skytap-migration) from Support. 
- ![migrate5](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate5.png)
- Your reservation details will be something like:
- ![migrate7](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate6.png) 

### 3. Use the URL at top of Template Builder reservation to connect to the Template Builder RHEL Bastion VM
- Use the vSphere console login information at the bottom of the reservation details from within Template Builder RHEL VM
- ![migrate7](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate7.png)
- Open Chrome and use credentials for vSphere found at bottom of Template Builder reservation.
- Best to use Chrome for clipboard access or if using Firefox and unable to copy/paste in Remote Desktop Web Client, please enable clipboard See https://sudoedit.com/firefox-async-clipboard/.` 

### 4. Download the target VM from Aspera to the Template Builder RHEL Bastion VM
- Install Aspera in the Template Builder RHEL Bastion VM  if Aspera is not installed. The following image will be seen when navigating to the IBM Aspera site when it is not installed.
- ![install-aspera](https://user-images.githubusercontent.com/18425410/201855212-c4c8a934-4d37-4bdb-a722-cc96f2bc2c00.jpg)
- Download the target VM from Aspera to the /home/techzone folder.
- Once downloaded, extract the files using 
```
yum -y install p7zip
```
and 
```
7za e <archive name>
```
- Wait 10 min or over an hour, until the files extract.

### 5. Edit the .vmx, remove floppy, and set power options to “soft” (depending on errors when running the ovftool,  other references may need to be removed)
- ![migrate14b](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate14b.png)
- ![migrate13](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate13.png)
- ![migrate12](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate12.png)

### 6. Convert VM to OVA.  
- This combines all VMware files into one giant image that can be run on the IBM Cloud.
- In a terminal window run
```
cd ~ 
ovftool session.vmx mynewvm.ova
```
  ![migrate14](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate14.png)

### 7. **Optional** *assuming the target Aspera files are not protected* backup the VM files by uploading to Cloud Object Storage
- Using the Cloud Object Storage environment reserved in Step 1, log into cloud.ibm.com, navigate to Storage under resources and open the dte-cos instance.
- ![migrate15](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate15.png)
- Put images in the skytap-exports bucket.
- ![migrate16](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate16.png)

### 8. Log into vSphere in Chrome using the credentials at the bottom of the Template Builder reservation.  
- Find a relevant folder or create a folder in vSphere under templates-shared where the OVA file can be uploaded into.   

### 9. Upload the OVA File VM to vSphere 
- Right click to Deploy OVF and select the OVA file file created in step #6
- ![migrate17](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate17.png)
- For "Select a name and folder", be consistent with the other resources the target folder
- ![migrate19](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate19.png)
- Select the gym member default compute resource:
- ![migrate20](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate20.png)
- Click next on Review Details screen
- Select the storage "datastore-shared":
- ![migrate21](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate21.png)
- Select the network "gym-segment-shared":
- ![migrate22](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate22.png)
- Click Finish
- ![migrate23](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate23.png)
- ![migrate24](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate24.png)
- Wait for recent tasks to complete before proceeding to the next step
- ![recent-tasks](https://user-images.githubusercontent.com/18425410/201935029-73647d6d-4554-4c5f-b6ca-da16feaa4d04.jpg)

### 10. Upgrade vSphere compatibility
- ![migrate25](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate25.png)
- ![migrate26](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate26.png)

### 11. Clone to Template
- ![migrate27](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate27.png)
- ![migrate28](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate28.png)

### 12. Create Techzone environment in https://techzone.ibm.com/collection/skytap-test
- ![migrate36](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate36.png)

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
- Infrastructure admin to template and provide env details


