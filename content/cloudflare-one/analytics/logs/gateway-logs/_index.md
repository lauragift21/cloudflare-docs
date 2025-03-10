---
pcx_content_type: reference
title: Gateway activity logs
layout: single
weight: 3
---

# Gateway activity logs

{{<Aside>}}

Gateway logs will only show the public Source IP address. Private IP addresses are NAT-ed behind a public IP address.

{{</Aside>}}

Gateway activity logs show the individual DNS queries, Network packets, and HTTP requests inspected by Gateway. You can also download encrypted [SSH command logs](/cloudflare-one/policies/filtering/network-policies/ssh-logging/) for sessions proxied by Gateway.

To view Gateway activity logs, log in to your [Zero Trust dashboard](https://dash.teams.cloudflare.com/) and navigate to **Logs** > **Gateway**. Click on an individual row to investigate the event in more detail.

## Selective logging

By default, Gateway logs all events, including DNS queries and HTTP requests that are allowed and not a risk. You can choose to disable logs or only log blocked requests. To customize what type of events are recorded, log in to your [Zero Trust dashboard](https://dash.teams.cloudflare.com/) and navigate to **Settings** > **Network**. Under **Activity Logging**, indicate your DNS, Network, and HTTP log preferences.

These settings will only apply to logs displayed on the Zero Trust dashboard. [Logpush data](/cloudflare-one/analytics/logs/logpush/) is unaffected.

## DNS logs

### Explanation of the fields

{{<table-wrap>}}
| **Field**             | **Description**                                                                                                                     |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| **Query**             | The name of the domain that was queried.                                                                                            |
| **Email**             | The email address of the user who made the DNS query. This is generated by the WARP client.                                         |
| **Action**            | What [Action](/cloudflare-one/policies/filtering/dns-policies/#actions) Gateway applied to the query (for example `Allowed` or `Blocked`).       |
| **Time**              | The timestamp of the DNS query.                                                                                                     |
| **Source IP**         | The public source IP of the DNS query.                                                                                              |
| **Request type**      | The DNS query type. [This page](https://en.wikipedia.org/wiki/List_of_DNS_record_types) contains a list of all the DNS query types. |
| **Port**              | The port that was used to make the DNS query.                          |
| **Protocol type**     | The protocol that was used to make the DNS query (for example, `https`).                                                            |
| **User ID**           | The ID of the user who made the DNS query. This is generated by the WARP client.                                                    |
| **User email**        | The email address of the user who made the DNS query. This is generated by the WARP client.                                         |
| **Device ID**         | The ID of the device that made the DNS query. This is generated by the WARP client.                                                 |
| **DNS location**          | The user-configured location from where the DNS query was made.                                                                     |
| **Categories**        | Content categories that the domain belongs to.                                                                                      |
| **Resolver decision** | The reason why Gateway applied a particular **Action** to the request. Refer to the [list of resolver decisions](#resolver-decisions).|

{{</table-wrap>}}

### Resolver decisions

{{<table-wrap>}}

| **Value**  |  **Description** |
|--------------| ----------------|
| allowedByQueryName | Domain or hostname in the query matched an Allow policy.  |
| blockedByQueryName | Domain or hostname in the query matched a Block policy.                         |
| allowedRule              | IP address in the response matched an Allow policy.  |
| blockedRule               | IP address in the response matched a Block policy. |
| blockedByCategory |Domain or hostname matched a category in a Block policy.                           |
| blockedAlwaysCategory    | Domain or hostname is always blocked by Cloudflare.               |
| allowedOnNoLocation     | Allowed because query did not match a Gateway DNS location. |
| allowedOnNoPolicyMatch | Allowed because query did not match a policy.                              |
| overrideForSafeSearch  | Response was overridden by a SafeSearch policy.                               |
| overrideApplied            | Response was overridden by an Override policy.              |

{{</table-wrap>}}

## HTTP logs

{{<Aside type="note">}}

When an HTTP request results in an error, the first 512 bytes of the request are logged for 30 days for internal troubleshooting. Otherwise, HTTP bodies are not logged.

{{</Aside>}}

### Explanation of the fields

{{<table-wrap>}}

| Field              | Description                                                                                                              |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| **Host**           | The hostname in the HTTP header for the HTTP request.                                                                    |
| **Method**         | The HTTP method used for the request (e.g., GET, POST, etc.)                                                             |
| **Decision**       | The Gateway action taken based on the first rule that matched. For example: Allowed, Blocked, Bypass, etc.               |
| **Time**           | The timestamp of the HTTP request                                                                                        |
| **URL**            | The full URL of the HTTP request                                                                                         |
| **Device**         | The ID of the device that made the request. This is generated by the WARP client on the device that created the request. |
| **Referer**        | The Referer request header contains the address of the page making the request.                                          |
| **User Agent**     | The user agent header sent in the request by the originating device.                                                     |
| **File Name**      | File name string if a file transfer occurred or was attempted.                                                           |
| **HTTP version**   | The HTTP version of the origin that Gateway connected to on behalf of the user.                                          |
| **Policy details** | The policy corresponding to the decision Gateway made based on the traffic criteria of the request.                      |

{{</table-wrap>}}

### Isolate requests

When a user creates a policy to isolate traffic, the initial request that triggers isolation will be logged as an `Isolate` decision and the `is_isolated` field will return `false`. This is because that initial request is not isolated yet — but it initiates an isolated session.

Since the request is generated in an isolated browser, the result is rendered in the isolated browser and rendered back to the user securely. This request and all subsequent requests in the isolated browser are logged to include the terminal Gateway action that gets applied (e.g. Allow / Block) and the `is_isolated` field as `true`.

## Network logs

### Explanation of the fields

{{<table-wrap>}}

| Field                | Description                                                                             |
| -------------------- | --------------------------------------------------------------------------------------- |
| **Source IP**        | The IP address of the user sending the packet.                                          |
| **Destination IP**   | The IP address of the packet’s target.                                                  |
| **Source port**      | The source port number for the packet.                                                  |
| **Destination port** | The destination port number for the packet.                                             |
| **Protocol**         | The protocol over which the packet was sent.                                            |
| **SNI**              | The host whose Server Name Indication (SNI) header Gateway will filter traffic against. |
| **Policy name**      | The name of the policy corresponding to the decision Gateway made.                      |
| **Policy ID**        | The ID of the policy enforcing the decision Gateway made.                               |
| **Device ID**        | The ID of the device that sent the packet. This is generated by the WARP client.        |
| **User ID**          | The ID of the user sending the packet. This is generated by the WARP client.            |
| **User email**       | The email address of the user sending the packet. This is generated by the WARP client. |
| **Categories**       | Category or categories associated with the packet.                                      |

{{</table-wrap>}}
