# Certificates for webservers

This role automatically creates SSL certificates for webservers (*.gcc.rug.nl), currently used for:
  * jenkins.gcc.rug.nl
  * docs.gcc.rug.nl

### Replacing existing certificates with new ones

On the webserver, delete the existing certificates, auto-renewal config and symlinks:

```
rm -r /etc/letsencrypt/live/${certificates_folder_name}
```

```
rm /etc/letsencrypt/renewal/${certificates_folder_name}.conf
```

```
rm /etc/pki/tls/private/localhost.key
rm /etc/pki/tls/certs/localhost.crt
rm /etc/pki/tls/certs/server-chain.crt
```

After running the above commands, you can run the certs_webserver role for the specific webserver. This will create new certificates, as well as the symlinks and auto-renewal configuration.
