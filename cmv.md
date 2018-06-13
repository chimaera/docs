### Chimaera Workspace Database Format Design Specifications

Written By - Chimaera Team

---
Chimaera works on top of a graph database that links all sorts of events together in a meaningful way. This type of graph database provides chimaera the flexibilty to link events related to a host together and also as it is using a custom solution, it provides speed. Doing things from the disc allows us to save the state of program at a moment and resume it later.

Chimaera Workspace format uses JSON file format as the basic structural format. Each chimaera workspace begins with a root node which usually contains the hostname or an IP Address or an IP-Range. Each workspace can contain multiple ranges. For the sake of this document, we are assuming we have 2 root nodes - 

```
1) NodeType = NodeTypeNostname, NodeValue = google.com
2) NodeType = NodeTypeIP, NodeValue = 10.10.10.70
```

Each root node basically has it's own directory where other child nodes are present. In Chimaera workspace, when the workspace is decoded, each node stands for a folder. So a trailing / (slash) in the end in the diagrams presented below means that that node is a folder. The extension for Chimaera workspace files is .cmv . 

The workspace might look something like this for this example - 

```
example.cmv/
		   |- google.com/
		   |- 10.10.10.70/
		   |- database.json
```

As apparent, `google.com` and `10.10.10.70` are two nodes that are present. The **database.json** file is a type of schema for the whole workspace. It contains some misc details on the structure of the workspace. 

The database.json file can look like this for example - 

```
{
	"nodes":[
        "google.com":{
            "Type":"Hostname"
        },
        "10.10.10.70":{
            "Type":"IP"
        }
	],
	
    "about":{
        "version":"1.0",
        "timestamp":"1528881960"
    }
}
```

- The **nodes** contain details on the root nodes currently present in the workspace. 
- The **about** section contains misc information about the chimaera program such as version as well as some misc info such as platform version, etc.

> Note - This structure is just a basic description of what we are attempting to accomplish. It can be extended furthur in much many ways.

Each of these nodes can contain subnodes, and each of those subnodes can contain other sub-sub-nodes which provide furthur details about them. Let's take the example of the Hostname block. 

Reading the hostname node, we see it has 3 subnodes called Subdomains which are apparently subdomains of the current domain and also each of those 3 subnodes contain IP addresses details inside the subdomain.json file which correspond to those subdomains. In this way, we maintain a relationship between found subdomains and domains. The additional data such as found WHOIS info, found Subdomain Takeovers, Config Files, Ports, etc all are present inside the sub-node's details.json file.

```
google.com/
			  |- corp.google.com/
			  					|- corp.google.com.json (Details file)
			  |- mail.google.com/
			  					|- mail.google.com.json 
			  |- game.google.com/
			  					|- game.google.com.json 
			  |- 10.19.16.75/ 
			  				|- 10.19.16.75.json
			  | google.com.json
```



Each of these json file contains details about a sub-node which can be a subdomain, or something else depending on the context of the root node. Now, google.com.json is a very important file as it contains information on all possible sub-nodes found.

```
> cat google.com.json
{
    "nodes":[
        "corp.google.com":{
            "Type":"Subdomain"
        },
        "mail.google.com":{
            "Type":"Subdomain"
        },
        "game.google.com":{
            "Type":"Subdomain"
        },
        "10.19.16.75":{
            "Type":"IP"
        }
    ]
}
```

Furthur changes can be made to this structure as needed. Next, we move to sub-nodes found. Let's take a look at the json file inside them.

```
> cat corp.google.com.json
{
    "nodes":{
        "10.19.12.26":{
            "Type":"IP"
        },
        "10.19.12.27":{
            "Type":"IP"
        }
        "corp.google.com/.git":{
            "Type":"Content"
        }
    }
}
```

Here, the first value in the nodes is the IP address that resolves to **corp.google.com**

> Citiation Needed - How do we maintain relationships between IP addresses and hostnames? I propose we store unique ones as sub-nodes and for hostnames that have an IP resolving to them, we simply add a sub-sub-note inside the sub-node json file. :p

