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

1. The Authentication Server grants clients seeking access to the Hadoop cluster a Ticket Granting Ticket (TGT). 
2. The customer decrypts the TGT using their credentials and using the TGT gets a service ticket from the Ticket Granting Server (TGS) - The TGS grants access to the Hadoop cluster. 
3. Customers use the service tickets granted by the TGS to access the Hadoop cluster. The Kerberos protocol is implemented as a series of businesses between the clients, the Authorization Server and the Ticket Granting Server.

Authorization is complex in Hadoop. Since Hadoop stores its data on a file system like a Linux system and not in tables like a relational database, it is not possible to restrict users by granting them partial access to the data. There is no central authorization system in Hadoop to help you limit data access by granting partial access to data files, but we can use Apache Sentry or Apache Ranger.

## Kerberos Protocol

* Three main components involved: Users (who will do the authentication), service to which users want to authenticate and the server responsible for all this the KDC.

![ticket_service](https://github.com/douglasmitsue/data-lake-security/blob/master/ticket-service.png)

1. The request starts with the Kerberos KDC client.
2. O Authentication Service(AS) returns a message to the user saying he can access, go there and look for your ticket (Access Permission).
3. The user goes to TGS(Ticket Granting Service) and takes the ticket (security key).
4. Kerberos Access.
5. Kerberos Cliente with acess ao Kerberos service.
6. Data.

## KDC(Key Distribution Center)
It is a Kerberos server that contains an encrypted database that stores all entries pertaining to users, hosts, and services (also called principals), including domain information.
Contains two components: Authentication Service(AS) and Ticket Granting Server(TGS).
AS and TGS together handle all authentication and access requests made to a Kerberos-secured Hadoop cluster.

## Keytab File
It is a secure file that contains the passwords for all service principals in a domain.
Each Hadoop service requires a keytab file on all its hosts.
When Kerberos renews the TGT for a service, it looks for the keytab file.

Process summary:

![summary](https://github.com/douglasmitsue/data-lake-security/blob/master/summary-process.png)

## Realm, Principals e Tickets
Realm(Domain: areas with the same configuration or share common characteristics).
Realm it is the basic administrative domain for authenticating users and serves to establish an administrative server's boundaries for users, hosts, and services.

## Principal
A principal is a user, host, or service that is part of a given domain.
Refers to users with **user principals**, and principals relating to services such as **service principals**.
#item - UPNs - User Principal Names - representam usuários comuns.
#item - SPNs - Service Principal Names - logins required to run Hadoop services or background processes(HDFS and YARN).

## Ticket
When a user wants to authenticate against a Kerberos supported cluster, the administration server generates a ticket. This ticket contains information such as the user's name, the primary service, the customer's IP address, and a record timestamp.


## Configuring Kerberos

![ticket_service](https://github.com/douglasmitsue/data-lake-security/blob/master/ticket-service.png)

Example:
- **Realm:** DM.COM
- **User principal:** student@DM.com
- **Service principal:** testservice/hadoop01.dm.com@DM.COM
- **KDC:** kdc.dm.com

1. When a user logs in to a cluster, he contacts the AS at kdc.dm.com using his UPN, for example student@DM.com
2. The AS grants the user a TGT, which is a token with which the user can authenticate. The TGT is encrypted with a key that is the same as the **student** user's password. For **service principals**, credentials are passed to the authentication server from the **Keytab** file stored on the host.
3. The user decrypts the TGT using the principal's password **student@DM.com**. The student user presents the TGT to the TGS at **kdc.dm.com** to request a service called **testservice/hadoop01.dm.com@DM.COM** . This service ticket allows the customer to access the cluster. The client continues to use the same TGT for multiple requests to the TGS until the TGT expires.
4. When the TGS validates the TGT presented by the **student** user, it provides a service ticket encrypted with the SPN key called **testservice/hadoop01.dm.com@DM.COM** .
5. The **student** user authenticates to a specific service on the secure cluster with the service ticket received in step 4. Once the student user presents the service ticket to the service named testservice, the service decrypts it with the SPN key **testservice/hadoop01.dm.com@DM.COM** to validate the service ticket.
6. Now that the student user has successfully authenticated, the service called testservice will allow the student user to use the service.

## Adding Kerberos Authorization to the Hadoop Cluster

Website with documentation and download [kerberos Pages](https://web.mit.edu/kerberos/).
It does not have step by step details for configuration in Hadoop.

### Installing, configuring and compiling Kerberos. Step by step
* [Configuration File Kerberos](https://github.com/douglasmitsue/data-lake-security/blob/main/07-InstalandoKerberos.txt)
* [Configuration File kdc](https://github.com/douglasmitsue/data-lake-security/blob/main/kdc.conf)
* [Configuration File krb5](https://github.com/douglasmitsue/data-lake-security/blob/main/krb5.conf)

### Hadoop
* [Environment Variables](https://github.com/douglasmitsue/data-lake-security/blob/main/01-Variaveis.txt)

