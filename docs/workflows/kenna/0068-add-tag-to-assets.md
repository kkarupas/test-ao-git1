---
layout: page
title: Add Tag to Assets
permalink: /workflows/kenna/0068-add-tag-to-assets
redirect_from:
  - /workflows/0068
parent: Kenna Security
grand_parent: Workflows
---

# Add Tag to Assets
<div markdown="1">
Workflow #0068
{: .label }

Response Workflow
{: .label }
</div>

This workflow searches Kenna for assets matching the observable provided and adds a tag to them. A casebook is created for each asset if "Create Casebook" is set to true. Supported observables: `ip`, `hostname`, `mac_address`

[<i class="fab fa-github"></i> GitHub]({{ site.github.repository_url }}/tree/Main/Workflows/0068-Kenna-AddTagToAssets__definition_workflow_01YAVEJRBRIMK03XhV2kP4BgpGep4vKJ2Gj){: .btn-cisco-outline }

---

## Change Log

| Date | Notes |
|:-----|:------|
| Aug 4, 2022 | - Initial release |

_See the [Important Notes]({{ site.baseurl }}/notes) page for more information about updating workflows_

---

## Requirements
* The following [system atomics]({{ site.baseurl }}/atomics/system) are used by this workflow:
	* Kenna - Add Tag to Asset
	* Kenna - Search Assets
	* Threat Response - Create Casebook
	* Threat Response - Generate Access Token
* The following atomic actions must be imported before you can import this workflow:
	* None
* The [targets](#targets) and [account keys](#account-keys) listed at the bottom of the page
* Kenna Security

---

## Workflow Steps
1. Fetch global variables
1. Build the query string based on the observable provided
1. Search for matching assets
1. Convert the asset list to a table
1. Fetch an access token for Threat Response
1. For each asset:
	* Add the tag to the asset
	* If creating casebooks is enabled, create a casebook

---

## Configuration
* Add your Kenna API token to the `API Token` local variable (or, if you have an API key in a global variable already, set the local variable to the global's value using the `Fetch Global Variables` group at the beginning of the workflow)
* Set the `Create Casebook` local variable to `true` or `false` depending on whether or not you want a casebook created for each tagged asset
* Set the `Kenna Instance URL` local variable to the URL of your Kenna instance (for example: `customer.kennasecurity.com`)
* Set the `Tag to Add` local variable to the tag you want added to matching assets in Kenna
* If you want to change the name of this workflow in the pivot menu, change its display name

---

## Targets
Target Group: `Default TargetGroup`

| Target Name | Type | Details | Account Keys | Notes |
|:------------|:-----|:--------|:-------------|:------|
| [CTR_For_Access_Token]({{ site.baseurl }}/targets/default#ctr_for_access_token) | HTTP Endpoint | _Protocol:_ `HTTPS`<br />_Host:_ `visibility.amp.cisco.com`<br />_Path:_ `/iroh` | CTR_Credentials | Created by default |
| Kenna_Target | HTTP Endpoint | _Protocol:_ `HTTPS` <br/> _Host:_ `api.kennasecurity.com` <br/> _Path_: None | None | |
| [Private_CTIA_Target]({{ site.baseurl }}/targets/default#private_ctia_target) | HTTP Endpoint | _Protocol:_ `HTTPS`<br />_Host:_ `private.intel.amp.cisco.com`<br />_Path:_ None | None | Created by default |

---

## Account Keys

| Account Key Name | Type | Details | Notes |
|:-----------------|:-----|:--------|:------|
| [CTR_Credentials]({{ site.baseurl }}/account-keys/default#ctr_credentials) | HTTP Basic Authentication | _Username:_ Client ID<br />_Password:_ Client Secret | Created by default |