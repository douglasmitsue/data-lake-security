![data_lake_security](https://github.com/douglasmitsue/data-lake-security/blob/master/protocolo-kerberos.png)

## Kerberos
In Greek mythology Kerberos (in Greek, Cerberos) is the ferocious multi-headed dog that guards the gates of Hades to ensure that no one leaves.

## Data Lake Security
Haddop is an open source project that includes several independently developed modules to add various types of functionality, does not have a coherent security model, and companies are concerned about the security of business data that is increasingly stored in Hadoop-based environments .
Unlike a traditional database, in Hadoop there is no server or central authentication mechanism. Users who can access a server running NameNode and have permissions appropriate to Hadoop binaries can read unauthorized data and even delete that data. There is no role-based access control(RBAC - Role Based Acess Control), object level or other granular authentication features in Hadoop.
How data is stored in various DataNodes, each of them is a possible point of invasion. 
To encrypt data between clients and DataNodes, we should use the Kerberos or a structure SASL(Simple Authentication and Security Layer).

## Authentication, Authorization and Auditing
We can create three layers of security in Hadoop.

**Authentication** - Best and most accepted form of authentication for Hadoop - Kerberos security.
Kerberos provides strong authentication of users and services working with a Hadoop cluster.

**Authorization** - Determining access to data in a Hadoop cluster is a top security concern. We use an access control list(ACLs) to configure the authorization. 
O Apache Sentry allows a strong and detailed authentication configuration for the data.

**Auditing** - Optional security layer that has an interesting feature. 
Authentication and Authorization are defined before the user does anything.
Auditing is a layer we put in after user action.
It is a tracking of activity within the cluster, and may include user or administrative activity.

## Defense in Depth

Data is the main focus where we want to ensure security, until we get to data security we have layers of security that must be considered.
**PHISICAL** -> **PERIMETER** -> **NETWORK** -> **HOST** -> **APP** -> **DATA**(foco).
Pay attention not only to the defense of the data, but to the layers of defense that surround the data.

## Hadoop Security Design

Kerberos is an open source network authentication protocol that allows cluster nodes to verify their identities with each other. Is a pure authentication protocol, so it does not manage file and directory permissions. When you implement, users wanting access to the cluster will first contact the central Kerberos server called the Key Distribution Center (KDC), which contains the credential database. If the credentials provided by the user are correct, the KDC grants access to the Hadoop cluster.

1.The Authentication Server grants clients seeking access to the Hadoop cluster a Ticket Granting Ticket (TGT). 
2.The customer decrypts the TGT using their credentials and using the TGT gets a service ticket from the Ticket Granting Server (TGS) - the TGS grants access to the Hadoop cluster. 
3.Customers use the service tickets granted by the TGS to access the Hadoop cluster. The Kerberos protocol is implemented as a series of businesses between the clients, the Authorization Server and the Ticket Granting Server.

Authorization is complex in Hadoop. Since Hadoop stores its data on a file system like a Linux system and not in tables like a relational database, it is not possible to restrict users by granting them partial access to the data. There is no central authorization system in Hadoop to help you limit data access by granting partial access to data files, but we can use Apache Sentry or Apache Ranger.

## Kerberos Protocol

* Three main components involved: Users (who will do the authentication), service to which users want to authenticate and the server responsible for all this the KDC.

![ticket_service](https://github.com/douglasmitsue/data-lake-security/commit/16e3f6e708e52644827d019435a9fd3200142a5d)
1 - The request starts with the Kerberos KDC client.
2 - O Authentication Service(AS) returns a message to the user saying he can access, go there and look for your ticket (Access Permission).
3 - The user goes to TGS(Ticket Granting Service) and takes the ticket (security key).
4 - Kerberos Access.
5 - Kerberos Cliente with acess ao Kerberos service.
6 - Data.

## KDC(Key Distribution Center)
It is a Kerberos server that contains an encrypted database that stores all entries pertaining to users, hosts, and services (also called principals), including domain information.
Contains two components: Authentication Service(AS) and Ticket Granting Server(TGS).
AS and TGS together handle all authentication and access requests made to a Kerberos-secured Hadoop cluster.

## Keytab File
It is a secure file that contains the passwords for all service principals in a domain.
Each Hadoop service requires a keytab file on all its hosts.
When Kerberos renews the TGT for a service, it looks for the keytab file.

Process summary:

![summary](https://github.com/douglasmitsue/data-lake-security/commit/16e3f6e708e52644827d019435a9fd3200142a5d)

## Realm, Principals e Tickets
Realm(Domain: areas with the same configuration or share common characteristics).
Realm it is the basic administrative domain for authenticating users and serves to establish an administrative server's boundaries for users, hosts, and services..

## Principal
A principal is a user, host, or service that is part of a given domain.
Refers to users with **user principals**, and principals relating to services such as **service principals**.
UPNs - User Principal Names - representam usuários comuns.
SPNs - Service Principal Names - logins required to run Hadoop services or background processes(HDFS and YARN)


## Configuring Kerberos




