## Change Size Limit for Uploading Files in SAS Studio on SAS Viya 3.5

1. Uploading file via GUI has default limitation
   - Default Size Limit:  12.5 Mega Byte (104,857,600 Bit)
   - Maximum Available Size Limit:  256 Mega Byte (2,147,483,647 Bit)
   
2. Modify ***maxUploadSize*** to change size limit for uploading files in SAS Studio   
   2.1  Log-in to ***SAS Environment Manager*** and select ***Configuration*** option
   2.2  Select View to ***All Services*** and find ***SAS Studio Viya*** service
   2.3  Find ***sas.studiov*** and set new value in ***maxUploadSize***

*. SAS Doc: [Configuration Properties: Reference (Applications)](https://go.documentation.sas.com/doc/en/calcdc/3.5/calconfig/n08025sasconfiguration0admin.htm)

*. [Data size converter](https://convertlive.com/c/convert/data-size)

---

3. [Notes for All Local Data Files](https://go.documentation.sas.com/doc/en/calcdc/3.5/datahub/n08ysqrqe3rexzn1c5gwo61noixg.htm?homeOnFail)

   You might experience long wait-times when you import large local files through a web browser. Accordingly, the CAS Management service option `maxFileUploadSize` is set to 4 Gb by default. If the import of a local file fails because the file size is too large, it might be because you have exceeded the `maxFileUploadSize` value for the CAS server. Your administrator can change the value of this option as appropriate for your site
   
   ```yaml
   1) Environment Manager -> All Services -> CAS Management Service
   2) sas.casmanagement -> maxFileUploadSize set its value to -1
   ```
   
   
