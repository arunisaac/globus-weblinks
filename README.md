This python script is a quick hack to download data from Globus via HTTPS and without having to set up any Globus-specific tools. This is convenient if you simply want to download data from a Globus collection, and don't wish to set up their complex proprietary tooling.

# How it works

We use [globus-sdk](https://pypi.org/project/globus-sdk/) to traverse the file tree in our Globus collection and obtain HTTPS links for them. Then, we download the files authenticating using cookies extracted from the Globus web app.

Really, this is hacky abuse of the single file HTTPS download links that the Globus web app provides. Doing everything through globus-sdk would be preferable, but I haven't figured out how. And, it works for now.

# Dependencies

[globus-sdk](https://pypi.org/project/globus-sdk/) and requests are the only dependencies. The easiest way to obtain them is to use GNU Guix. You will need the [guix-bioinformatics channel](https://git.genenetwork.org/guix-bioinformatics/about/).
```
guix shell -m manifest.scm
```

# Find the endpoint ID of your collection

Log in to the Globus web app, go to the `Collections` page, and find the collection you are interested in. When you click on it, you will be taken to an `Overview` page which will show the `UUID` of the collection. That is the endpoint ID.

# Extract cookies from browser

You will need cookies to authenticate the HTTPS download. You need to extract these cookies from a browser session. This is a somewhat cumbersome process. Here's one way to do it. In the Globus web app, download any file from your collection whilst inspecting network traffic. Copy the HTTPS request for the file by right clicking it and selecting "Copy as cURL". One of the parameters in the copied curl command should be the cookie header we need. Put it in a file `cookies.json` like so:
```
{
  "mod_globus_OIDC": "aloooooooooooongrandomcookiestring"
}
```

# Download your files

Now, all that remains is to download your files. Do it like so:
```
./globus-weblinks YOUR-ENDPOINT-ID cookies.json
```
Enjoy!
