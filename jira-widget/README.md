# Jira widget

The widget shows the number of tickets assigned  to the user and when clicked shows them in the list with some additional information. When item in the list is clicked - it opens the issue in browser.

![git](./out.gif)

## How it works

Widget uses cURL to query Jira's [REST API](https://developer.atlassian.com/server/jira/platform/rest-apis/). In order to be authenticated, widget uses a [netrc](https://ec.haxx.se/usingcurl/usingcurl-netrc) feature of the cURL, which is basically to store basic auth credentials in a .netrc file in home folder.

If you are on Atlassian Cloud, then instead of providing a password in netrc file you can set an [API token](https://confluence.atlassian.com/cloud/api-tokens-938839638.html) which is a safer option, as you can revoke/change the token at any time.

## Customization

It is possible to customize widget by providing a table with all or some of the following config parameters:

| Name | Default | Description |
|---|---|---|
| `host` | Required | Ex: _http://jira.tmnt.com_ |
| `query` | `jql=assignee=currentuser() AND resolution=Unresolved` | JQL query |
| `icon` | `~/.config/awesome/awesome-wm-widgets/jira-widget/jira-mark-gradient-blue.svg` | Path to the icon |

## Installation

Create a .netrc file in you home directory with following content:

```bash
machine turtlejira.com
login mikey@tmnt.com
password cowabunga
```

Then change file's permissions to 600 (so only you can read/write it):

```bash
chmod 600 ~/.netrc
```
And test if it works by calling the API:

```bash
curl -s -n 'https://turtleninja.com/rest/api/2/search?jql=assignee=currentuser()+AND+resolution=Unresolved'
```

Clone/download repo and use widget in **rc.lua**:

```lua
local jira_widget = require("awesome-wm-widgets.jira-widget.jira")
...
s.mytasklist, -- Middle widget
	{ -- Right widgets
    	layout = wibox.layout.fixed.horizontal,
		...
		-- default
		jira_widget({host = 'http://jira.tmnt.com'}),
		...
```
