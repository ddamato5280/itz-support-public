# Migrate to VMware in Techzone from Aspera

This page contains the steps to migrate a VMware image from Aspera to the VMware on IBM Cloud infrastructure.

### 1. Request access to private channel `#skytap-migration-support` for migration inquiries via Slack `#ITZ-TECHZONE-SUPPORT` or https://techzone.ibm.com/help

### 2. Make a reservations for Template Builder RHEL Bastion VM in Techzone and optionally IBM Cloud Object Storage from https://techzone.ibm.com/collection/skytap-migration
- If the Template Builder reservation expires, everything is lost except what is uploaded to COS!!! Confirm that the VMware Files on Aspera is secure when skipping this step. 
- Request access to [TechZone's skytap-migration collection](https://techzone.ibm.com/collection/skytap-migration) from Support. 
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
- Find a relevant folder or create a folder in vSphere under `templates-shared` where the OVA file can be uploaded into.   

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
- On the uploaded OVA file actions menu, select Compatibility and then Upgrade VM Compatibility
- ![migrate25](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate25.png)
- Configure for compatibility as seen below
- ![migrate26](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate26.png)

### 11. Clone to Template
- Right click on the uploaded OVA file and select Clone to Template
- ![migrate27](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate27.png)
- Select a name and folder for the template.
- ![clone-machine-to-template](https://user-images.githubusercontent.com/18425410/202279921-557b8503-e883-4d48-bd74-2867a0190db8.jpg)
- Select the default compute resource
- Select datastore-share for storage
- ![migrate28](https://github.com/IBM/itz-support-public/blob/main/Skytap/Skytap-Runbooks/Images/skytapmigrate28.png)
- Click finish
- ![clone-machine-to-template-4](https://user-images.githubusercontent.com/18425410/202280977-981bf5a7-2e49-463b-a373-58ec4f224273.jpg)


### 12. Create Techzone environment in a collection
- In TechZone, click on Share your content
- ![tz-share-your-content](https://user-images.githubusercontent.com/18425410/202511281-e20ad4f5-146d-4dbc-bc85-1057115cf195.jpg)
- Scroll down the page and click on Add an environnment
- ![environment](https://user-images.githubusercontent.com/18425410/202511571-4ceea114-f99d-44d9-94e8-f2a1efd52ca9.jpg)
- Select IBM Cloud for Infrastructure and vmware-template for Gitops Pattern. Also, fill out Title and Description
- ![environment-infr-gitops](https://user-images.githubusercontent.com/18425410/202512561-cb3e2243-d069-41e8-bc76-6f629d51d13d.jpg)
- In settings pick `itzvmware` for Account Pool and Cloud Account. As the techzone team in `#ITZ-TECHZONE-SUPPORT` which Geo, Region, and Datacenter and click the + sign to add the target data center option
- ![environment-settings](https://user-images.githubusercontent.com/18425410/202514310-289106d0-3d3a-4dba-85c1-ce9e8909af14.jpg)
- Populate the **Terraform Variables Overrinding** as follows
  - `vm_domain` - unless otherwise specifice by Tech Zone support - `ibmdte.net`
  - `vm_router_ip` - unless otherwise specifice by Tech Zone support - ``
  - `vm_subnet` - unless otherwise specifice by Tech Zone support -  ``
  - `vm_template_folder` - the `templates-shared` picked in Step #9 above
  - `vm_template_id` - the relevant folder or created a folder in Step #9 above as seen in example below
  - ![env-terraform-vars](https://user-images.githubusercontent.com/18425410/202517636-3f41b73e-cbf6-40dd-aa2e-1d7dce65c660.jpg)
  - `vm_map_string` - each VM in the folllwing json will get started as described
  - ![env-terraform-var-map-string](https://user-images.githubusercontent.com/18425410/202518864-fd322a62-1942-459b-9154-aa3727d88a6b.jpg)
  - Click Save to close the environment dialog

### 13. Test reservation
- Save the collection with Collection Status as Draft, click on Reserve in the Environment section, and confirm that it runs as expected
- ![environment-reserve](https://user-images.githubusercontent.com/18425410/202519758-eb52e5bb-c663-452c-9484-f360e6c1ee7f.jpg)
- Add collaborators using their emails and ask them to Reserve an Environment to confirm that it runs as expected for them too. They will be able to see the Preview page when they are added in as collaborators
![test](https://user-images.githubusercontent.com/18425410/202519399-76666221-2275-43ff-95b5-2d675d6847cf.jpg)

### 14. Production rollout
- Work with `#ITZ-TECHZONE-SUPPORT` to confirm that the environment is ready for production before changing the status to Active. Once it is active, everyone will be able to see it and use it. 

