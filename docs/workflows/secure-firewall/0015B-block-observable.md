---
layout: page
title: Block Observable
permalink: /workflows/secure-firewall/0015B-block-observable
redirect_from:
  - /workflows/secure-firewall/0015B-block-observables
  - /workflows/0015B
parent: Cisco Secure Firewall
grand_parent: Workflows
---

# Block Observable
<div markdown="1">
Workflow #0015B
{: .label }

Response Workflow
{: .label }
</div>

This workflow blocks an observable on Cisco Secure Firewall by creating a judgement for it in SecureX Threat Response. Once a judgement is created, the observable will appear on a feed which Secure Firewall polls for observable information. Supported observables: `domain`, `ip`, `ipv6`, `sha256`, `url`

<div class="cisco-alert cisco-alert-info"><i class="fa fa-info-circle mr-1 cisco-icon-info"></i> This workflow has been updated to use the new "SecureX Token" account key. For more information about this, please see <a href="{{ site.baseurl }}/account-keys/securex-token">this page</a>. If you want to use legacy authentication, you can import an older version of the workflow.</div>

<div class="cisco-alert cisco-alert-info"><i class="fa fa-info-circle mr-1 cisco-icon-info"></i> This workflow is similar to workflow <a href="{{ site.baseurl }}/workflows/0065">0065</a> but works differently. Workflow 0065 adds observables to groups by making API calls directly to Secure Firewall, typically through an <a href="{{ site.baseurl }}/remote/">orchestration remote</a>. This workflow adds observables to feeds in SecureX which Secure Firewall then consumes.</div>

[<i class="fab fa-github"></i> GitHub]({{ site.github.repository_url }}/tree/Main/Workflows/0015B-SecureFirewall-BlockObservable__definition_workflow_01ML39FS25W1V5nR5onJGG8OBgXRuGG8y79){: .btn-cisco-outline }

---

## Change Log

| Date | Notes |
|:-----|:------|
| Apr 19, 2021 | - Initial release |
| Sep 10, 2021 | - Updated to use the new [system atomics]({{ site.baseurl }}/atomics/system) |
| Aug 31, 2022 | - Updated to support [SecureX Tokens]({{ site.baseurl }}/account-keys/securex-token) |

_See the [Important Notes]({{ site.baseurl }}/notes) page for more information about updating workflows_

---

## Requirements
* The following [system atomics]({{ site.baseurl }}/atomics/system) are used by this workflow:
	* Threat Response - Create Relationship
* The following atomic actions must be imported before you can import this workflow:
	* None
* The [targets](#targets) and [account keys](#account-keys) listed at the bottom of the page
* Cisco Secure Firewall

---

## Important Notes
* You must create the required indicators and feeds in SecureX Threat Response by running workflow [015A]({{ site.baseurl }}/workflows/0015A) prior to using this workflow.

---

## Workflow Steps
1. Convert the observable type to the types we use when creating indicators
1. Check if the observable type is supported. If it isn't, end the workflow and return an error
1. Generate a Threat Response access token
1. Search for the indicator for this observable type
1. Check if we found the indicator. If not, end the workflow and return an error
1. Extract the indicator's ID
1. Create a judgement in Threat Response for the observable
1. Relate the judgement to the indicator

---

## Configuration
* If you want to change the name of this workflow in the pivot menu, change its display name

---

## Targets
Target Group: `Default TargetGroup`

| Target Name | Type | Details | Account Keys | Notes |
|:------------|:-----|:--------|:-------------|:------|
| [Private_CTIA_Target]({{ site.baseurl }}/targets/default#private_ctia_target) | HTTP Endpoint | _Protocol:_ `HTTPS`<br />_Host:_ `private.intel.amp.cisco.com`<br />_Path:_ None | CTR_Credentials | Created by default |

---

## Account Keys

| Account Key Name | Type | Details | Notes |
|:-----------------|:-----|:--------|:------|
| [CTR_Credentials]({{ site.baseurl }}/account-keys/default#ctr_credentials) | SecureX Token | | See [this page]({{ site.baseurl }}/account-keys/securex-token) |
