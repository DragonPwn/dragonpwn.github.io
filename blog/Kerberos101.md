# Kerberos

![Windows-Service.png](Kerberos%20130467127f6d4010a55ca39900aa977c/Windows-Service.png)

First of all in this blog we will discuss an authentication protocol called Kerberos.

- what is Kerberos?
- How does it works?
- Authentication flow
- How can you see your tickets?

### What is Kerberos?

Kerberos is an network security authentication protocol  allow nodes to communicate with each other across untrusted network by using tickets, Kerberos builds on symmetric key cryptography and runs as a third party trusted server called Key Distribution Center (KDC), All environment users, machines, service that use Kerberos depend on KDC. 

### How does it works?

Client : its a user who is request the service.

Server : The service that the client want to access.

Ticket Granting Ticket (TGT) : Its a user authentication token issued by Key Distribution Center (KDC) used to request access token from (TGS).

Key Distribution Center (KDC) : The trust third party which contain database and Authentication Service (AS) and the Ticket Granting Server (TGS).

Authentication Server (AS) : Authenticated service which authenticates client and issues them tickets and (TGS) which accept authenticated clients and issues them tickets to access another recourse.

Ticket Granting Server (TGS) : Its part of (KDC) That issues a service ticket .

Note: Before issued service ticket you must have The (TGT) for AD Domain first  .

### Authentication flow

![Visio-KerberosComms.png](Kerberos%20130467127f6d4010a55ca39900aa977c/Visio-KerberosComms.png)

1- AS REQ (request TGT)

The User Asks for the (TGT) From Authentication Server (AS).

The request include the user principle Name (UPN) and timestamp , and its encrypts using the user password hash.

2- AS REP (receive TGT) 

Verify User's Creds

The KDC uses the UPN and search for the User in its database and uses the password hash to decrypt the message if is successfully decrypts the request and if the timestamp within the KDS's configured time, the authentication is successful.

The Authentication Server computes (TGS) secret key then create a session key (sk1) encrypted by User secret key, then Authentication server generate a (TGT) contain the User ID , User network address, timestamp, lifetime, and SK1 the (TGS) secret key then encrypts the ticket .

The User Decrypt the message by using its secret key and extract the (SK1) and (TGT) to create the authenticator that validates the User's (TGS) .

3- TGS REQ (Present TGT, request TGS)

The User send the present (TGT) to request (TGS) 

4- TGS REP (receive TGS)

the KDC validates the (TGT), Then Generate the (TGS), The (TGS) uses (TGS) secret key to decrypt the (TGT) that received from user and extract the (SK1) and checks the user ID , network address and timestamp to make sure the (TGT) has not expired .

if all checks are successful then the (KDC) generate a service session key (SK2) that is shared between the User and the target server . 

Then the (KDC) create service ticket that contain User ID, network address, timestamp and (SK2) , The ticket will encrypted with server secret key , then the User receive a message with the service ticket and (SK2) , all encrypted with (SK1) . 

5- AP REQ (present TGS for access)

The target serve uses the server session key to decrypt the ticket and extract the (SK2) to decrypt the authenticator then checks the User ID, network address , also the server checks if the server ticket is expired .

6- AP REP (optional, used when mutual authentication is requests)

once check is finished the User receive message from Application server for verifying that they can authenticate each other . 

### How can you see your tickets?

you can see all Kerberos ticket by using klist.exe you can view all issues tickets to you on your computer, you can use klist tickets command on cmd or power shell