# Dead Simple URL Redirector for a Specific Domain

* Generates a URL redirection Chrome extension for a domain you specify
 * can only touch requests from this domain
* Minimalist so you can easily audit the code

# One Time Setup

Create a hostname mappings file. Two hosts separated by space per line, e.g. [MAPPINGS-EXAMPLE](MAPPINGS-EXAMPLE):
```
example.com b.com
test.example.com test.b.io
```
Run:
```
./SETUP example.com MAPPINGS-EXAMPLE
```

This generates `manifest.json` and `background.js`.

Next:
1. Audit the generated code
 * Start from the permissions claimed in the generated `manifest.json`
1. Go to `chrome://extensions`
1. Enable "Developer mode"
1. Load unpacked -> select directory containing this code
1. Test that the URL redirections work

# Develoment Process
* [Chrome Extensions Getting Started Tutorial](https://developer.chrome.com/extensions/getstarted)
* [chrome://extensions/](chrome://extensions/)

``` manifest.json
{
    "name": "Dead Simple URL Redirector for example.com",
    "version": "1.0",
    "description": "Redirect URLs",
    "manifest_version": 2
}
```

## background.js
``` manifest.json
{
    "name": "Dead Simple URL Redirector for example.com",
    "version": "1.0",
    "description": "Redirect URLs",
    "background": {
      "scripts": ["background.js"],
      "persistent": false
    },
    "manifest_version": 2
}
```

## WebRequest.OnBeforeRequest returning BlockingResponse

Change "background" - "persistent" to true to access the `WebRequest` API:
``` manifest.json
{
    "name": "Dead Simple URL Redirector for example.com",
    "version": "1.0",
    "description": "Redirect URLs",
    "background": {
        "scripts": ["background.js"],
        "persistent": true
    },
    "permissions": [
        "webRequest", "webRequestBlocking", "*://*example.com/"
    ],
    "manifest_version": 2
}
```

See: [manifest.json.in](manifest.json.in), [background.js.in](background.js.in)

## API References
* [webNavigation](https://developer.chrome.com/extensions/webNavigation) can't redirect requests
* [webRequest.OnBeforeRequest](https://developer.chrome.com/extensions/webRequest) callbacks returning a [BlockingResponse](https://developer.chrome.com/extensions/webRequest#type-BlockingResponse) can
* [manifest permissions](https://developer.chrome.com/extensions/declare_permissions) and [match patterns](https://developer.chrome.com/extensions/match_patterns)
* Use the standard [URL interface](https://developer.mozilla.org/en-US/docs/Web/API/URL) for URL parsing
