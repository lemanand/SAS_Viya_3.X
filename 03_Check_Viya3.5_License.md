## Check SAS Product Licences in SAS Viya 3.5

1. **In SAS Environment Management**

   - Environment Management -> Licenced Products
   
2. **In SAS StudioV**
   - Execute SAS code 1:
   - ```sas
     cas my_sess ;
     cas my_sess listabout ;
     ```
     
   - Output 1:
   - ```bash
     Section: license
     	site = Internal_Test_VDMML_MM
     	siteNum = 70180938
     	expires = 21Mar2022:00:00:00
     	gracePeriod = 45
     	warningPeriod = 47
     ```
     
   - Execute SAS code 2:
   - Output 2:
   - ```bash
     Original site validation data
     Current version: V.03.05M0P111119
     Site name:    'Internal_Test_VDMML_MM'.
     Site number:  70180938.
     CPU A: Model name='' model number='' serial=''.
     CPU B: Model name='' model number='' serial=''.
     CPU C: Model name='' model number='' serial=''.
     Expiration:   21MAR2022.
     Grace Period:  45 days (ending 05MAY2022).
     Warning Period: 47 days (ending 21JUN2022).
     System birthday:   14MAR2021.
     Operating System:   LIN X64 .
     Product expiration dates:
     ---Base SAS Software
     etc.
     ```
   
