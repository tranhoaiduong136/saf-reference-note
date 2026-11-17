# OpenSAF Cluster Membership Service (CLMSv)

**Release:** Release 5.17.11 Programmer's Reference  
**Date:** November 2017  
**Copyright:** © OpenSAF, 2010  

> [!NOTE]
> For OpenSAF architecture overview, SAF AIS compliance, and document conventions, see the [OpenSAF Overview](OpenSAF_Overview_PR.md) and centralized [GLOSSARY.md](GLOSSARY.md).

---

# Overview

## Cluster Membership Service Overview

The Cluster Membership Service (CLMSv) provides applications with
membership information about the nodes that have been administratively
configured in the cluster configuration, these nodes are called as
cluster nodes or configured nodes. CLMSv is core to any clustered
system. A cluster consists of the set of configured nodes, each with a
unique node name.

A member node is a configured node that the CLMSv has recognized to be
healthy and well-connected to be used for deploying High Availability
(HA) applications and services.

The implementation of CLMSv is compliant to SAF AIS SAI-AIS-CLM-B.04.01
and SA-AIS-CLM.B.01.01* *specifications.

## Models and Concepts

This section provides information about CLMSv architecture and
compliance report.

### Service Structure Overview

Cluster Membership Service is comprised of the following modules:

  - Cluster Membership Server (CLMS)
  - Cluster Membership Agent (CLMA)
  - Cluster Membership Node Agent (CLMNA)

Figure 1 Cluster Membership Service -Sub parts

#### CLM Server

The Cluster Membership Server (CLMS) maintains the database of all nodes
in cluster. CLMS maintains information of applications that have
registered for cluster tracking and the applications are notified
through callbacks about membership changes to the cluster nodes.

The CLMS process runs on a system controller node in a 2N redundancy
model. CLMS uses Message Based Checkpoint Service (MBCSv) to checkpoint
the runtime data between ACTIVE-CLMS and STANDBY-CLMS.

CLMSv integrates with the IMMSv for configuration and object management.

CLMSv uses OpenSAF notification service (NTFSv) to send notifications
compliant to the SAI-AIS-CLM-B.04.01 specification.

CLMS integrates with Platform Management Service (PLMSv) and subscribes
for the PLMSv readiness track callback to track the status of the
underlying PLM Execution Environments (EE) in the following scenarios:

  - PLMSv is enabled in the system, by default in *imm.xml* mapping of
    CLM-PLM EE is commented. To enable PLM, user needs to uncomment.
  - All CLM nodes are mapped to respective EEs in the *imm.xml, *partial
    mapping of CLM nodes is not supported*.*

The saClmNodeID is the same as the node\_id of the OpenSAF
infrastructure, i.e. the MDS node\_id.

CLMSv considers a CLM node as a qualified member of the cluster when
**all** of the following conditions are met:

  - A configuration object exists in the IMM database for a given CLM
    node.
  - The admin state of the CLM node is SA\_CLM\_ADMIN\_UNLOCKED.
  - Connectivity (MDS/TIPC) to the CLM node exists.
  - If PLM is running in the system and the readiness state of the EE
    hosting (if any) the CLM node is SA\_PLM\_READINESS\_IN\_SERVICE.

CLMSv considers a CLM node as being evicted from the cluster during
**any** of the following conditions:

  - Connectivity (MDS/TIPC) to the CLM node is lost.
  - If PLM is running in the system and the readiness state of the EE
    hosting (if any) the CLM node is
    SA\_PLM\_READINESS\_OUT\_OF\_SERVICE.
  - If the admin state of the CLM node is not SA\_CLM\_ADMIN\_UNLOCKED.
  - If the AMFND process on that CLM node exits during a scenario when
    OpenSAF does not controls the TIPC connectivity, i.e. when
    OPENSAF\_MANAGE\_TIPC=NO flag is set in the /etc/opensaf/nid.conf.

**Note**: You can choose whether CLM should use the services of PLM or
not by using the following options:

  - During compile time, by specifying *./configure --disable-ais-plm
    --disable-ais-hpi*
  - During run time we cannot change the execution environment attribute
    of the saClmNode object.

#### CLM Agent

The Cluster Membership Agent (CLMA) is a library that any process must
link with, to access CLMSv functionality. CLMA supports two sets of
API’s based on SAI-AIS-CLM-B.04.01 and SAI-AIS-CLM-B.01.01 SAF
specifications respectively.

#### CLM Node Agent

One instance of the Cluster Membership Node agent (CLMNA) runs on each
node in a NO redundancy model with recommended recovery as NodeFailFast.
The CLMNA updates the node specific attributes to the CLMS. The CLMNA
allows a node to join the cluster only if it is a configured node. For
details on how to specify the node\_name please refer Section 3.4.1.

Currently, CLMNA updates the node\_id and node\_name attributes to the
CLMS.

