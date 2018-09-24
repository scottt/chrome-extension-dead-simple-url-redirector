# Dead Simple URL Redirector for a Specific Domain

* Generates an URL redirection Chrome/Firefox extension for a domain you specify
  * Specifying a domain limits the network requests the extension can see or touch
* Minimalist so you can easily audit the code

# One Time Setup

Create a hostname mappings file. Two hosts separated by space per line, e.g. (from [MAPPINGS-EXAMPLE](MAPPINGS-EXAMPLE)):
```
example.com b.com
test.example.com test.b.io
```
Run:
```
./SETUP example.com MAPPINGS-EXAMPLE
```

* This generates `manifest.json`, `background.js` and `url-redirector-<YOUR_DOMAIN>.zip`
* The zip file is for Firefox. Chrome only needs the first two files.

Next, audit the generated `manifest.json` and `background.js`. Start from the permissions claimed in `manifest.json` ([manifest.json.in](manifest.json.in))

To Load the Extension in Chrome:
1. Go to `chrome://extensions`
1. Enable "Developer mode"
1. "Load unpacked" -> Select directory containing this code
. Test that the URL redirections work

For Firefox:
1. Go to `about:debugging`
1. "Load Temporary Add-on"
1. Select `manifest.json`
1. See [Firefox.md](Firefox.md) on how to get a signed XPI file that you don't need to manually load each time you restart Firefox

# Develoment Process
* [Chrome Extensions Getting Started Tutorial](https://developer.chrome.com/extensions/getstarted)
* [chrome://extensions/](chrome://extensions/)

``` manifest.json
{
    "name": "URL Redirector for example.com",
    "version": "1.0",
    "description": "Redirect URLs",
    "manifest_version": 2
}
```

## background.js
``` manifest.json
{
    "name": "URL Redirector for example.com",
    "version": "1.0",
    "description": "Redirect URLs",
    "background": {
      "scripts": ["background.js"],
      "persistent": false
    },
    "manifest_version": 2
}
```

## Add WebRequest.OnBeforeRequest listener returning BlockingResponse

Change "background" - "persistent" to true to access the `WebRequest` API:
``` manifest.json
{
    "name": "URL Redirector for example.com",
    "version": "1.0",
    "description": "Redirect URLs",
    "background": {
        "scripts": ["background.js"],
        "persistent": true
    },
    "permissions": [
        "webRequest", "webRequestBlocking", "*://*.example.com/", "*://example.com/"
    ],
    "manifest_version": 2
}
```

``` background.js
chrome.webRequest.onBeforeRequest.addListener(listener, { urls: [ ... ] }, ['blocking'])
```

See: [manifest.json.in](manifest.json.in), [background.js.in](background.js.in)

## API References
* [webNavigation](https://developer.chrome.com/extensions/webNavigation) can't redirect requests
* [webRequest.OnBeforeRequest](https://developer.chrome.com/extensions/webRequest) callbacks returning a [BlockingResponse](https://developer.chrome.com/extensions/webRequest#type-BlockingResponse) can
* [manifest permissions](https://developer.chrome.com/extensions/declare_permissions) and [match patterns](https://developer.chrome.com/extensions/match_patterns)
* Use the standard [URL interface](https://developer.mozilla.org/en-US/docs/Web/API/URL) for URL parsing
