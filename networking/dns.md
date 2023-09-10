# DNS

**Name Server** - is a computer designated to translate domain names into IP addresses.

Name servers can be **authoritative**, meaning that they give answers to queries about domains under their control. Otherwise, they may point to other servers, or serve cached copies of other name servers’ data.

**Zone File** - is a simple text file that contains the mappings between domain names and IP addresses. This is how the DNS system finally finds out which IP address should be contacted when a user requests a certain domain name. Zone files reside in name servers.

## SOA Records

**Records** - Within a zone file, records are kept. In its simplest form, a record is basically a single mapping between a resource and a name. These can map a domain name to an IP address, define the name servers for the domain, define the mail servers for the domain, etc.

## Root servers

DNS is, at its core, a hierarchical system. At the top of this system is what are known as **root servers**. These servers are controlled by various organizations and are delegated authority by ICANN (Internet Corporation for Assigned Names and Numbers).

> There are currently 13 root servers in operation. However, as there are an incredible number of names to resolve every minute, each of these servers is actually mirrored. The interesting thing about this set up is that each of the mirrors for a single root server share the same IP address. When requests are made for a certain root server, the request will be routed to the nearest mirror of that root server.

Root servers handle requests for information about Top-level domains.

So if a request for `www.wikipedia.org` is made to the root server, the root server will not find the result in its records. It will check its zone files for a listing that matches `www.wikipedia.org`. It will not find one.

It will instead find a record for the `org` TLD and give the requesting entity the address of the name server responsible for `org` addresses.

## TLD servers

The requester then sends a new request to the IP address (given to it by the root server) that is responsible for the top-level domain of the request.

So, to continue our example, it would send a request to the name server responsible for knowing about `org` domains to see if it knows where `www.wikipedia.org` is located.

Once again, the requester will look for `www.wikipedia.org` in its zone files. It will not find this record in its files.

However, it will find a record listing the IP address of the name server responsible for wikipedia.org.

## Domain-level name servers

At this point, the requester has the IP address of the name server that is responsible for knowing the actual IP address of the resource. It sends a new request to the name server asking, once again, if it can resolve `www.wikipedia.org`.

The name server checks its zone files and it finds that it has a zone file associated with `wikipedia.org`. Inside of this file, there is a record for the `www` host. This record tells the IP address where this host is located. The name server returns the final answer to the requester.

## Resolving name Servers

In almost all cases, the requester will be what we call a `resolving name server.`

Basically, a user will usually have a few resolving name servers configured on their computer system. The resolving name servers are usually provided by an ISP or other organizations.

When you type a URL in the address bar of your browser, your computer first looks to see if it can find out locally where the resource is located. It checks the `hosts` file on the computer and a few other locations. It then sends the request to the resolving name server and waits back to receive the IP address of the resource.

The resolving name server then checks its cache for the answer. If it doesn’t find it, it goes through the steps outlined above.

