# OpenSAF Notification Service (NTFSv)

**Release:** Release 5.26.02 Programmer's Reference  
**Date:** February 2026  
**Copyright:** © OpenSAF, 2008  

> [!NOTE]
> For OpenSAF architecture overview, SAF AIS compliance, and document conventions, see the [OpenSAF Overview](OpenSAF_Overview_PR.md) and centralized [GLOSSARY.md](GLOSSARY.md).

---

# Overview

## Notification Service Overview

The Notification Service (NTFSv) is the OpenSAF implementation of the
SAF Notification service version A.01.01.

The purpose of this document is to describe how to use the Notification
Service (NTFSv) and it focuses on what is supported from the
recommendations of SAF described in the NTF Specification.

The Notification Service Specification contains all the information
about how to use the Notification Service API and also it is a
prerequisite that the reader is familiar with that document.

## Models and Concepts

NTFSv comprises two subparts distributed among many processes across the
cluster:

  - Notification Server (NTFS)
  - Notification Agent (NTFA)

Figure 1 Notification Service -Subparts

### Notification Server

The Notification service is implemented as a server process executing on
the controllers using a 2N redundancy model. Message based check
pointing is used to synchronize the two server instances. The NTF Server
holds a database of the subscribers in the cluster and send incoming
notifications to subscribers. The NTF server also holds a cache of the
latest sent alarm and security alarm notifications default the cache
size is 10000 notifications. The cached notifications can be read by
using the reader API.

### NTF Agent

The Notification Agent (NTFA) is the library that any process must link
with to access NTFSv functionality. It supports a subset of the API
defined by SAF’s NTF Service specification. Supported API is described
in SAI-AIS-NTF-A.01.01. Notifications sent by invoking the API in the
Notification Service are forwarded to the NTFS by NTFA.

### Compliance Report

The NTFSv conforms to the Notification Service specifications mentioned
in the SAF document SAI-AIS-NTF-A.01.01 with the following exceptions:

  - Does not implement internationalization (I18N) feature
  - Reader API reads only from a cache with a configurable size the
    latest alarm and security alarm notifications.
  - Notification service includes generic saNtf.h for versions up to
    A.03.01 but current implementation supports A.01.02.
  - There is global return type SA\_AIS\_ERR\_NOT\_SUPPORTED for any
    functionality/cross-validation which are totally not supported.
  - Only stateId is used for filtering the struct SaNtfStateChangeT
    according to the correction 1.3.1, p12 in SAI-AIS-NTF-A.02.01.

# OpenSAF Notification Service API

## Application Interface Specification

The document SAI-AIS-NTF-A.01.01, available at the following link, is a
SAF-standard document. It provides the service definition for the
Notification Service:

