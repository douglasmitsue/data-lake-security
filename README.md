![data_lake_security]()
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

## Kerberos Protocol

## Configuring Kerberos



