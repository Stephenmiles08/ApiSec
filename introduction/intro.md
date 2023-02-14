What is an API?
(from redhat.com)
API stands for application programming interface, which is a set of definitions and protocols for building and integrating application software.(basically its an app that connects other apps together).
APIs let you open up access to your resources while maintaining security and control. How you open access and to whom is up to you. API security is all about good API management, which includes the use of an API gateway. Connecting to APIs, and creating applications that consume the data or functionality exposed by APIs, can be done with a distributed integration platform that connects everything—including legacy systems, and the Internet of Things (IoT).

API release policies
Private API:
This are intended for use, privately, within an organization, it gives organizations the most control over their API. These APIs are often documented less than partner APIS, if at all, and if any documentation exists it is even harder to find.

Partner API:
This are intended to be used exclusively by partners of the provider. These might be harder to find if you are not a partner. Partner APIs may be documented, but documentation is often limited to the partner.

Public API:
The API is available to everyone. This allows third parties to develop apps that interact with your API and can be a source for innovation.

Remote APIS
Remote APIs are designed to interact through a communications network.

Web APIs typically use HTTP for request messages and provide a definition of the structure of response messages. These response messages usually take the form of an XML or JSON file. Both XML and JSON are preferred formats because they present data in a way that’s easy for other apps to manipulate.

SOAP(simple object access protocol) APIs designed with SOAP use XML for their message format and receive requests through HTTP or SMTP. SOAP makes it easier for apps running in different environments or written in different languages to share information.

REST(Representational state transfer) THis is an architecture, APIs are RESTful as long as they comply with the 6 guiding constraints of a RESTful system:

* Client-server architecture: REST architecture is composed of clients, servers, and resources, and it handles requests through HTTP.

* Statelessness: No client content is stored on the server between requests. Information about the session state is, instead, held with the client.

* Cacheability: Caching can eliminate the need for some client-server interactions.

* Layered system: Client-server interactions can be mediated by additional layers. These layers could offer additional features like load balancing, shared caches, or security.

* Code on demand (optional): Servers can extend the functionality of a client by transferring executable code.

* Uniform interface: This constraint is core to the design of RESTful APIs and includes 4 facets:

	* Resource identification in requests: Resources are identified in requests and are separate from the representations returned to the client.

	* Resource manipulation through representations: Clients receive files that represent resources. These representations must have enough information to allow modification or deletion.

	* Self-descriptive messages: Each message returned to a client contains enough information to describe how the client should process the information.

	* Hypermedia as the engine of application state: After accessing a resource, the REST client should be able to discover through hyperlinks all other actions that are currently available.

link: https://www.redhat.com/en/topics/api/what-are-application-programming-interfaces

API Reconnaissance

Passive API recon: by leveraging open sourc intelligence, the goal is get API endpoints, exposed credentials, version information, API documentation  and information about the API'S business purpose. Any discovered API endpoints will become your targets later, during active reconnaissance. Credential-related information will help you test as an authenticated user or, better, as an administrator. Version information will help inform you about potential improper assets and other past vulnerabilities. API documentation will tell you exactly how to test the target API. Finally, discovering the API’s business purpose can provide you with insight into potential business logic flaws.
e.g google dorking inurl:"api', intitle:"api"
gitDorking, extension:.json, filename:filename.json, 
Begin by searching GitHub for your target organization’s name paired with potentially sensitive types of information, such as “api key,” "api keys", "apikey", "authorization: Bearer", "access_token", "secret", or “token.” 

Impoper asset management(Zombie API), If the API has not been managed well over time, then there is a chance that you could find retired endpoints that still exist even though the API provider believes them to be retired "using wayback machine"
OWASP API TOP 10

Active API Reconnaissance:
Active reconnaissance is the process of interacting directly with the target primarily through the use of scanning

amass

$ amass intel -addr [target IP addresses]

If this scan is successful, it will provide you with domain names. These domains can then be passed to intel with the whois option to perform a reverse Whois lookup:

$ amass intel -d [target domain] –whois

This could give you a ton of results. Focus on the interesting results that relate to your target organization. Once you have a list of interesting domains, upgrade to the enum subcommand to begin enumerating subdomains. If you specify the -passive option, Amass will refrain from directly interacting with your target:

$ amass enum -passive -d [target domain]

The active enum scan will perform much of the same scan as the passive one, but it will add domain name resolution, attempt DNS zone transfers, and grab SSL certificate information:

$ amass enum -active -d [target domain]

To up your game, add the -brute option to brute-force subdomains, -w to specify the API_superlist wordlist, and then the -dir option to send the output to the directory of your choice:

$ amass enum -active -brute -w /usr/share/wordlists/API_superlist -d [target domain] -dir [directory name]  

kiterunner
Kiterunner will try POST /api/v1/user/create, mimicking a more realistic request

kr brute <target> -w ~/api/wordlists/data/automated/nameofwordlist.txt

One of the coolest Kiterunner features is the ability to replay requests. Thus, not only will you have an interesting result to investigate, you will also be able to dissect exactly why that request is interesting. In order to replay a request, copy the entire line of content into Kiterunner, paste it using the kb replay option, and include the wordlist you used:

$ kr kb replay "GET     414 [    183,    7,   8]

devtools..there's a lot that can be done using this..

postman has a proxy for intercepting request

$ mitmweb 
$ mitmproxy2swagger -i <inputfile> -o <outputfile> -p 'apiendpoint' -f 'flow'
swaggereditor.io

API documentation
Review the documentation for functionality, or actions that you can take using the given API. This will be represented by HTTP methods(POST,GET,PUT,PATCH,TRACE,OPTIONS) and an endpoint. Every organization’s APIs will be different, but you can expect to find functionality related to user account management, options to upload and download data, different ways to request information, and so on. 
When making a request to an endpoint, make sure you note the request requirements. Requirements could include some form of authentication, parameters, path variables, headers, and information included in the body of the request. The API documentation should tell you what it requires of you and mention which part of the request that information belongs in

Excessive Data Exposure.
Excessive Data Exposure occurs when an API provider sends back a full data object, typically depending on the client to filter out the information that they need. From an attacker's perspective, the security issue here isn't that too much information is sent, instead, it is more about the sensitivity of the sent data