CLMNA also tracks the presence of ACTIVE and STANDBY system controller
nodes. Upon detecting that the ACTIVE or STANDBY node is down, CLMNA
starts an election timeout configuration attribute (see
[20](#anchor-31)[20](#anchor-31)). Upon expiry of this timer, CLMNA
starts an election to choose a new ACTIVE system controller node by
nominating the self node (spare system controller node) also in the
election.

### Compliance Report

  - Use of pointer that refers to freed space results in undefined
    behavior with  saClmClusterNotificationFree\_4().

Table 1 Compliance table

<table>
<tbody>
<tr class="odd">
<td></td>
<td>Not Supported</td>
<td>4.4</td>
</tr>
<tr class="even">
<td>Admin operation on active controller</td>
<td>Not Supported</td>
<td>5.1.2</td>
</tr>
<tr class="odd">
<td></td>
<td><p>Supported when MDS uses TCP as the mode of transport.</p>
<p>Validation of IP address and address family values configured in imm.xml and the ip address configured in /etc/opensaf/dtmd.conf is not supported. It is the responsibility of the end user to ensure that the same IP address values are configured in imm.xml and in the dtmd.conf</p>
<p>Modifying these attributes at runtime is not supported.</p>
<p>The values of saClmNodeAddressFamily and saClmNodeAddress shall be delivered via</p>
<p>SaClmNodeAddressT in the cluster track callback.</p>
<p>Note: When user chooses TIPC as mode of transport, then the address length will be</p>
<p>set to 0 and address family shall contain the default value of</p>
<p>SA_CLM_AF_INET(i.e. SAF equivalent of AF_INET).</p>
<p>This is done for backward compatibility with pre-OpenSAF 4.4 libraries</p>
<p>and also because the CLM spec does not support any value other than</p>
<p>SA_CLM_AF_INET(i.e. SAF equivalent of AF_INET) and SA_CLM_AF_INET6 (i.e. SAF equivalent of AF_INET6).</p></td>
<td>3.3.3</td>
</tr>
<tr class="even">
<td></td>
<td>A reboot is not triggered when a node leaves the cluster. Support for this attribute becomes more relevant when OpenSAF supports the scenario when a CLM cluster is a bigger set than the AMF cluster. Also, this line from the spec is questionable and requires spec update.</td>
<td>3.1.2, Line 27</td>
</tr>
</tbody>
</table>

# OpenSAF Cluster Membership API

## Application Interface Specification

The document SAI-AIS-CLM-B.04.01 is available at
[*https://opensaf.sourceforge.io/documentation.html*](http://www.saforum.org/).

## Implementation Notes

  -  If the CLMSv API returns SA\_AIS\_ERR\_TRY\_AGAIN, application
    should attempt the API call only after a couple of milliseconds. The
    recommended wait time is 100 milliseconds.
  -  It is the responsibility of the System Integrator to customize the
    opensaf\_reboot script to enable support of rebooting remote
    (non-local) nodes, when the CLM node is not mapped tor PLM EE in the
    imm.xml. By default, the opensaf\_reboot script does not supports
    remote-reboots.
  -  The CLM handle (SaClmHandleT) held by an application becomes
    invalid if both system controller nodes are rebooted. As a result of
    this the CLM application shall receive ERR\_BAD\_HANDLE for any
    invocation of the CLM API. The application is expected to obtain a
    new CLM handle by calling saClmInitialize() and to honor the
    SA\_AIS\_ERR\_TRY\_AGAIN error code during this scenario.
  -  For the Relative Distinguished Name (RDN) of a CLM node name to be
    longer than 64 characters, the 'long DN' feature must be enabled as
    described in the OpenSAF IMMSv\_PR.odt (section “Support for long
    DNs in existing IMM APIs”). However, upon enabling the 'long DN'
    feature, although the RDN of CLM node name does not have a
    limitation of 64 bytes, the CLM node name DN must still be less than
    256 characters.

## Service Dependency

The CLMSv has interfaces to the following services.

  - Layered Environment for Accelerated Portability (LEAP) – CLMSv uses
    timers, encode-decode utility and handle manager services provided
    by LEAP.
  - Message Distribution Service (MDS) - Messaging interaction between
    the sub parts of CLMSv. (CLMS  CLMA) and (CLMS  CLMNA)
  - Availability Management Framework- For high availability CLMSv is
    modeled as an AMF component employing the 2N redundancy model.
  - Message Based Checkpoint Service (MBCSv) - The ACTIVE CLMSv uses
    MBCSv to checkpoint the state information to a STANDBY CLMSv.
  - Information Model Management Service (IMMSv) for Configuration and
    Object Management.
  - Notification Service (NTFSv) for Sending SAF-AIS-CLM-B.04.01 based
    notifications.
  - Platform Management Service (PLMSv) for managing the EE that is
    mapped to the CLM node.
  - Logtrace for logging and tracing.

## Management Interface

### Configuration

  - */etc/opensaf/node\_name*

This file should contain the value of the Relative Distinguished Name
(RDN) of the CLM node name. The desired RDN value for the node name
should be specified in the nodes.cfg while generating the imm.xml. The
same value should be specified in the */etc/opensaf/node\_name*.

**Note**: The CLMNA does not allow the node to join the cluster if the
RDN value specified in the */etc/opensaf/ node\_name* and the *imm.xml
*entry do not match.

An example RDN value for the */etc/opensaf/node\_name* is given below.
Here the *imm.xml* was generated based on the RDN values for the node
names as specified in the *nodes.cfg.*

An example *nodes.cfg* entry (generated for a 2 system controller and 2
payload cluster) is as below. This can be edited to the desired values
and then run the .***/immxml-configure***:

SC SC-1 SC-1

SC SC-2 SC-2

PL PL-3 PL-3

PL PL-4 PL-4

Example:

*\<object class="SaClmNode"\>*

*                \<dn\>safNode=**PL-3**,safCluster=myClmCluster\</dn\>*

\<attr\>

\<name\>saClmNodeLockCallbackTimeout\</name\>

\<value\>50000000000\</value\>

\</attr\>

\</object\>

\<object class="SaClmNode"\>

*               \<dn\>safNode=**SC-1**,safCluster=myClmCluster\</dn\>*

\<attr\>

\<name\>saClmNodeLockCallbackTimeout\</name\>

\<value\>50000000000\</value\>

\</attr\>

\</object\>

In the above example, the */etc/opensaf/node\_name* should contain
**PL-3** on payload node and **SC-1** on controller node.

  - */etc/opensaf/clmna.conf*

This file contains configuration options for the CLM node agent. This
file has to be modified in the following cases:

CLMNA\_ELECTION\_DELAY\_TIME should be set to the time in milliseconds
without any active or standby system controller node that OpenSAF will
wait before starting an election for a new *active* system controller.

CLMNA will detect the presence of active and standby system controllers
in the cluster. If no active or standby system controller has been seen
during the configured time period, CLMNA will initiate an election with
the own node as a candidate for a new active system controller. Note
that lowering this value will increase the risk for split-brain. Values
lower than 5000 are not recommended for clusters with more than two
system controllers. For legacy installations with only two system
controller nodes, the default delay of 200 ms should be sufficient. For
systems with a very large number of configured system controller nodes
and/or with unreliable network connections, values larger than 5000 may
be needed.

### CLM Admin Operations

The \<clm node name\> is the DN of the CLM should be passed as input to
the following commands, and the DN should be the same as mentioned in
the *imm.xml*.

  - CLM Node Lock Operation:

The following IMM command is used to lock a CLM node:

***immadm -o 2 \<clm node name\>***

Example:-

immadm -o 2 safNode=PL-3,safCluster=myClmCluster

  - CLM Node Unlock Operation:

The following IMM command is used to unlock a CLM node:

immadm -o 1 \<clm node name\>

Example:-

immadm -o 1 safNode=PL-3,safCluster=myClmCluster

  - CLM Node Shutdown Operation:

The following IMM command is used to shutdown a CLM node:

***immadm -o 3 \<clm node name\>***

Example:-

immadm -o 3 safNode=PL-3,safCluster=myClmCluster

  - CLM Cluster Reboot Operation:

"Note: This admin operation on the cluster object is an OpenSAF
extension".

The following IMM command is used to order a cluster reboot:

***immadm**** -o 4 \<clm cluster name\>***

Example:-

immadm -o 4 safCluster=myClmCluster

  - CLM Node Reboot Operation:

"Note: This admin operation on the node object is an OpenSAF extension".

The following IMM command is used to order a node reboot:

***immadm**** -o 4 \<clm node name\>***

Example:-

immadm -o 4 safNode=PL-3,safCluster=myClmCluster

  - Remotely Executing CLM script:

"Note: This admin operation on the cluster object or a cluster node is
an OpenSAF extension".

The admin operation will remotely execute a script stored in
/usr/lib/opensaf/clm-scripts, and the script is prefixed with osafclm\_
word.

The admin operation is only used for the cluster management purposes,
and should not be used for remotely executing scripts by applications.

The operator must be aware that scripts inherit environment variables
from CLM, but this will be changed in the future. Applications should
not rely on environment variables provided by CLM.

The following IMM command is used to execute script on all nodes:

***immadm**** -o 5 -p saClmAction:SA\_STRING\_T:\<clm script name\>
\<clm cluster name\>***

The following IMM command is used to execute script on one node:

***immadm**** -o 5 -p saClmAction:SA\_STRING\_T:\<clm script name\>
\<clm node name\>***

Examples:-

Executing osafclm\_test script on all nodes:

immadm -o 5 -p saClmAction:SA\_STRING\_T:test \\

safCluster=myClmCluster

Executing osafclm\_test script on PL-3 node:

immadm -o 5 -p saClmAction:SA\_STRING\_T:test \\

safNode=PL-3,safCluster=myClmCluster
