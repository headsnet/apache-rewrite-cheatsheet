# Apache Rewrite Rule Cheatsheet

A collection of example Apache rewrite rules that perform various commonly needed actions.

## Redirecting WWW and non-WWW versions of a domain

Redirect to the WWW subdomain, keeping any specified page URI
```
RewriteCond %{HTTP_HOST} !^www [NC]
RewriteRule (.*)         https://%{HTTP_HOST}$1 [L,R=permanent,QSA]
```

Redirect to the non-WWW subdomain, keeping any specified page URI
```
RewriteCond %{HTTP_HOST} ^www [NC]
RewriteRule (.*)         https://some-domain.com$1 [L,R=permanent,QSA]
```

Redirect alternative versions of the domain name to the main domain name. E.g. redirect *some-domain.co.uk* and *some-domain.org* to *some-domain.com*
```
RewriteCond %{HTTP_HOST} !^some-domain.com [NC]
RewriteRule (.*)         https://%{HTTP_HOST}$1 [L,R=permanent,QSA]
```

## Redirect to SSL

Redirect the current URL to the HTTPs equivalent
```
RewriteCond %{HTTPS} off
RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI} [L,R=permanent,QSA]
```

## Redirecting specific URLs

Redirect a single URL to another URL
```
RewriteRule ^/page-foo$ /page-bar [L,R=permanent,QSA]
```

Redirect any page under a directory "/foo" to the equivalent under "/bar"
```
RewriteRule ^/foo/(.*) /bar/$1 [L,R=permanent,QSA]
```

## Removing .php extensions from URLs

This will make "/foo" to "/foo.php" on the filesystem

```
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^(.*) $1.php [QSA]
```
