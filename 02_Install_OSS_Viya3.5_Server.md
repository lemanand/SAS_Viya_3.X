## Install OSS on SAS Viya 3.5 Server

1. **Install [Anaconda](https://www.anaconda.com/)**

	1.1 Browser [Anaconda developer download page](https://www.anaconda.com/products/individual/download-success)  and check the latest version
	
	1.2 Download and install Anacoda
	
	```bash
	# Download Anaconda
	wget https://repo.anaconda.com/archive/Anaconda3-2020.11-Linux-x86_64.sh
	# Install Anaconda
	bash Anaconda3-2020.11-Linux-x86_64.sh
	# Check installed version of Anaconda
	anacoda --version
	```
	
	1.3 Include Anacoda path to all user
	
	```bash
	cat << EOF >> /etc/profile
	export PATH=$PATH:$HOME/anaconda3/bin
	EOF
	```
	
	1.4 Activate and check modification
	
	```bash
	source /etc/profile
	echo $PATH
	```
	
	1.5 Update Anacoda
	
	```bash
	conda update --all
	```
	
	
	
2. **Install [Python 3.8](https://anaconda.org/anaconda/python)**

   ```bash
   # Check current Python version (2.7.5)
   python --version
   # Install python 3.8
   conda install -c anaconda python
   # Check installed version of Python (2.7.5 and 3.8.5)
   python --version
   python3 --version
   ```

   2.1 Set Python 3.8 as default

   ```bash
   cat << EOF >> /etc/profile
   alias python=python3
   EOF
   # Apply alias
   source /etc/profile
   # Check default Python version
   python --version
   python
   ```

3. **Install [JupyterHub](https://github.com/jupyterhub/jupyterhub)**

   ```bash
   # Install JupyterHub
   conda install -c conda-forge jupyterhub
   # Install Notebook or JupyterLab to run server locally
   conda install notebook
   conda install jupyterlab
   ```

   3.1 Verify installed packages

   ```bash
   conda list jupyterhub
   conda list notebook
   conda list jupyterlab
   ```

   3.2 Launch Jupyterhub server

   ```bash
   nohup jupyterhub --ip 172.27.64.71 --port 8080 Spawner.cmd="['jupyter-labhub']" --no-ssl &
   ```

   3.3 Set permission to access Jupyterhub server

   ```bash
   chmod 755 /root
   ```

   3.4 Access Jupyterhub server

   ```http
   http://172.27.64.71:8080/
   ```

   3.5 Create Python notebook and check Python readiness

   ```python
   import getpass
   getpass.getuser()
   ```

4. **Install Python SAS packages**

   4.1 [SAS SWAT](https://github.com/sassoftware/python-swat) package

   ```bash
   # Install and verify SAS SWAT
   conda install -c sas-institute swat
   conda list swat
   ```

   4.2 [SAS DLPy](https://github.com/sassoftware/python-dlpy) package

   ```bash
   # Install and verify SAS DLPy
   conda install -c sas-institute sas-dlpy
   conda list sas-dlpy
   ```

   4.3 Search and update Python SAS packages (If Needed)

   ```bash
   # Check currently installed version of the package
   conda list sas-dlpy
   # Search dlpy package
   conda search -c sas-institute sas-dlpy
   # Update dlpy package
   conda update -c sas-institute sas-dlpy
   # Check new installed version
   conda list sas-dlpy
   ```

   4.4 Establish CAS connection for using of SWAT and DLPy in Python 

   ```python
   # Import SWAT, DLPy and print their versions
   import swat
   import dlpy
   print("SWAT version =", swat.__version__)
   print("DLPy version =", dlpy.__version__)
   
   # Set CAS_CLIENT_SSL_CA_LIST environment variable
   import os
   os.environ['CAS_CLIENT_SSL_CA_LIST'] = '/opt/sas/viya/config/etc/SASSecurityCertificateFramework/cacerts/trustedcerts.pem'
   
   # Binary connection to CAS (5570 port, CAS protocol)
   conn = swat.CAS(hostname='172.27.64.71', port=5570, username='viyademo01', password='demopw')
   conn
   
   # Text connection to CAS (8777 port, https protocol) - Option 1
   conn = swat.CAS(hostname='172.27.64.71', port=8777, username='viyademo01', password='demopw')
   conn
   
   # Text connection to CAS (8777 port, https protocol) - Option 2
   conn = swat.CAS(hostname='https://172.27.64.71/cas-shared-default-http/', port=8777, username='viyademo01', password='demopw')
   conn
   ```

   4.5.1 Define '~/.authinfo' file

   - [Client Authentication Using an Authinfo File](https://go.documentation.sas.com/doc/en/pgmsascdc/9.4_3.5/authinfo/n0xo6z7e98y63dn1fj0g9l2j7oyq.htm#p07eral76ozzxen1aiuhg3baprxg)

   ```bash
   cat << EOF >> ~/.authinfo
   default user viyademo01 password demopw
   EOF
   
   chmod 600 ~/.authinfo
   chown viyademo01:sasadmins ~/.authinfo
   ```

   1.5.2 Simplified/Secured Establishment of CAS connection for using of SWAT and DLPy in Python 

   ```python
   # Import SWAT, DLPy and print their versions
   import swat
   import dlpy
   print("SWAT version =", swat.__version__)
   print("DLPy version =", dlpy.__version__)
   
   # Set CAS_CLIENT_SSL_CA_LIST environment variable
   import os
   os.environ['CAS_CLIENT_SSL_CA_LIST'] = '/opt/sas/viya/config/etc/SASSecurityCertificateFramework/cacerts/trustedcerts.pem'
   
   # Binary connection to CAS (5570 port, CAS protocol)
   conn = swat.CAS(hostname='172.27.64.71', port=5570)
   conn
   
   # Text connection to CAS (8777 port, https protocol) - Option 1
   conn = swat.CAS(hostname='172.27.64.71', port=8777)
   conn
   
   # Text connection to CAS (8777 port, https protocol) - Option 2
   conn = swat.CAS(hostname='https://172.27.64.71/cas-shared-default-http/', port=8777)
   conn
   ```
   
5. **Execute SAS code in Jupyter Notebook**

   5.1 Install [SAS Kernel](https://anaconda.org/anaconda/sas_kernel)

   ```bash
   # Install sas_kernel
   conda install -c anacoda sas_kernel
   # Verify installation of sas_kernel
   conda list sas_kernel
   jupyter kernelspec list
   ```

   5.2 Find location of 'sascfg.py' file that is the same location with \'__init\_.py' file.

   ```bash
   python
   import saspy
   saspy
   # <module 'saspy' from '/root/anaconda3/lib/python3.8/site-packages/saspy/__init__.py'>
   exit()
   ```

   5.3 Create and modify 'sascfg_personal.py' file

   ```bash
   cd /root/anaconda3/lib/python3.8/site-packages/saspy
   cp sascfg.py sascfg_personal.py
   ```

   - vi sascfg_personal.py
	```bash
default  = {'saspath'  : '/opt/sas/spre/home/SASFoundation/bin/sas_u8'
			}
ssh      = {'saspath' : '/opt/sas/spre/home/SASFoundation/bin/sas_en',
            'ssh'     : '/usr/bin/ssh',
            'host'    : 'remote.linux.host',
            'encoding': 'latin1',
            'options' : ["-fullstimer"]
            }
	```
	
	5.4 Run SAS code in Jupyter Notebook
	
	```SAS
	ods html5 style = statistical ;
	ods graphics /width=500 height=400 ;
	proc sgplot data = sashelp.cars ;
	    histogram msrp ;
	    density msrp ;
	run ;
	
	proc means data = sashelp.cars ; run 
	```
	
	5.5 Connection to CAS Server
	
	```SAS
	option cashost = "172.27.64.71" casport = 5570 casauthinfo = '~/.authinfo' ;
	cas mysess;
	caslib _all_ assign ;
	```
	
	5.6 Simplified Connection to CAS Service
	
	```SAS
	option cashost = "172.27.64.71" casport = 5570 ;
	cas mysess;
	caslib _all_ assign ;
	```

6. **Execute R code in Jupyter Notebook**

   6.1 Check R version and location (if it is installed)

   ```bash
   R --version
   # R version 3.2.2 (2015-08-14) -- "Fire Safety"
   which R
   # /root/anaconda3/bin/R
   ```

   6.2 Install / Update R 

   ```bash
   # R packages are available in the EPEL repositories
   yum repolist
   yum install epel-release
   
   # Install R
   yum install R -y
   
   # Check R installed several location
   which R
   find / -type f -name R
   
   # Check R versions
   /root/anaconda3/lib/R/bin/R --version
   # 3.2.2
   /usr/bin/R --version
   # 3.6.0
   
   # Re-activate PATH for /user/bin/
   source /etc/profile
   which R
   R --version
   ```

   6.3 Install [SAS R-SWAT](https://github.com/sassoftware/R-swat/releases)

   ```bash
   # Download SAS R-SWAT package
   wget https://github.com/sassoftware/R-swat/releases/download/v1.6.0/R-swat-1.6.0+vb20060-linux-64.tar.gz
   
   # Install prerequisite packages
   yum install libcurl-devel -y
   R -e 'install.packages("curl", repo="http://cran.us.r-project.org")'
   R -e 'install.packages("httr", repo="http://cran.rstudio.com/")'
   R -e 'install.packages("jsonlite", repo="http://cran.rstudio.com/")'
   # (Alternatively install in R )
   # install.packages('jsonlite',  repos="http://cran.rstudio.com/")
   
   # Install SAS R-SWAT
   R CMD INSTALL R-swat-1.6.0+vb20060-linux-64.tar.gz
   ```

   6.4 Check SAS R-SWAT package

   ```R
   # In R
   library(swat)
   # SWAT 1.6.0
   ```

   6.5 Install [IRkernel](https://irkernel.github.io/installation/)

   ```bash
   # Install IRkernel package
   R -e 'install.packages("IRkernel", repo="http://cran.rstudio.com/")'
   ```

   6.6 Make IRkernel available to Jupyter

   ```R
   # In R
   # Check IRkernel
   packageDescription('IRkernel')
   
   # Register IRkernel
   install.packages('IRkernel')
   ```

   6.7 Connection in Jupyter Notebook

   ```R
   # Load SWAT package
   libary(swat)
   
   # Binary connection to CAS (5570 port, CAS protocol)
   Sys.setenv(CAS_CLIENT_SSL_CA_LIST = '/opt/sas/viya/config/etc/SASSecurityCertificateFramework/cacerts/trustedcerts.pem')
   conn <- swat::CAS('172.27.64.71', protocol='cas', 5570)
   conn
   
   # Text connection to CAS (8777 port, https protocol)
   Sys.setenv(CAS_CLIENT_SSL_CA_LIST = '/opt/sas/viya/config/etc/SASSecurityCertificateFramework/cacerts/trustedcerts.pem')
   conn <- swat::CAS('172.27.64.71', protocol='https', 8777)
   conn
   ```
   
    6.8 (Optional) Location of R

   ```bash
   which R
   # if it is not '/usr/bin', but '/root/anaconda3/bin/' then run 'source /etc/profile'
   ```