[https://opensaf.sourceforge.io/documentation.html](http://www.saforum.org/)

The later specification SAI-AIS-NTF-A.04.01 released under the Artistic
License contains clarifications that can be useful for a better
understanding of the NTF spec. Especially the clarifications for filter
and search criteria part for the Reader API is needed to get a correct
understanding of what to expect when using the implemented Reader API.

The following information can be found in the referenced document:

  - Service concept definitions and descriptions
  - Functional behaviors and relationships
  - The complete set of service data types exposed to the service user
  - The set of service APIs available to the service user

## Service Extensions

OpenSAF does not have any service extensions other than the
standards-based service definition explained in the document
SAI-AIS-NTF-A.01.01.

Three non SAF-standard command line programs *ntfsend*, *ntfsubscribe*
and *ntfread* is provided. The *ntfsend* program can be used in a script
or on the command line to send notifications. *ntfsubscribe* can be used
to receive notifications. *ntfread* can be used for reading old alarm
and security alarm notifications .

### ntfsend

*ntfsend* is a SAF NTF client used to send notifications. The *ntfsend*
program can be used in script or on the command line to send
notifications.

*ntfsend -h* or *ntfsend –-help *will print the available options.

SYNOPSIS

ntfsend \[OPTIONS\]

EXAMLE

\# ntfsend -T 0x5000 -s 4 --probableCause 74 -a “additional information”

sends one security alarm notification with perceived severity =
SA\_NTF\_SEVERITY\_MAJOR, problableCause = SA\_NTF\_UNSPECIFIED\_REASON
and additional text “additional information”. The other parameters are
set to default values.

\# ntfsend

generates a default alarm notification.

### ntfsubscribe

*ntfsubscribe* is a SAF NTF client used to receive notifications. The
received notification is printed to stdout. *ntfsubscribe -h *or*
ntfsubscripe –-help *will print the available options. It is possible to
filter for a specific notification type. Default is to subscribe to all
notification types.

SYNOPSIS

*ntfsubscribe \[OPTIONS\]*

EXAMPLE

\# ntfsubscribe -y

\=== Oct 6 11:42:42 - Security Alarm ===

eventType = SA\_NTF\_INTEGRITY\_VIOLATION

notificationObject = "default notification object"

notifyingObject = "default notifying object"

notificationClassId = 193.0.0 (0x0)

additionalText = "additional information"

probableCause = SA\_NTF\_UNSPECIFIED\_REASON

severity = SA\_NTF\_SEVERITY\_MAJOR

Security Alarm Detector Type: 5

Security Alarm Detector Value: 1

Service User Type: 5

Service User Value: 3

Service Provider Type: 5

Service Provider Value: 2

Receives the notification sent in the ntfsend example above. Option “-y”
is used to subscribe for security alarms.

### ntfread

*ntfread* is a SAF NTF client used for receiving old alarm and security
alarm notifications. The received notifications is printed to stdout.
*ntfread -h *or* ntfread –-help *will print the available options.

It is possible to define filter options if no options is defined the
default values is to read both alarms and security alarms. The default
settings for the search criteria is set to SA\_NTF\_SEARCH\_ONLY\_FILTER
and search direction is set to SA\_NTF\_SEARCH\_YONGER. If ntfread is
set to receive both alarm and security alarms the filter options given
is common for both. Exception is event type where a separate
securityEventType is defined for the security alarm. The parameter
perceivedSeverity is used for both severity and perceivedSeverity.

Note that the ntfsend without any option will retrieve the whole history
of alarm and security alarms which could be up to 10000 notifications
with the default cache size set see configuration below. It is
recommended to set a filter (if possible), in order to lower the
pressure on the network and the NTF server.

SYNOPSIS

*ntfread \[OPTIONS\]*

EXAMPLE

\# ntfread -s 4 --probableCause 74

\=== Oct 6 12:20:41 - Security Alarm ===

eventType = SA\_NTF\_INTEGRITY\_VIOLATION

notificationObject = "default notification object"

notifyingObject = "default notifying object"

notificationClassId = 193.0.0 (0x0)

additionalText = "additional information"

probableCause = SA\_NTF\_UNSPECIFIED\_REASON

severity = SA\_NTF\_SEVERITY\_MAJOR

Security Alarm Detector Type: 5

Security Alarm Detector Value: 1

Service User Type: 5

Service User Value: 3

Service Provider Type: 5

Service Provider Value: 2

Receives the notification sent in the ntfsend example above. Option “*-s
4*” is used to define the severity numerical which corresponds to
*SA\_NTF\_SEVERITY\_MAJOR *and the option ”*--probableCause 74*”
corresponds to* SA\_NTF\_UNSPECIFIED\_REASON*.

### Support for long DNs

This enhancement will allow any SaNameT elements in a notification
longer than 255 bytes, which is currently applicable for
notificationObject(s), notifyingObject(s) and any SaNtfValue elements as
SA\_NTF\_VALUE\_LDAP\_NAME.

In order to utilize the long DNs support in NTF, the NTF client
(producer, consumer) must also use the SaNameT tunneling primitives
(saAisNameLend, saAisNameBorrow) to manipulate SaNameT elements, and the
environment variable SA\_ENABLE\_EXTENDED\_NAMES is set to "1" in the
NTF client process. Otherwise, the producer will fail to send long DNs
notification with returned code SA\_AIS\_ERR\_INVALID\_PARAM, and any
long DNs notification will be invisible to the consumer.

The support long DNs in IMM must also be enabled so that the NTF IMCN
can send notification when long DNs objects are created, deleted, or
their attributes are changed.

Using SA\_NTF\_VALUE\_LDAP\_NAME as long DNs, please see "Implementation
Notes".

### Support absence of system controllers (SC absence)

The absence of both system controller(SC) nodes means that the NTF
service has lost its server (director).

After SC absence period, all information that existed in the server on
the SC-nodes is lost. When the SC node(s) are started again, the NTF
agent will send information about existing clients to the server so they
can be restored.

Recovery procedure will include the following client's information:

  - ntfHandle
  - Notification subscriptions including filters
  - Reader handle

However, all logged notifications will be lost. This means that even if
reader handle is restored, reader will not be able to read notifications
before SC absence period.

#### Behavior during absence period of SC

The following APIs will return SA\_AIS\_ERR\_TRY\_AGAIN until NTF server
is available again : saNtfInitialize, saNtfNotificationSend,
saNtfNotificationSubscribe, saNtfNotificationReadInitialize, and
saNtfNotificationReadNext.

All remaining APIs (including SaNtfFinalize, saNtfNotificationUnsubcribe
and saNtfNotificationReadFinalize) can be used during SC absence period.

#### Recovery handling after SC absence period

After absence period of SC, client recovery procedure is only started
when client needs to reintroduce itself to NTF server, which could be
executed as instance of consumer (subscriber, reader) or producer
(sender)

  - Subscriber: A dummy callback is sent to the client triggering a call
    to saNtfDispatch that will restore the client subscription. A call
    to saNtfNotificationSubscribe or saNtfNotificationUnsubscribe will
    also restore the client if not already restored via saNtfDispatch.
  - Reader: Recovery is started if client calls
    saNtfNotificationReadInitialize, saNtfNotificationReadNext, or
    saNtfNotificationReadFinalize.
  - Sender: Recovery is started if client calls saNtfNotificationSend .

Once recovery succeeds, client can continue using existing handles to
read or send notifications as well as receive subscribed notifications.
In other words, client does not have to do anything for recovery, NTF
Agent will handle recovery.

If recovery fails, the corresponding handle will be invalidated. This
mean that client will get SA\_AIS\_ERR\_BAD\_HANDLE. If this happen
client must initialize new handle.

### Intergration of NTFSv with CLMSv for return code SA\_AIS\_ERR\_UNAVAILABLE

From OpenSAF 5.0 release, NTFSv gets integrated with CLMSv service.
NTFSv, currently, conforms to A.01.01 SAF spec version. Integration of
NTFSv service with CLMSv is not mentioned in A.01.01. In the later spec
version viz A.02.01, section "3.13 **Unavailability of the Notification
Service API on a Non-Member Node**" page no. 42 talks about integration
of NTFSv with CLMSv. OpenSAF release 5.0, now supports this
functionality for A.01.01 APIs via enhancement ticket

https://sourceforge.net/p/opensaf/tickets/1639/

This enhancement does not implement any new API of A.02.01.

Important facts for existing users and new users :

1)  Highest supported version for NTFSv is now A.01.02 (Minor version
    got updated).
