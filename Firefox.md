# Getting an Extension that Firefox Would Load Permanently

1. Go to [addons.mozilla.org](https://addons.mozilla.org/en-US/developers/)
1. Login with a Firefox account
1. "Submit a New Add-on"
1. When asked "Do you want your add-on to be distributed on this site?"
   Choose "on your own. Your submission will be immediately signed for self-distribution."
1. "Select a file" -> url-director-_YOUR-DOMAIN_.zip
1. Download signed `.xpi` file
1. Go to `about:addons` -> "Extensions" tab -> cog icon (setup) -> "Install Add-on From File..."
1. Select downloaded signed `.xpi` file -> "Add" to confirm

Then test if the URL redirection works.
