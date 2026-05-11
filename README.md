<p align="center">
<img width="600" alt="osTicket Logo" src="images/osTicket Logo.png" />

</p>
<h1>osTicket: Installation</h1>
<p>
This project demonstrates the installation of a self-hosted osTicket help desk system on Windows and serves as the first part of a three-part series covering installation, configuration, and ticket lifecycle management. The focus is on deploying osTicket using modular infrastructure components rather than a single bundled installer, highlighting how the web server, scripting runtime, and database are prepared and integrated to support the application. This foundational setup establishes the environment that subsequent projects will build upon to explore system configuration and real-world ticket workflows.</p>

<h2>2. Technologies Used</h2>

<p align="left">
<img src="https://skillicons.dev/icons?i=azure,windows,php,mysql" />&nbsp;&nbsp;<img src="images/osticket-icon-dark.png" width="48"></p>

 - Internet Information Services (IIS)
 - PHP
 - MySQL
 - osTicket

<h2>3. Operating Systems</h2>

- Windows 10 Enterprise 22H2

<h2>4. High-Level Deployment Overview</h2>

- Prepare the Windows server by enabling required web server roles and features.
- Install and configure the PHP runtime and supporting dependencies.
- Deploy and configure the database backend for osTicket.
- Integrate all components and complete the osTicket web-based setup.

<h2>5. Deployment & Installation Steps</h2>
Download Installation files: https://drive.google.com/file/d/1D4v2vQkaHXbWNHPZVXbRT2oAi1U71HpK/view?usp=drive_link
<h3>1. CREATE VM IN AZURE</h3>
Create an Azure Virtual Machine with the following settings.

- Name: vm-osticket
- Operating System: Windows 10 Enterprise 22H2
- Size: 4vCPUs, 32GB Ram
- Username: labuser
- Password: Password123!

> [!NOTE]
> Passwords are shown in this tutorial for learning purposes only. In real-world environments, it is never good practice to store passwords in plain text, credentials should always be managed securely using a password manager.

<h3>1. ENABLE IIS WEB SERVICES AND CGI</h3>
Remote Desktop Connect(RDP) to the newly created VM, download the osTicket-Installation-Files.zip from the repo and unzip the whole folder unto your VM desktop. Search the start menu for "Turn Windows features on or off". In the pop-up find Internet Information Services and mark the checkbox. Expand World Wide Web > Expand Application Development Features, find CGI and mark the checkbox, and select OK and wait for features to be installed.

<details><summary>See screenshots</summary>
<img src="images/Step 2a.PNG" width="40%" >
</details> 

> [!NOTE]
> Internet Information Services (IIS) is the web server that will host the osTicket application.


<h3>2. INSTALL PHP MANAGER AND PREREQUISITES</h3>
A. In our Installation folder, find the PHP Manager Installer (PHPManagerForIIS_V1.5.0.msi) and install it.

> [!NOTE]
> osTicket is a PHP-based application, and PHP Manager allows IIS to run and manage PHP files so the application functions properly. Installing PHP and supporting runtime dependencies, ensuring version compatibility with osTicket.

B. In our Installation folder, find the Rewrite Module (rewrite_amd64_en-US.msi) and install it.

> [!NOTE]
> osTicket uses rewritten URLs to work correctly, and the Rewrite module allows IIS to handle those requests properly.

C. On the C: drive, create a new folder named <code>PHP</code>.  From our installation folder, unzip or extract the contents of <code>php-7.3.8-nts-Win32-VC15-x86.zip</code> into the C:\PHP folder. The extracted files contain the actual PHP runtime that IIS will use, while the PHP Manager we installed is used to configure and manage it.


D. In our installaion folder, find VC_redist.x86.exe and install. We need to install the Visual C++ Redistributable because PHP depends on these runtime libraries to run correctly on Windows.


E. In our installation folder, find <code>mysql-5.5.62-win32.msi</code>, and install it. MySQL is installed to provide the database where osTicket stores all tickets, users, and system data. Select Typical > Install, and then Launch Configuration Wizard after installation. Choose Standard Configuration, then select Next until reaching the Modify Security Settings screen. Enter <code>root</code> for both the username and password fields, then continue by selecting Next and finally Execute and Finish.

<details open><summary>See screenshots</summary>
<img src="images/Step 3 Ea.PNG" width="40%" > <img src="images/Step 3 Eb.PNG" width="40%" >
</details> 

