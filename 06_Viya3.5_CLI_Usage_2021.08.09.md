## Usage of SAS Viya 3.5 CLI (Command-Line Interface)

1. The CLI facility is found in the SAS Viya ***home/bin*** directory
   ```(bash)
   cd /opt/sas/viya/home/bin/
   ```

2. If TLS (Transport Layer Security) is enabled, ***SSL_CERT_FILE*** environment variable must be set to the path location of the ***trastedcerts.pem*** file (SAS default truststore) or the path location of your organization's certificate (internal stuststore) 

   ```(bash)
   # Define SSL_CERT_FILE environment variable and CLI path
   cat << EOF >> /etc/profile
   export SSL_CERT_FILE=/opt/sas/viya/config/etc/SASSecurityCertificateFramework/cacerts/trustedcerts.pem
   export PATH=$PATH:/opt/sas/viya/home/bin
   EOF
   
   # Apply /etc/profile changes
   source /etc/profile
   ```

3. (Optional for root) Create a default (unnamed) profile or named profile, such as ***sas-admin -- profile <profilename> init*** 

   ```(bash)
   sas-admin profile init
   
   # Enter the configuration options:
   # Service Endpoint: http://172.27.64.73
   # Output type (text | json | fulljson): text
   # Enable ANSI colored output (y/n): y
   
   # Indication of the successfull profile creation
   # Saved 'Default' profile to /root/.sas/config.json.
   ```

4. (Optional for root) Initiate the sign-in process. The default profile is used.

   By default, authentication remains active for 12 hours. You can use the ***auth logout*** command for sing out.

   ```(bash)
   sas-admin auth login
   
   # Userid: viyademo01
   # Password: ******
   # Login succeeded. Token saved.
   ```

5. Obtain CLI help information

   **sas-admin** command is SAS Administrative Command Line Interface

   ```(bash)
   sas-admin help
   
   # NAME:
   #   sas-admin - SAS Viya Command Line Interface
   # USAGE:
   #   sas-admin [global options] command [command options] [arguments...]
   ```

6. Verify the valid users of the environment

   ```(bash)
   sas-admin identities list-users
   
   # Id           Name                       Description   State
   # sasldap      SAS LDAP Service Account   <nil>         active
   # viyademo01   Viya Demo User 01          <nil>         active
   # viyademo02   Viya Demo User 02          <nil>         active
   # viyademo03   Viya Demo User 03          <nil>         active
   ```

7. Examine SAS Viya environment license information

   ```(bash)
   sas-admin licenses count
   
   # There are 110 products licensed.
   ```

8. SAS Viya Directories

   - Location of SAS Viya Binaries

     ***/opt/sas/viya/home/bin***

     

   - Location of SAS Viya Servers Configuration

     ***/opt/sas/viya/config/etc***

     - For example, CAS server (***/opt/sas/viya/config/etc/cas/default***)

     

   - Location of LOG directory for the servers and services

     ***/opt/sas/viya/config/var/log***

     - For example, CAS server (***/opt/sas/viya/config/var/log/cas/default***)

     ***/var/log/sas/viya***
     
     - ***/var/log/sas/viya*** is soft-link to ***/opt/sas/viya/config/var/log***


9. CLI to Examin the Environment

   **sas-ops** command is SAS Operations Command Line Interface

   ```(bash)
   sas-ops help
   
   # NAME:
   #   sas-ops - SAS Operations command line interface
   # USAGE:
   #   sas-ops [global options] command [arguments...]
   ```

10. View Information for the Machine

    ```(bash)
    sas-ops env
    
    # Host Information:
    #   Full hostname             : korand03.sas.com
    #   Short hostname            : korand03
    #   Consul node name          : korand03.sas.com
    # Etc.
    ```

11. View Properties of the Components of the Machine

    ```(bash)
    sas-ops info
    
    # korand03.sas.com
    #    common
    #       architecture : amd64
    #    linux
    #       kernel-release : 3.10.0-862.el7.x86_64
    ```

12. Obtain a list of CAS servers

    ```(bash)
    sas-admin cas servers list
    
    # Name                 State     Description   Host           Port   Rest Port   Rest Protocol   Tags
    # cas-shared-default   running   controller    viya.sas.com   5570   8777        https           controller
    #                                                                                                primary
    #                                                                                                restProtocol=https
    #                                                                                                appServerEnabled=true
    #                                                                                                ssl
    ```

13. Obtain a list of CAS sessions in the server

    ```(bash)
    sas-admin cas sessions list -server cas-shared-default -owner viyademo01
    
	# Name									Session ID								Owner		State		Authentication Type
    # KORANDSESSION01:Thu Aug 12 10:30:08 2021	ea89511c-3cf9-b64c-ae29-558caf366201	viyademo01	Connected		OAuth
	# KORANDSESSION02:Thu Aug 12 10:30:08 2021	08c36e5f-2d99-df4a-9967-c246e0473f33	viyademo01	Connected		OAuth
    ```

14. Delete a CAS session

    ```(bash)
    sas-admin cas sessions delete -server cas-shared-default -session-id 08c36e5f-2d99-df4a-9967-c246e0473f33
    ```

