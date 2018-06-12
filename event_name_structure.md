# Proposal for event name structure

Event names are what is used by Chimaera agents to exchange data with each other. Since there will likely be a lot of different events in Chimaera, it's important that we agree on a structuring that is clean and predictable to keep it manageble and easy to work with.

## Proposal

    <state>:<master-category>:<category>[:<sub-category1[...:<sub-categoryN>]]

* `<state>` **(required)**: One of:
 * `new`: The event data is new
 * `changed`: The event data has changed
 * `removed`: The event data has been removed
* `<master-category>` **(required)**: The main category that the following category is associated to
* `<category>`, `<sub-categoryX>`: Increasingly precise categories for the data type

## Examples

* `new:host:ip:v4`
* `new:host:ip:v6`
* `new:host:name` (e.g. target.com, admin.target.com, vpn.corp.target.com)
* `new:host:ip:port:tcp`
* `new:host:ip:port:udp`
* `new:host:ip:port:tcp:screenshot` (screenshot of a web page or RDP session)
* `new:host:ip:geolocation`
* `new:range:cidr`
