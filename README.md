This python script is a quick hack to download data from Globus via HTTPS and without having to set up any Globus-specific tools. This is convenient if you simply want to download data from a Globus collection, and don't wish to set up their complex proprietary tooling.

# Dependencies

[globus-sdk](https://pypi.org/project/globus-sdk/) is the only dependency. The easiest way is to use GNU Guix. You will need the [guix-bioinformatics channel](https://git.genenetwork.org/guix-bioinformatics/about/).
```
guix shell -m manifest.scm
```

# Find the endpoint ID of your collection

Log in to the Globus web app, go to the `Collections` page, and find the collection you are interested in. When you click on it, you will be taken to an `Overview` page which will show the `UUID` of the collection. That is the endpoint ID.

# Authorize app and get HTTPS links for all files in your collection

Run the globus-weblinks script passing in your endpoint ID. The script will prompt you for authorization. Once authorized, it will print out HTTPS links to all your files. Write the links to a file.
```
./globus-weblinks <YOUR-ENDPOINT-ID> > weblinks
```

# Download your files using wget

You can now download your files using `wget`. But first, you will need cookies to authenticate the download. We need to extract these cookies from a browser session. This is a somewhat cumbersome process. Here's one way to do it. In the Globus web app, download any file from your collection whilst inspecting network traffic. Copy the HTTPS request for the file by right clicking it and selecting "Copy as cURL". One of the parameters in the copied curl command should be the cookie header we need. Use it with wget like so.
```
wget --header 'Cookie: mod_globus_OIDC=aloooooooooooongrandomcookiestring' -i weblinks
```

Enjoy!
