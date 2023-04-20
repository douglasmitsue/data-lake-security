![data_lake_security]()

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

## Configuring Kerberos



