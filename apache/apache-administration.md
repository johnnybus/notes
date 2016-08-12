# Apache Administration
### Instalation from source
###### Download the source tar
    wget http://mirrors.fe.up.pt/pub/apache//httpd/httpd-2.4.23.tar.gz
###### For the installation, make sure that the following packages are installed
    yum install -y apr-util.x86_64 apr.x86_64 libapreq2.x86_64 apr-devel.x86_64 apr-util-devel.x86_64 pcre-devel
###### Configure Apache source tree for the current platform and personal requirements, the `--prefix` tag will set the desired installation location
    ./configure --prefix=/app/apache24/
###### Build the various parts which form the Apache package
    make
###### Install it, it will be installed on the `--prefix` location
    make install
###### Configuration file of Apache is located at
    <PREFIX>/conf/httpd.conf
###### To test it, just try to start and stop the server
    <PREFIX>/bin/apachectl -k start
    <PREFIX>/bin/apachectl -k stop

### Apache configuration
###### Most used tags
- DocumentRoot - Directory that forms the main document tree visible from the web

#### Virtual Hosts
The term Virtual Host refers to the practice of running more than one web site (such as company1.example.com and company2.example.com) on a single machine. Virtual hosts can be:
- IP-based (different IP address for every web site)
- name-based (multiple names running on each IP address)

###### By default will be available this tag on `<PREFIX>/conf/httpd.conf`, uncomment it. The Virtual Hosts will be placed on `conf/extra/httpd-vhosts.conf`
    # Virtual hosts
    Include conf/extra/httpd-vhosts.conf
###### Its needed to set the type of Virtual Host and port
    NameVirtualHost \*:80
###### Virtual host (IP-based) example:
    <VirtualHost 172.20.30.40:80>
        ServerAdmin webmaster@www1.example.com
        DocumentRoot "/www/vhosts/www1"
        ServerName www1.example.com
        ErrorLog "/www/logs/www1/error_log"
        CustomLog "/www/logs/www1/access_log" combined
    </VirtualHost>

    <VirtualHost 172.20.30.50:80>
        ServerAdmin webmaster@www2.example.org
        DocumentRoot "/www/vhosts/www2"
        ServerName www2.example.org
        ErrorLog "/www/logs/www2/error_log"
        CustomLog "/www/logs/www2/access_log" combined
    </VirtualHost>
###### Virtual host (name-based) example:
    <VirtualHost *:80>
        # This first-listed virtual host is also the default for *:80
        ServerName www.example.com
        ServerAlias example.com
        DocumentRoot "/www/domain"
    </VirtualHost>

    <VirtualHost \*:80>
        ServerName other.example.com
        DocumentRoot "/www/otherdomain"
    </VirtualHost>

#### Authentication and Authorization
- ** Authentication **
    - mod_auth_basic
    - mod_auth_digest
- ** Authentication Provider **
    - mod_authn_anon
    - mod_authn_dbd
    - mod_authn_dbm
    - mod_authn_file
    - mod_authn_ldap
- ** Authorization **
    - mod_authz_ldap
    - mod_authz_dbm
    - mod_authz_groupfile
    - mod_authz_host
    - mod_authz_owner
    - mod_authz_user

##### Authentication over htaccess
To configure the server authentication, his configuration can be placed on the `httpd.conf` file or by using an `.htaccess` file
###### First its needed to create an password file, this file should be somewhere not accessible form the web.
    htpasswd -c </path/to/password/file> <user>
###### Add the authentication configuration (.htacces or httpd.conf)
    AuthType Basic
    AuthName "Restricted Files"
    # (Following line optional)
    AuthBasicProvider file
    AuthUserFile "/usr/local/apache/passwd/passwords"
    Require user rbowen

- ** AuthType ** - The AuthType directive selects that method that is used to authenticate the user. The most common method is Basic, and this is the method implemented by mod_auth_basic. It is important to be aware, however, that Basic authentication sends the password from the client to the server unencrypted. This method should therefore not be used for highly sensitive data, unless accompanied by mod_ssl. Apache supports one other authentication method: AuthType Digest. This method is implemented by mod_auth_digest and was intended to be more secure.

- ** AuthArea ** - Sets the Realm to be used in the authentication. The realm serves two major functions. First, the client often presents this information to the user as part of the password dialog box. Second, it is used by the client to determine what password to send for a given authenticated area.

- ** AuthBasicProvider ** - Is, in this case, optional, since file is the default value for this directive. You'll need to use this directive if you are choosing a different source for authentication, such as `mod_authn_dbm` or `mod_authn_dbd`.

- ** AuthUserFile ** - This directive sets the path to the password file that we just created with htpasswd. If you have a large number of users, it can be quite slow to search through a plain text file to authenticate the user on each request. Apache also has the ability to store user information in fast database files. The `mod_authn_dbm` module provides the ** AuthDBMUserFile ** directive. These files can be created and manipulated with the `dbmmanage` and `htdbm` programs. Many other types of authentication options are available from third party modules in the Apache Modules Database.

- ** Require ** - Provides the authorization part of the process by setting the user that is allowed to access this region of the server. In the next section, we discuss various ways to use the Require directive. Some ** Require ** options provided by `mod_authz_core`:

###### Access is allowed unconditionally.
    Require all granted
###### Access is denied unconditionally.
    Require all denied
###### Access is allowed only if one of the given environment variables is set.
    Require env env-var [env-var] ...
###### Access is allowed only for the given HTTP methods.
    Require method http-method [http-method] ...
###### Access is allowed if expression evaluates to true.
    Require expr expression

Some of the allowed syntaxes provided by `mod_authz_user`, `mod_authz_host`, and `mod_authz_groupfile` are:
###### Only the named users can access the resource.
    Require user userid [userid] ...
###### Only users in the named groups can access the resource.
    Require group group-name [group-name] ...
###### All valid users can access the resource.
    Require valid-user
###### Clients in the specified IP address ranges can access the resource.
    Require ip 10 172.20 192.168.2


### Modules Instalation (from source)
Modules are stored at `<PREFIX>/modules/`
###### Configure Apache source tree for the current platform and personal requirements, the `--prefix` tag will set the desired installation location, `--enable-mods-shared` defines a list of modules to be enabled and build as dynamic shared modules (these module have to be loaded dynamically by using the LoadModule directive).
    ./configure --prefix=/app/apache24/ --enable-mods-shared="rewrite headers"
###### Build the various parts which form the Apache package
    make
###### Install it, it will be installed on the `--prefix` location. This process will generate an file `.so` (the module it self)
    make install
###### To enable a module
    a2enmod <module name>
###### To disable a module
    a2dismod <module name>
###### After enable or disable a module, apache configuraions must be reloaded
    <PREFIX>/bin/apachectl -k reload
