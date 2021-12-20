
**SSL directory on Ubuntu/Debian/Gentoo
**
The correct directory place to store your_domain_com.crt and CA_Bundle.crt  is /etc/ssl/certs


Save your private keys to **/etc/ssl/private**

**SSL directory on CentOS
**
The correct directory place to store your_domain_com.crt and CA_Bundle.crt  is **/etc/pki/tls/certs**
Save your private keys to **/etc/pki/tls/private**






Locate your Apache VHost configuration file
The location of your Apache config files will vary depending on your Linux distribution's default layout. For Ubuntu with Apache2 the main VHost config file is typically located in /etc/apache2/sites-enabled/your_site_name. 

Having trouble locating your server's VHost config file? Try one of these commands to point you in the right direction.

apache2ctl -V | grep SERVER_CONFIG_FILE
apachectl -V | grep SERVER_CONFIG_FILE
grep -i -r "SSLCertificateFile" /etc/
Edit your Apache <VirtualHost> config file
Open the Apache config file for editing. Locate the <VirtualHost> container. Below is ano-frills example of a virtual host with three directives in bold that must be configured for SSL.

<VirtualHost 192="" 168="" 0="" 1:443="">
  DocumentRoot /var/www/
  SSLEngine on
  SSLCertificateFile /path/to/your_domain_com.crt
  SSLCertificateKeyFile /path/to/your_private.key
  SSLCertificateChainFile /path/to/CA_Bundle.crt
</VirtualHost> 
Adjust file names to match names of your cert files.

SSLCertificateFile is your SSL domain server certificate file: your_domain_com.crt
SSLCertificateKeyFile is the private key you created when you generated the CSR: private.key
SSLCertificateChainFile is the CA intermediate(s) bundle file: CA_Bundle.crt
Note: Some versions of Apache will not accept the SSLCACertificateFile directive. Try using SSLCertificateChainFile instead.
Test your Apache SSL configuration
After making changes to your Apache config file it is good practice to check for syntax errors before restarting. Apache will not start if there are config syntax errors. The command will return Syntax OK if there are no errors.

~$ apachectl configtest
Syntax OK
Restart Apache
You can use apachectl commands to restart Apache with SSL support.

~$ apachectl stop
~$ apachectl start
