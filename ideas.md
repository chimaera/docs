# Ideas

## Core

- **Modules:** The tool will be structured into small modules that will perform a specific task on data. The modules might produce more data, or they might enrich data, but they should be atomic and perform one thing well
- **Event based core:** Modules will consume and emit events on a bus. One module
might emit an event about a new subdomain being discovered, which can be consumed
by other modules which will perform arbitrary actions on this newly discovered
subdomain, e.g. scan for open ports. Information on open ports will also be emited
as an event on the bus in order to be picked up by other modules and so on.
- **Event bus orchestration:** In order to avoid duplication of work between modules, we will need some kind of state component which modules can use to *reserve* incoming events on a first come, first served basis
- **Event ancestry:** A hierachy of events should somehow be maintained in order to be able to tell how a specific piece of data was found. e.g: `new:domain{admin.target.com}` --> `new:port{8080,admin.target.com}` --> `new:endpoint{/robots.txt,8080,admin.target.com}` --> . . .
Having information on the parent/child relationship between events/data would make it possible to do some interesting stuff with data graphs, etc.

## Features

- **Subdomain discovery:** Subdomains could be discovered on domains through various methods, both passive and active
- **Port scanning:** Discovered hosts could be port scanned for a configurable list of ports
- **Host Discovery:** Discover alive ip addresses when specified with an IP range
- **Website screenshots:** Discovered web servers could be screenshotted for easy analysis
- **Remote Desktop screenshots:** Discovered RDP hosts could be screenshotted for easy analysis
- **Grouping of similar sites:** Similar sites could be grouped together by analysing returned HTTP responses in a *fuzzy* manner to tolerate smaller differences
- **Banner grapping:** Banners from discovered ports could be captured and potentially fingerprinted
- **WHOIS lookup:** Discovered hosts and domains could be looked up in WHOIS to gather information on network range and owner, etc.
- **Host geolocation:** Discovered hosts could be geolocated through a web service or by looking up in an internal database
- **Module aggressiveness rating:** Modules could have an *aggressiveness* rating associated with them in order for users to specify how aggressive/noisy or stealthy they want their assessment to be. Some modules perform passive things, while others perform port scanning and other direct interaction with target infrastructure which might not always be desirable
- **Scoping of IP ranges and domains:** The tool has the potential to *run wild* which can be dangerous on professional engagements with a defined scope. It should be possible to limit the tool's reach to specific IP ranges and domains
- **Config File Discovery:** Discovered hosts can be searched for interesting endpoints and files leading to easy wins.
- **Platform Identification:** Discovered hosts should be searched using a database or something to find the software running on them.
- **Known exploit scanning:** (TBD) Discovery some common vulnerabilities that may be present based on the host.
- **SSL Checks:** Check for common SSL Vulnerabilities & Also other stuff like Heartbleed, Poodle, etc
- **VHostScan:** Check for VHosts in found HTTP websites. 

## Challenges

## Reference
