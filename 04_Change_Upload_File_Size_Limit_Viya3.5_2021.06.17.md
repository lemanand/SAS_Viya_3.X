## Change Size Limit for Uploading Files in SAS Studio on SAS Viya 3.5

1. Uploading file via GUI has default limitation
   - Default Size Limit:  12.5 Mega Byte (104,857,600 Bit)
   - Maximum Available Size Limit:  256 Mega Byte (2,147,483,647 Bit)
   
2. Modify **maxUploadSize** to change size limit for uploading files in SAS Studio   
   2.1  Log-in to **SAS Environment Manager** and select **Configuration** option
   2.2  Select View to **All Services** and find **SAS Studio Viya** service
   2.3  Find **sas.studiov** and set new value in **maxUploadSize**

---

I. SAS Doc: [Configuration Properties: Reference (Applications)](https://go.documentation.sas.com/doc/en/calcdc/3.5/calconfig/n08025sasconfiguration0admin.htm)

II. [Data size converter](https://convertlive.com/c/convert/data-size)