2)  A user initializing with A.01.01 (exact match) will not get
    SA\_AIS\_ERR\_UNAVAILABLE as return code anytime for any API even on
    CLMSv Non-Member node. Such a user will get A.01.01 as returned
    version and not the highest supported version. This ensures backward
    compatibility for existing users. Also in this way, this enhancement
    introduces a minor deviation from spec by conditionally returning
    the highest supported version.
3)  An application trying to initialize other than A.01.01 will get
    returned version A.01.02 provided release Code and Major Version are
    respectively ‘A’ and ‘01’.
4)  There is no impact on NTFSv integrated OpenSAF middleware services
    (like AMF, PLMSv, CLMSv and SMFSv) as they initialize with NTFSv
    with A.01.01 version.
5)  All A.PIs except saNtfFinalize() returns SA\_AIS\_ERR\_UNAVAILABLE
    on a CLM Non-Member node. Also once a CLM node loses membership then
    all the API calls on previously obtained handles will fail with
    SA\_AIS\_ERR\_UNAVAILABLE (except saNtfFinalize()) even after Node
    becomes Member. So upon receiving SA\_AIS\_ERR\_UNAVAILABLE for an
    API call, user application is expected to call saNtfFinalize() to
    free all the library handles and all resources associated with these
    handles.

## Implementation Notes

