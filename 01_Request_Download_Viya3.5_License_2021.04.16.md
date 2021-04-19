## Request License and Download Image of SAS Viya 3.5

1. **Request Employee Viya 3.5 license**
   
   - [SAS MakeOrder Product Ordering Application](https://makeorder.sas.com/makeorder/)
   
2. **Download and Install [SAS Mirror Manager](https://support.sas.com/en/documentation/install-center/viya/deployment-tools/34/mirror-manager.html)**

	```bash
	gunzip mirrormgr-linux.tgz
	tar -xvf mirrormgr-linux.tar
	```

3. **Download Viya 3.5 Image (Sample)**

	```bash
	./mirrormgr mirror --deployment-data /opt/install/SAS_Viya3.5_Full_21W13_2021.04.16.zip --path /opt/install/SAS_Viya3.5_Full_21W13_2021.04.16 --log-file /opt/install/SAS_Viya3.5_Full_21W13_2021.04.16_Download.log --platform x64-redhat-linux-6 --latest --debug --workers 1
	```

4. **Verify Downloaded Viya 3.5 Image (Sample)**

	```bash
	./mirrormgr verify --path /opt/install/SAS_Viya3.5_Full_21W13_2021.04.16 --log-file /opt/install/SAS_Viya3.5_Full_21W13_2021.04.16_Verify.log --platform x64-redhat-linux-6 --latest
	```

## Download and Update SAS Viya Packages  

1. **List Packages Available for Update**

   ```bash
   ./mirrormgr mirror diff --deployment-data /opt/install/SAS_Viya3.5_Full_21W13_2021.04.16.zip --path /opt/install/SAS_Viya3.5_Full_21W13_2021.04.16 --log-file /opt/install/SAS_Viya3.5_Full_21W13_2021.04.16_Update.log --platform x64-redhat-linux-6 --latest --debug --workers 10
   ```

2. **Download Packages Available for Update**

   ```bash
   ./mirrormgr mirror --deployment-data /opt/install/SAS_Viya3.5_Full_21W13_2021.04.16.zip --path /opt/install/SAS_Viya3.5_Full_21W13_2021.04.16 --log-file /opt/install/SAS_Viya3.5_Full_21W13_2021.04.16_Download.log --platform x64-redhat-linux-6 --latest --debug --remove-old --workers 20
   ```

3. **Save Package Info Before Update** 

   ```bash 
   sudo rpm -qg SAS | sort > /opt/install/SAS_Pkg_Before.txt
   ```

4. **Apply Packages for Update**

   ```bash
   ansible-playbook update-only.yml
   ```

5. **Save Package Info After Update** 

   ```bash
   sudo rpm -qg SAS | sort > /opt/install/SAS_Pkg_After.txt
   ```

6. **Save Package Difference** 

   ```bash
   diff SAS_Pkg_Before.txt SAS_Pkg_After.txt > SAS_Pkg_Difference.txt
   ```