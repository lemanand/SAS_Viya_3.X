## SAS Base  (MEMSIZE, SORTSIZE, CASDATALIMIT) Configuration



1. **[MEMSIZE system option](https://go.documentation.sas.com/doc/en/pgmsascdc/9.4_3.5/hostunx/n09y5anvvpzrmnn0ztkyf59qgzvr.htm)**
- Specifies the limit on the total amount of virtual memory that can be used by a SAS session.  
- Default is 2G bytes.
- MAX : Specifies to set the memory size to the largest reasonable value depending on the amounts of physical memory and paging space that are available when SAS is started.

- Check MEMSIZE settings in SAS Studio

  ```SAS
  proc options option = memsize ; run ;
  /* 
  SAS (r) Proprietary Software Release V.03.05  TS1M0
  MEMSIZE=2147483648
  Specifies the limit on the amount of virtual memory that can be used during a SAS session.
  */
  ```



2. **[SORTSIZE system option](https://go.documentation.sas.com/doc/en/pgmsascdc/9.4_3.5/hostunx/n0u1mn6n3akhmvn1q4n0otf9i1c8.htm)**

- Specifies the amount of memory available to the SORT procedure. 

- Default is 1G bytes.

- MAX: Specifies the maximum addressable memory for the operating environment.

- Check SORTSIZE settings in SAS Studio

  ```SAS
  proc options option = sortsize ; run ;
  /*
  SAS (r) Proprietary Software Release V.03.05  TS1M0
  SORTSIZE=1073741824
  Specifies the amount of memory that is available to the SORT procedure.
  */
  ```

  

3. **[CASDATALIMIT system option](https://go.documentation.sas.com/doc/en/pgmsascdc/9.4_3.4/casref/p0wxc12m9hqke7n1bfcfnznsbpgo.htm)**

- Specifies the maximum number of bytes of data from a single CAS table that can be transferred from the CAS server to SAS.

- Default is 100M bytes.

- ALL: Specifies that the entire file can be read, no matter how large it is.

- Check CASDATALIMIT settings in SAS Studio

  ```SAS
  proc options option = casdatalimit ; run ;
  /* 
  SAS (r) Proprietary Software Release V.03.05  TS1M0
  CASDATALIMIT=100M 
  Specifies the maximum number of bytes that can be read from a file.
  */
  ```

  

4. **Change MEMSIZE, SORTSIZE and CASDATALIMIT system options**

- Check all system options (MEMSIZE, SORTSIZE, CASDATALIMIT) before modification

  ```SAS
  proc options option = (memsize sortsize casdatalimit) ; run ;
  /* 
  SAS (r) Proprietary Software Release V.03.05  TS1M0
  MEMSIZE=2147483648
  Specifies the limit on the amount of virtual memory that can be used during a SAS session.
  
  SORTSIZE=1073741824
  Specifies the amount of memory that is available to the SORT procedure.
  
  CASDATALIMIT=100M 
  Specifies the maximum number of bytes that can be read from a file.
  */
  ```

- Backup and then modify */opt/sas/spre/home/SASFoundation/sasv9.cfg* file

  ```bash
  # Default -MEMSIZE 2G to
  -MEMSIZE MAX
  
  # Default -SORTSIZE 1G to
  -SORTSIZE MAX
  
  # Add new line
  -CASDATALIMIT ALL
  ```

- Restart Compute Server

  ```bash
   systemctl restart sas-viya-compute-default.service
   systemctl status sas-viya-compute-default.service
  ```

- Check all system options (MEMSIZE, SORTSIZE, CASDATALIMIT) after modification

  ```SAS
  proc options option = (memsize sortsize casdatalimit) ; run ;
  /*
  SAS (r) Proprietary Software Release V.03.05  TS1M0
  MEMSIZE=63321300480
  Specifies the limit on the amount of virtual memory that can be used during a SAS session.
  
  SORTSIZE=MAX      
  Specifies the amount of memory that is available to the SORT procedure.
  
  CASDATALIMIT=ALL  
  Specifies the maximum number of bytes that can be read from a file.
  */ 
  ```

  