### Handling SA\_AIS\_ERR\_TRY\_AGAIN

If the NTF Service API returns *SA\_AIS\_ERR\_TRY\_AGAIN*, application
should attempt the API call only after a couple of milliseconds. The
suggested wait time is 100 milliseconds.

### Using SA\_NTF\_VALUE\_LDAP\_NAME as long DNs in saNtfPtrValAllocate()

Before the long DNs in NTF has been supported, and the
saNtfPtrValAllocate has normally being used as the way below:

saNtfPtrValAllocate(handle, sizeof(SaNameT)+1, (void\*\*)\&destPtr,

\&value);

memcpy(destPtr, \&name, sizeof(name)); /\* which @name as SaNameT,

@destPtr as char\* \*/

Provided that producer has adapted long DNs, however the long DNs string
is not fully aligned in *@name*, so the above codes will make the string
in *@name* (as long DNs) truncated to fix the size of allocated memory.
Since OpenSAF 4.5, if a NTF producer has supported long DNs, it's
recommended to use the SaNameT tunneling API to pass SaNameT into
*@destPtr* as below:

if (saNtfPtrValAllocate(handle,

strlen(saAisNameBorrow(\&name) + 3,

(void\*\*)\&destPtr, \&value) == SA\_AIS\_OK) {

saAisNameLend(saAisNameBorrow(\&name), (SaNameT\*)destPtr);

}

This should work for both short and long DNs.

### Using SA\_NTF\_VALUE\_LDAP\_NAME as long DNs in saNtfPtrValGet()

Provided that consumer has adapted long DNs, otherwise the
saNtfPtrValGet will return error code SA\_AIS\_ERR\_NAME\_TOO\_LONG if
the input value is type of SA\_NTF\_VALUE\_LDAP\_NAME as a long DNs.
Since OpenSAF 4.5, if a NTF consumer has supported long DNs, it's
recommended to use the SaNameT tunneling API retrieve SaNameT via
*@destPtr* as below:

if (saNtfPtrValGet(handle, \&value, \&destPtr, \&size) == SA\_AIS\_OK) {

saAisNameLend((SaConstStringT)destPtr + 2, \&name); /\* @name

as SaNameT, @destPtr as char\* \*/

}

This should work for both short and long Dns.

### Clarification in saNtfNotificationReadInitialize()

Current NTF implementation which conforms to A.01.01 spec does not
consider searchCriteria as optional parameter in
saNtfNotificationReadInitialize() API. The API will return
SA\_AIS\_ERR\_INVALID\_PARAM if searchCriteria is passed incorrectly.

The reason is saNtfNotificationReadNext() API requires seachCriteria so
that notification can be read based on serach criteria and it is a
"\[in\]" parameter in saNtfNotificationReadInitialize() API.

This is valid for future implementation also.

## Service Dependencies

The NTF Service has internal interfaces to the following other services:

  - Layered Enhanced Accelerated Portability (LEAP)— NTFSv uses timers,
    encode-decode utility, and handle manager services provided by LEAP.
  - Message Distribution Service (MDS)—messaging interaction between the
    subparts of NTFSv.
  - Availability Management Framework (AMF)—for high availability NTFS
    is modelled as component employing the 2N redundancy model.
  - Message Based Checkpoint Service (MBCSv)— The active NTFS uses MBCSv
    to checkpoint the state information with a standby NTFS.
  - Log Service (LOGSv)—NTFSv uses LOGSv for logging alarm
    notifications.

## Management Interface

### Command Line Interface

The NTF Service does not provide any management CLI commands.

### Configuration

The following configuration file get installed for the NTFSv service:

/etc/opensaf/ntfd.conf

If the default values in this file does not fit your system, edit this
file and rebuild the relevant rpms in order to setup your system
properly.

The following table lists environment variables and their default values
that are required to be specified in the ntfd.conf.

<table>
<tbody>
<tr class="odd">
<td>Environment variable</td>
<td>Default value</td>
<td>Comment</td>
</tr>
<tr class="even">
<td>NTFSV_ENV_CACHE_SIZE</td>
<td>10000</td>
<td><p>The size of the notifications cache in the NTF server processes running on the Controller nodes.</p></td>
</tr>
<tr class="odd">
<td>NTFSV_LOGGER_BUFFER_CAPACITY</td>
<td>10</td>
<td>Alarm notification log buffer size. Valid values are 10 to 5000 stored notifications.<br />
All alarm notifications are logged using the OpenSAF log service. NTF has a buffer to store alarm notifications to be logged later in case the log service returns TRY AGAIN when NTF writes the log record. When the log service is available again (returns OK) all alarm notifications in the buffer will be written before the NTF service can service any new alarm notification send-requests. If the buffer is big this may take some time and may cause the requests of NTF client to time out.<br />
If the buffer is full, NTF will return TRY AGAIN when alarm notification send-request is called. The send-requests of other types of notifications are handled normally.</td>
</tr>
</tbody>
</table>

# Sample Application

## Overview

The previously described *ntfsend**, **ntfsubscribe* and *ntfread* CLI
command, is used as sample application. There is no need for special
sample application since *ntfsend**, **ntfsubscribe** and **ntfread*
command uses NTFSv API as “normal” NTFSv application, is already
available (following with distribution) so it does not need to be
specifically built for demo.

The process of subscribing for notifications consists of the following
steps:

1.  Initialize the notification service library
2.  Obtain a selection object
3.  Allocate filter
4.  Subscribe for notifications
5.  Receive notification based on subscription filter
6.  Free the notification
7.  Close the subscription
8.  Finalize the notification service library

The process of sending a notification consists of the following steps:

1.  Initialize the notification service library
2.  Allocate a notification
3.  Send the notification
4.  Receive notification ID
5.  Free the notification
6.  Finalize the notification service library

The process of read notifications consists of the following steps:

1.  Initialize the notification service library
2.  Allocate filter
3.  Initialize the reader
4.  Issue read next
5.  Receive notification based on the filter and the search criteria
6.  Free the notification
7.  Close the reader.
8.  Finalize the library

## Running the Sample Application

The *ntfsend**, **ntfsubscribe* and *ntfread* command is used as a
sample applications. The offline location of the source code (in the
OpenSAF distribution) for *ntfsend.c, ntfsubscribe.c and ntfread.c is**
*** *osaf/tools/safntf/**.***

These commands can be used together. First run *ntfsubscribe* on one
node and after that run *ntfsend* on another or the same node. The
*ntfsubscribe* will printout the sent notification. If *ntfsend* is
repeated the two alarm notifications could be read by *ntfread*.

EXAMPLE 1

*ntfsubscribe*

This command subscribes for all notifications and printout the received
notifications

EXAMPLE 2

*ntfsend*

This command sends an alarm notification.

EXAMPLE 3

*ntfread*

This command receives the alarm and security alarm history.