15. Backup and Restore CLI commands

    - start (start an ad hoc backup)
    - list (list all backups)
    - show (show backup details)

    ```(bash)
    sas-admin backup start -h
    sas-admin backup list -h
    sas-admin backup show -h
    ```

16. List the Viya Users

    ```(bash)
    sas-admin identities list-users
    
    # Id           Name                       Description   State
    # sasldap      SAS LDAP Service Account   <nil>         active
    # viyademo01   Viya Demo User 01          <nil>         active
    # viyademo02   Viya Demo User 02          <nil>         active
    ```

17. Create a New Custom Group and Adding Users to the Group

    ```(bash)
    # List Existing Groups
    sas-admin identities list-groups
    
    # Create new Custom Group
    sas-admin identities create-group -name ViyaFinanceGroup -id ViyaFinanceID
    
    # Add Users to the New Group
    sas-admin identities add-member -group-id ViyaFinanceID -user-member-id viyademo05
    
    # Check Added User
    sas-admin identities list-members -group-id ViyaFinanceID
    
    # Delete User from the Group
    sas-admin identities remove-member -user-member-id viyademo05 -group-id ViyaFinanceID
    
    # Delete Custom Group
    sas-admin identities delete-group -id ViyaFinanceID
    ```

18. Define CAS Library (caslib) and Load Data Using CLI

    ```(bash)
    # 1. Define caslib
    sas-admin cas caslibs create path -name test_caslib -path /korand_temp -server cas-shared-default
    # The requested caslib "test_caslib" has been added successfully.
    
    # 2. List all caslibs
    sas-admin cas caslibs list -server cas-shared-default
    
    # 3. Shows information for the specified caslib
    sas-admin cas caslibs show-info -caslib test_caslib -server cas-shared-default
    
    # 4. Load CSV, SASHDAT data to caslib
    sas-admin cas tables load -caslib test_caslib -server cas-shared-default -table=*
    # The table "TEST_CSV" has been loaded to CAS.
    # The table "TEST_TABLE" has been loaded to CAS
    
    # 5. List content in caslib
    sas-admin cas tables list -caslib test_caslib -server cas-shared-default
    # Name         Source Table Name    Scope    State
    # TEST_CSV     test_csv.csv         global   loaded
    # TEST_TABLE   test_table.sashdat   global   loaded
    
    # 6. Unload particalar table from the memory (caslib)
    sas-admin cas tables unload -caslib test_caslib -server cas-shared-default -table TEST_TABLE
    # The table "TEST_TABLE" has been unloaded from CAS.
    
    # 7. List content in caslib after unloading the table
    sas-admin cas tables list -caslib test_caslib -server cas-shared-default
    # Name         Source Table Name    Scope    State
    # TEST_CSV     test_csv.csv         global   loaded
    # TEST_TABLE   test_table.sashdat   None     unloaded
    
    # 8. Delete caslib
    sas-admin cas caslibs delete -caslib test_caslib -server cas-shared-default
    # The caslib "test_caslib" has been deleted from server "cas-shared-default"
    ```

19. CAS Library and Data Loading Using SAS Studio

    ```(sas)
    ********************************* ;
    * Caslibs and Loading Data to CAS ;
    ********************************* ;
    
    * 1. Start CAS session ;
    cas korand_sess sessopts = (caslib=casuser timeout=99 locale="en_US") ;
    
    * 2. Create Session scoped CAS Library ;
    *    Libref 8 symbols ;
    caslib mycaslib path = "/korand_temp/" type = path ;
    
    * 3. Show CAS Libs in SAS Studio ;
    caslib _all_ assign ;
    
    * 4. Load Data ;
    proc casutil ;
    	load data = sashelp.air outcaslib = "casuser" casout = "air" ;
    run ;
    
    * 5. List CAS table information from caslib ;
    proc casutil ;
    	list tables incaslib = "mycaslib" ;
    run ;
    
    * Drop caslib and terminate session ;
    caslib mycaslib drop ;
    
    cas korand_sess terminate ;
    ```

20. Settings CAS Access Control Using CLI

    ```(bash)
    # 1. Define caslib
    sas-admin cas caslibs create path -name test_caslib -path /korand_temp -server cas-shared-default
    
    # 2. Show Access Information
    sas-admin cas sources list -caslib test_caslib -server cas-shared-default
    # Source               Owner   Group   Size (kb)   Created On                  Permissions   Encryption
    # test_csv.csv         root    root    0           2021-08-17T14:46:08+09:00   -rw-r--r--
    # test_table.sashdat   root    root    5423        2021-08-17T15:01:07+09:00   -rwxr-xr-x    NONE
    # test_txt.txt         root    root    0           2021-08-17T14:46:36+09:00   -rw-r--r--
    
    # 3. Grant "readInfo" permission 
    sas-admin cas caslibs add-control -caslib test_caslib -server cas-shared-default -user viyademo01 -grant readInfo
    # The requested permission, "readInfo", and type, "grant", was applied to the identity, "viyademo01" on the caslib "test_caslib"
    
    # 4. Grant "select" permission
    sas-admin cas caslibs add-control -caslib test_caslib -server cas-shared-default -user viyademo01 -grant select
    # The requested permission, "select", and type, "grant", was applied to the identity, "viyademo01" on the caslib "test_caslib".
    
    # 5. Grant "limitedPromote" permission
    sas-admin cas caslibs add-control -caslib test_caslib -server cas-shared-default -user viyademo01 -grant limitedPromote
    # The requested permission, "limitedPromote", and type, "grant", was applied to the identity, "viyademo01" on the caslib "test_caslib"
    ```

