## Setup Local YUM Repository in CentOS/RHEL 7

Source: [CentOS / RHEL 7 : How to setup yum repository using locally mounted DVD](https://www.thegeekdiary.com/centos-rhel-7-how-to-setup-yum-repository-using-locally-mounted-dvd/)

1. Mount the RHEL 7 installation media ISO to **/mnt** directory

   ```(bash)
   mount -o loop rhel-server-7.7-x86_64-dvd.iso /mnt
   ls /mnt
   ```

2. Copy the **media.repo** file from **/mnt** to **/etc/yum.repos.d/** and name it as **rhel7.repo**

   ```(bash)
   cp /mnt/media.repo /etc/yum.repos.d/rhel7.repo
   ```

3. Give appropriate permissions to the repository file

   ```(bash)
   chmod 644 /etc/yum.repos.d/rhel7.repo

4. Modify the repo file and change the parameter **gpgcheck=0** to **gpgcheck=1** and add below 3 lines to the same file

   ```(bash)
   enabled=1
   baseurl=file:///mnt/
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
   ```

5. Once you have done all the changes, the final repo file should look like below (**media-id** will be different)

   ```(bash)
   name=Red Hat Enterprise Linux 7.7
   mediaid=9859238196.834790
   metadata_expire=-1
   gpgcheck=1
   cost=500
   enabled=1
   baseurl=file:///mnt/
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
   ```

6.  Clear the related caches by **yum clean all** and **subscription-manager clean** 

     ```(bash)
   yum clean all
   subscription-manager clean
   ```

7. Verify if you can list out the packages from the repo you just created

   ```(bash)
    yum --noplugins list
    yum repolist -v
   ```

