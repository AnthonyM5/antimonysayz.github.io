---
layout: post
title:      "RESTful Architecture"
date:       2020-02-22 20:10:00 +0000
permalink:  restful_architecture
---

Upon our journey in becoming developers we are introduced to REST.  REST or REpresentational State Transfer is a software structural style that defines constraints used by web services.  Roy Fielding initially proposed this idea in his dissertation titled: Architectural Styles and the Design of Network-based Software Architectures; the paper defined a set of standards and design constraints that focuses on improvement of performance, scalablity, reliability, and ultimately to simplify web applications.  The constraints that are defined by Roy Fielding are:
<ul>

<li><strong>Client-server model </strong>
<br>
This principle addresses a seperation of concerns on in the client/server relationship which seperates user interface with data storage that improves portability by not relying on the server to store client sessions and vice versa.  This also enables systems to be developed independently, and allows for things like layering.

<li><strong>Uniform interface</strong>
<br>
Arguably the most important constraint, uniform interface aims to un-link the architecture between client - server models that will allow for interchangability and scalability while maintaining near universal communication across multple platforms or large networks.  This allows for abstraction of communications between models, and allows for independent modification/updates without disrupting the way information is relayed.  This constraint defines resource requests (like a URI) as seperate from server responses (like HTTP Codes).  Additionally, in a RESTful application, each message is "self-described" providing all information necessary to process a request, and information on cacheablility that helps achieve a stateless relationship.   

<li><strong>Statelessness</strong>
<br>
Relating to the client-server model, this constraint means that client sessions are not stored on the server between each request.  When you are loading up a webpage your client requests all the information necessary to render the page, but the state of each session is held by the client.  This session stored by the client can provide the context necessary to the server which provides a response based on the application state of individual clients without maintaining a large database of each client.  

<li><strong>Cacheability</strong>
<br>
Responses in the client - server model are requred to be defined as cacheable or noncacheable which enables the stateless constraint and helps to avoid situations where stale or old data can be transferred.  This constraint can eliminate or combine client-server requests (such as session data) which also improves performance and scalability.

<li><strong>Layered system</strong>
<br>
This constraint allows for intermediary systems like proxy servers, firewalls, or other servers to be connected along the way to a client from a server.  This factor allows for concepts like load balancing to efficently manage traffic, add seperation of logic (like a security layer) while not affecting communications between client to server.  In addition, servers can generate responses from shared resources to server to clients which greatly improves performance and scalibility.  

<li><strong>Code on demand</strong>
<br>
This constraint allows servers to extend logic or functionality to the client side and works with client side scripts (like Javascript) that can interpret client responses like mouse clicks to send back appropriate requests based on client state, this allows for dynamic webpages that have logic that does not explicitly exist in your browser, but can be distributed from the server.
</ul>

From what I have seen and read on REST and RESTful architecture, this format has been widely adopted as it makes clearer the relationships between what information is stored at each state of transfer, and how networks can be scaled by adding layers that do not impact overall communications.  In a complex system like the Web it is important that independent developers and systems not only have the ability to be flexible in terms of modification, but also that modification does not adversely impact the performance of the overall system.  To me, these constraints define an architecture that seem to address issues of individualization on a micro level with clients, and connect them through standards to a macro level of networking.  