<h3>3. REGISTER PHP WITH IIS</h3>
Search the start menu for "IIS", right-click and <b>Run as Admin</b>. Select Register New PHP Version. In the pop-up, select the three dots and browse to the <code>C:\PHP</code> folder, select the <code>php-cgi</code> file, and select OK.  We'll reload IIS by restarting the web server. On the left side under Connections, right-click the vm and select Stop, right-click vm again and select Start, minimize IIS Manager as we will come back to it.

<details><summary>See screenshots</summary>
<img src="images/Step 4a.PNG" width="50%" >
</details> 

<h3>5. ENABLE OSTICKET FEATURES AND ASSIGN CONFIG PERMISSIONS</h3>
In our installation folder, find <code>osTicket-v1.15.8.zip</code>, right-click and select Extract All, and then Extract. In the extracted folder, find and copy the upload folder to destination <code>C:inetpub\wwwroot</code>. Rename the upload folder to "osTicket" (no space, and uppercase T).

<details><summary>See screenshots</summary>
<img src="images/Step 5a.PNG" width="50%" >
</details> 

Reload IIS by restarting the web server again, stop and start instructions from Step 3 and minimize IIS Manager. Now in your web browser, navigate to "http://localhost/osticket/setup/".

<details><summary>See screenshots</summary>
<img src="images/Step 5b.PNG" width="50%" >
</details> 

Notice some features are not enabled here, we'll enable them next. Back in our IIS Manager, expand Site, expand Default Web Site and select folder osTicket.  Double-click PHP Manager, and select Enable or Disable an extension. 

- Enable php_imap.dll
- Enable php_intl.dll
- Enable php_opcache.dll

<details><summary>See screenshots</summary>
<img src="images/Step 5c.PNG" width="33%" > <img src="images/Step 5d.PNG" width="33%" ><img src="images/Step 5e.PNG" width="33%" >
</details> 

Refresh the osTicket site in the web browser and notice the changes.
Now we will be assigning permissions. Go to folder <code>C:\inetpub\wwwroot\osTicket\include</code> and find file <code>ost-sampleconfig.php</code>. Rename this file to <code>ost-config.php</code>. Now right-click file and select Properties. In Security tab, go to Advanced. Select Disable inhertiance > Remove all inhertied permissions. We are stripping away all current permissions here.

<details><summary>See screenshots</summary>
<img src="images/Step 5f.PNG" width="50%" >
</details> 


And then  we'll add permissions, select Add, Select Principles, in the object name text field, type, "Everyone" and then Check Names to underline our group and select OK. For basic permissions, select Full Control and OK.

<details><summary>See screenshots</summary>
<img src="images/Step 5g.PNG" width="33%" > <img src="images/Step 5h.PNG" width="33%" >
</details> 

> [!NOTE]
> Assigning Everyone permissions to ost-config.php is insecure because it allows unrestricted access to a sensitive configuration file. This is done temporarily in this lab to avoid installation issues; permissions should be restricted after setup in real environments.

<h3>6. INSTALL HEIDISQL AND CONFIGURE SQL</h3>

Back in the web browser, we will continue the osTicket setup, select Continue >> near the bottom. 
In System Settings, enter the help desk name and default email. In Admin User, enter admin name and admin email address, for username and password, we will set it to adminuser and <code>Password123!</code>. 

Before we select Install Now, we will need to configure our SQL and create the database and connection that osTicket will use.

<details><summary>See screenshots</summary>
<img src="images/Step 6a.PNG" width="50%" >
</details> 

Back in the installation folder, find <code>HeidiSQL_12.3.0.6589_Setup.exe</code>, and install it with all default settings, and select Finish to launch HeidiSQL. Select Skip, in this Session Manager window, Select +New, and type <code>root</code> for the password here, and select Open. Right-click the Unnamed session, and select Create new, and select Database. Enter for Name: osTicket (no space and capital T), and select OK.

<details><summary>See screenshots</summary>
<img src="images/Step 6b.PNG" width="40%" > <img src="images/Step 6c.PNG" width="40%" >
</details> 

Back in the web browser, we will continue the osTicket setup. Enter the following
- MySQL Database: osTicket
- MySQL Username: root
- MySQL Password: root
- **Select Install Now**

<details><summary>See screenshots</summary>
<img src="images/Step 6d.PNG" width="50%" >
</details> 

<h3>7. VERIFY INSTALLTION AND FUNCTIONAILTY</h3>
Congratulations, refer to the screenshots to ensure functionality!
For Admin/Analyst/Agent Login: http://localhost/osTicket/scp/login.php
For End User to create tickets: http://localhost/osTicket/

<details><summary>See screenshots</summary>
<img src="images/Step 7a.PNG" width="30%" > <img src="images/Step 7b.PNG" width="30%" > <img src="images/Step 7c.PNG" width="30%" >
</details> 

