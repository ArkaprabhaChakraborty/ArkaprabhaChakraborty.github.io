+++
showdate = true
title = "Set Up a BurpSuite Invisible Proxy"
date = 2022-03-29
url = "/blog/burpsuite_invisible_proxy"
+++

# Add a local domain mapping
Add a mapping for a domain to the loopback IP address or localhost in the default Hosts file. On Windows, this is located at `C:\Windows\System32\drivers\etc\hosts`. On Linux, this is located at `/etc/hosts`. The mapping should be as follows:
```
127.0.0.1 <domain>
```
Where the `<domain>` must be replaced with the gateway domain name. For example, if the gateway domain name is `gateway.example.com`, the mapping should be as follows:
```
127.0.0.1 gateway.example.com
```

# Configure the Burp Proxy
- Open Burp Proxy and go to the `Proxy` tab.
- Click on `Proxy Settings`, go to the Proxy Listener section and add a new Proxy Listener.
## Binding Configuration
- Set the `Bind to Port` to the desired port, in this case the port which the gateway address uses. For example, if the gateway address is `https://gateway.example.com:8443`, the port is `8443`.
- Set the `Bind to Address` to `loopback only`.
## Request Handling Configuration
- Set the `Redirect to Host` to the IP address of the gateway domain. Tip: The IP can be found out using the ```nslookup``` command.
- Set the `Redirect to Port` to the port of the gateway domain. For example, if the gateway address is `https://gateway.example.com:8443`, the port is `8443`.
- Check the `Support Invisible Proxying` checkbox for setting up the invisible proxy.
## Certificate Configuration
- Check the `Generate a CA signed certificate with a specific hostname` checkbox and set the Hostname in the provided field. This should be the same as the gateway domain name.


# Network Configuration
- Since you're already in the settings panel, Select Network` ⇒ `Connection` to open up the connection settings panel.
## Set Up Hostname Resolution Overrides
- Under connection settings, go to the `Hostname Resolution Overrides` section.
- Add the gateway domain name and the IP address of the gateway domain. 

