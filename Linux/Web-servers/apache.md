# Apache/Httpd
Install and configure **Apache** on **CentOS**.  

## Installation
Update repo and install apache.  
```sh
sudo dnf update -y && \
sudo dnf install httpd -y
```


## Configure
Change `/etc/httpd/conf/httpd.conf` file and edit listen to port `8088`.
Check current port.  
```sh

```

Change port number using **sed**.  
```sh
sed -i "s/80/8088/g" /etc/apache2/ports.conf
```

Start and enable **Apache**.  
```sh
sudo systemctl enable httpd && \
sudo systemctl start httpd && \
sudo systemctl status  httpd
```





With security
```conf
# Secure and Optimized Apache Configuration
ServerRoot "/etc/httpd"
Listen 5001

# Load essential security and performance modules
Include conf.modules.d/*.conf
LoadModule headers_module modules/mod_headers.so
LoadModule rewrite_module modules/mod_rewrite.so
LoadModule ssl_module modules/mod_ssl.so
LoadModule deflate_module modules/mod_deflate.so
LoadModule expires_module modules/mod_expires.so
LoadModule security2_module modules/mod_security2.so

# Basic server settings
User apache
Group apache
ServerAdmin root@localhost

# Hide server information (security)
ServerTokens Prod
ServerSignature Off

# Security Headers
Header always set X-Content-Type-Options nosniff
Header always set X-Frame-Options DENY
Header always set X-XSS-Protection "1; mode=block"
Header always set Referrer-Policy "strict-origin-when-cross-origin"
Header always set Permissions-Policy "geolocation=(), microphone=(), camera=()"
Header always set Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'"

# Remove server version from headers
Header unset Server
Header always unset X-Powered-By

# Timeout settings for security
Timeout 60
KeepAlive On
MaxKeepAliveRequests 100
KeepAliveTimeout 15

# Security: Disable dangerous HTTP methods
<LimitExcept GET POST HEAD>
    Require all denied
</LimitExcept>

# Root directory - maximum security
<Directory />
    AllowOverride none
    Require all denied
    Options None
    # Prevent access to version control directories
    <DirectoryMatch "^/.*/\.(git|svn|hg|bzr)/">
        Require all denied
    </DirectoryMatch>
</Directory>

# Document root with security restrictions
DocumentRoot "/var/www/html"
<Directory "/var/www">
    AllowOverride None
    Options -Indexes -Includes -ExecCGI
    Require all granted
    
    # Block access to backup and temporary files
    <FilesMatch "\.(bak|backup|old|orig|tmp|temp|log|~)$">
        Require all denied
    </FilesMatch>
    
    # Block access to configuration files
    <FilesMatch "\.(conf|ini|cfg|config)$">
        Require all denied
    </FilesMatch>
</Directory>

<Directory "/var/www/html">
    Options -Indexes -Includes -ExecCGI +FollowSymLinks
    AllowOverride None
    Require all granted
    
    # Prevent execution of PHP in uploads directory
    <FilesMatch "\.php$">
        <RequireAll>
            Require all granted
            Require not expr "%{REQUEST_URI} =~ m#^/uploads/#"
        </RequireAll>
    </FilesMatch>
</Directory>

# Secure blog directory
Alias /blog/ "/var/www/html/blog/"
<Directory "/var/www/html/blog/">
    Options -Indexes -Includes -ExecCGI +FollowSymLinks
    AllowOverride FileInfo Limit
    Require all granted
    
    # Rate limiting for blog
    <RequireAll>
        Require all granted
        # Basic rate limiting can be added here with mod_rate_limit if available
    </RequireAll>
</Directory>

# Secure games directory
Alias /games/ "/var/www/html/games/"
<Directory "/var/www/html/games/">
    Options -Indexes -Includes -ExecCGI +FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>

# Default directory index
<IfModule dir_module>
    DirectoryIndex index.html index.php index.htm
</IfModule>

# Security: Block access to sensitive files
<Files ~ "^\.">
    Require all denied
</Files>

<FilesMatch "^(\.htaccess|\.htpasswd|\.user\.ini|php\.ini|\.env|composer\.(json|lock)|package\.(json|lock))$">
    Require all denied
</FilesMatch>

# Block access to include files
<FilesMatch "\.(inc|conf)$">
    Require all denied
</FilesMatch>

# Enhanced logging with security focus
ErrorLog "logs/error_log"
LogLevel warn

<IfModule log_config_module>
    # Enhanced logging format with security info
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %D %{X-Forwarded-For}i" combined
    LogFormat "%h %l %u %t \"%r\" %>s %b" common
    
    <IfModule logio_module>
        LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %I %O %D" combinedio
    </IfModule>
    
    CustomLog "logs/access_log" combined
    
    # Security log for suspicious activity
    LogFormat "%h %t \"%r\" %>s \"%{User-Agent}i\"" security
    CustomLog "logs/security_log" security env=suspicious
</IfModule>

# Secure CGI configuration
<IfModule alias_module>
    ScriptAlias /cgi-bin/ "/var/www/cgi-bin/"
</IfModule>

<Directory "/var/www/cgi-bin">
    AllowOverride None
    Options +ExecCGI -Indexes -Includes
    Require all granted
    
    # Only allow specific CGI file extensions
    <FilesMatch "\.(cgi|pl|py|sh)$">
        SetHandler cgi-script
    </FilesMatch>
</Directory>

# MIME type configuration with security considerations
<IfModule mime_module>
    TypesConfig /etc/mime.types
    
    # Secure MIME type handling
    AddType application/x-compress .Z
    AddType application/x-gzip .gz .tgz
    AddType text/html .shtml
    AddOutputFilter INCLUDES .shtml
    
    # Prevent execution of scripts in uploads
    <LocationMatch "/uploads/">
        AddType text/plain .php .php3 .phtml .pht .pl .py .jsp .asp .sh
    </LocationMatch>
</IfModule>

# Character encoding
AddDefaultCharset UTF-8

# MIME Magic
<IfModule mime_magic_module>
    MIMEMagicFile conf/magic
</IfModule>

# Performance Optimizations

# Enable compression for better performance
<IfModule deflate_module>
    AddOutputFilterByType DEFLATE text/plain
    AddOutputFilterByType DEFLATE text/html
    AddOutputFilterByType DEFLATE text/xml
    AddOutputFilterByType DEFLATE text/css
    AddOutputFilterByType DEFLATE text/javascript
    AddOutputFilterByType DEFLATE application/xml
    AddOutputFilterByType DEFLATE application/xhtml+xml
    AddOutputFilterByType DEFLATE application/rss+xml
    AddOutputFilterByType DEFLATE application/javascript
    AddOutputFilterByType DEFLATE application/x-javascript
    AddOutputFilterByType DEFLATE application/json
    
    # Don't compress images and other binary files
    SetEnvIfNoCase Request_URI \
        \.(?:gif|jpe?g|png|zip|gz|tgz|bz2|tbz|mp3|ogg|mp4|avi)$ no-gzip dont-vary
</IfModule>

# Browser caching for static content
<IfModule expires_module>
    ExpiresActive On
    
    # Images
    ExpiresByType image/jpg "access plus 1 month"
    ExpiresByType image/jpeg "access plus 1 month"
    ExpiresByType image/gif "access plus 1 month"
    ExpiresByType image/png "access plus 1 month"
    ExpiresByType image/webp "access plus 1 month"
    ExpiresByType image/svg+xml "access plus 1 month"
    
    # CSS and JavaScript
    ExpiresByType text/css "access plus 1 month"
    ExpiresByType application/pdf "access plus 1 month"
    ExpiresByType application/javascript "access plus 1 month"
    ExpiresByType application/x-javascript "access plus 1 month"
    ExpiresByType text/javascript "access plus 1 month"
    
    # Fonts
    ExpiresByType font/ttf "access plus 1 year"
    ExpiresByType font/woff "access plus 1 year"
    ExpiresByType font/woff2 "access plus 1 year"
    
    # HTML files (shorter cache)
    ExpiresByType text/html "access plus 2 hours"
    
    # Default cache time
    ExpiresDefault "access plus 1 week"
</IfModule>

# Enable SendFile for better performance
EnableSendfile on

# Optimize file handling
FileETag None

# Prevent access to .git directories and other VCS
<DirectoryMatch "\.git">
    Require all denied
</DirectoryMatch>

<DirectoryMatch "\.svn">
    Require all denied
</DirectoryMatch>

# Rate limiting (basic) - consider using mod_rate_limit or fail2ban
<IfModule mod_reqtimeout.c>
    RequestReadTimeout header=20-40,MinRate=500 body=20,MinRate=500
</IfModule>

# SSL/TLS Configuration
<IfModule ssl_module>
    # SSL Engine off by default, enable when you have certificates
    # SSLEngine on
    # SSLCertificateFile /path/to/certificate.crt
    # SSLCertificateKeyFile /path/to/private.key
    
    # Modern SSL configuration
    # SSLProtocol all -SSLv2 -SSLv3 -TLSv1 -TLSv1.1
    # SSLCipherSuite ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384
    # SSLHonorCipherOrder on
    # SSLCompression off
    # SSLSessionTickets off
    
    # HSTS (HTTP Strict Transport Security)
    # Header always set Strict-Transport-Security "max-age=63072000; includeSubDomains; preload"
</IfModule>

# Additional security with ModSecurity (if available)
<IfModule security2_module>
    # Include ModSecurity configuration
    # IncludeOptional /etc/httpd/modsecurity.d/*.conf
    # IncludeOptional /etc/httpd/modsecurity.d/activated_rules/*.conf
</IfModule>

# Include additional configurations
IncludeOptional conf.d/*.conf
```