21. Add Content Folders Using CLI

    ```(bash)
    # 1. Add "Korand_Temp" folder into "Folders/SAS Contents/Users/viyademo01"
    # sas-admin folders create -name "Korand_Temp" -parent-path "/Users/viyademo01"
    
    # Id                fbd2edc5-2ccd-47bb-b519-87299fb492db
    # Name              Korand_Temp
    # Description
    # Type              folder
    # MemberCount       0
    # ParentFolderUri   /folders/folders/cafc0fe5-70ad-4c53-a85e-da54e9564495
    #The folder was created successfully.
    
    # 2. Get a list of folders
    sas-admin folders list-members -path "/Users/viyademo01" -recursive
    ```

22. Securing the Content Folder

    ```(bash)
    # URI from Contents Pane for "Korand_Temp" folder
    # /folders/folders/fbd2edc5-2ccd-47bb-b519-87299fb492db
    
    sas-admin authorization authorize -permissions Read,Add,Remove,Update,Delete -user viyademo01 -object-uri /folders/folders/fbd2edc5-2ccd-47bb-b519-87299fb492db/** -container-uri /folders/folders/fbd2edc5-2ccd-47bb-b519-87299fb492db
    
    # Id              4cdf4f80-34e6-4135-bbc9-4a18c46101f3
    # ObjectUri       /folders/folders/fbd2edc5-2ccd-47bb-b519-87299fb492db/**
    # Principal       viyademo01
    # PrincipalType   user
    # Type            grant
    # Permissions     [read add delete remove update]
    # The authorization rule has been created.
    ```

23. "gridmon.sh" to Monitor CAS Server

    - gridmon.sh is a console or terminal application that can be run from a Linux terminal and support only Linux platforms
    - It displays data streamed from all the machines on CAS server showing information about jobs, individual machines on the server, and attached disks

    ```(bash)
    /opt/sas/viya/home/SASFoundation/utilities/bin/gridmon.sh
    
    # type '?' - To show Help Information for gridmon.sh
    # enter 'm' - To run in Machine Mode
    # enter 'd' - To run in Disk Mode
    # enter 'j' - To go back to Jobe Mode
    # enter 'q' - To Exit
    ```

24. Command of SAS Viya Operations Infrastructure

    - sas-peak 

    ```(bash)
    # The sas-peek command peeks at the local host system and services, collects their metrics, 
    # and writes as SAS Metric Event payloads to stdout
    sas-peak -h
    
    # CAS Server metrics with a metric event detail level of 2
    sas-peek cas -level 2 -format pretty
    
    # CPU metrics
    sas-peek cpu -format pretty
    ```

    - sas-ops

    ```(bash)
    # sas-ops - SAS Operations command line interface
    # It runs a specified set of tasks to collect system metrics and to publish the metric data to the SAS Message Broker
    
    sas-ops -h
    
    # View a list of the tasks
    sas-ops tasks
    
    # ops-agentsrv runs a schedule of tasks that gether information from the SAS Message Broker and 
    # runs processes to organize data in the data mart
    sas-ops tasks -name ops-agentsrv
    
    # display the status of the Environment Manager data mart management tasks
    sas-ops datamarts
    
    # Validate System Health
    # sas-ops validate - Performs validation of the deployment
    sas-ops validate -h
    
    sas-ops validate -level 3
    
    # sas-ops metrics - Stream metrics or show the most recent alerts
    # Used for System Activity Report
    sas-ops metrics
    
    # View the audit records for User Activity Report
    sas-admin audit list
    
    # List of the taks that are available and displays the frequency and a brief description for each task
    sas-ops-agent list
    ```

    - sas-check

    ```(bash)
    sas-check -h
    
    # Display WARNING and ALERT(critical) messages
    sas-check cpu -warning 1 -format pretty
    sas-check cpu -warning 1 -format line
    
    sas-check cpu -waring 1 -critical 1 -format line
    ```

    - ops-dm-admin

    ```(bash)
    # The ops-dm-admin command performs administrative functions on a SAS Viya EMI Datamart
    ops-dm-admin -h
    
    # Show information about the data mart
    ops-dm-admin show
    ```

25. Find Changed Logs

    - Return all logs with ERROR for services on the machine

    ```(bash)
    grep ERROR: /var/log/sas/viya/*/default/*
    ```

    - Return all logs that changed today

    ```(bash)
    cd /var/log/sas/viya
    ls -ltr */* | grep 20210818
    ```

    



