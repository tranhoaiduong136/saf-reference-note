# OpenSAF Log Service (LOGSv)

**Release:** Release 5.20.01 Programmer's Reference  
**Date:** January 2020  
**Copyright:** © OpenSAF, 2008  

> [!NOTE]
> For OpenSAF architecture overview, SAF AIS compliance, and document conventions, see the [OpenSAF Overview](OpenSAF_Overview_PR.md) and centralized [GLOSSARY.md](GLOSSARY.md).

---

# Overview

## LOG Service Overview

The LOG Service (LOGSv) is the OpenSAF implementation of the SAF LOG
service version A.02.01 and some added functionality not described in
the AIS version A.02.01. The added functionality is described in this
document.

Logging information is a high-level cluster-significant information
suited primarily to help troubleshoot issues such as misconfigurations,
network disconnects and unavailable resources.

The most important conceptual objects internal to the Log Service are:

  - log stream - is a conceptual flow of log records. There are four
    distinct log stream types (alarm, notification, system, and
    application).
  - log record - is an ordered set of information logged by some
    process.

### Minor version

Handling of minor version when using the initialize API function is not
implemented exactly as described in AIS. In section “3.5.1
saLogInitialize()” the AIS says:

version - \[in/out\] As an input parameter, version is a pointer to a
structure containing  
the required Log Service version. In this case, minorVersion is ignored
and should be  
set to 0x00.  
As an output parameter, version is a pointer to a structure containing
the version  
actually supported by the Log Service. The SaVersionT type is defined in
\[1\].

Minor version is not ignored but is used as follows:

|               |                                                      |
| ------------- | ---------------------------------------------------- |
| Minor Version | Comment                                              |
| 0             | As 5.0.0 and earlier.                                |
| 1             | As 5.0.0 and earlier                                 |
| 2             | CLM support. See section [3.3.6](#anchor-11)         |
| 3             | Filter set callback. See section [3.3.7](#anchor-12) |

Note: If an invalid version including minor version is given
SA\_AIS\_ERR\_VERSION will be returned.

## Models and Concepts

LOGSv comprises two subparts distributed among many processes across the
cluster:

  - LOG Server (LGS)
  - LOG Agent (LGA)

Figure 1 LOG Service -Subparts

### Log Server

The Log service is implemented as a server process executing on the
controllers using a 2N redundancy model. Message based check pointing is
used to synchronize the two server instances. The Log Server holds a
database of the open streams in the cluster and writes incoming log
records to the file related to the corresponding stream. For each new
stream a configuration file is created. Also the log rotate
functionality is handled by the Log Server.

### Log Agent

The Log Agent (LGA) is the library that any process must link with to
access LOGSv functionality. It supports a subset of the API defined by
SAF’s LOG Service specification. Supported API is described in
SAI-AIS-LOG-A.0201. Log records sent by invoking the API in the Log
Service are forwarded to LGS by LGA.

### Compliance Report

The LOGSv conforms to the LOG Service specifications mentioned in the
SAF document SAI-AIS-LOG-A.02.1. The following table provides the
compliance report specific to this release.

The OpenSAF LOG Service follows the LOG Specification of SA Forum
described in SAI-AIS-LOG-A.02.01 with the limitations below:

  - Asynchronous calls are not supported for opening a log stream.
  - Synchronous write log calls are not supported.
  - Limit Fetch API is not supported.
  - Log file full actions: WRAP and HALT are not supported.
  - No alarms or notifications are generated.
  - Log Service Management Interface is not supported.
  - The Log file property "High Availability flag" is not supported by
    LOGSv. But to configure the service to use a replicated (shared &
    persistent) file system will give the same effect as always having
    the "High Availability flag" set to TRUE.

Note: The OpenSAF LOG service allows writing zero sized log records. A
call to saLogStreamOpen\_2() with the attribute
SaLogFileCreateAttributesT\_2.maxLogRecordSize set to 0 will succeed.

Table 1 Compliance Table – Life Cycle functions

|                           |                                   |                                |
| ------------------------- | --------------------------------- | ------------------------------ |
| Life Cycle function name  | **Statement of** **Compliance  ** | Chapter in SAI-AIS-LOG-A.02.01 |
| saLogInitialize()         | Yes                               | Section 3.5.1                  |
| saLogSelectionObjectGet() | Yes                               | Section 3.5.2                  |
| saLogDispatch()           | Yes                               | Section 3.5.3                  |
| saLogFinalize()           | Yes                               | Section 3.5.4                  |

Table 2 Compliance Table – Log Service Operations

|                                     |                                   |                                |
| ----------------------------------- | --------------------------------- | ------------------------------ |
| Log Service Operation function name | **Statement of** **Compliance  ** | Chapter in SAI-AIS-LOG-A.02.01 |
| saLogStreamOpen\_2()                | Yes                               | Section 3.6.1                  |
| saLogStreamOpenAsync\_2()           | No                                | Section 3.6.1                  |
| SaLogStreamOpenCallbackT            | No                                | Section 3.6.2                  |
| saLogWriteLog()                     | No                                | Section 3.6.3                  |
| saLogWriteLogAsync()                | Yes                               | Section 3.6.3                  |
| SaLogWriteLogCallbackT()            | Yes                               | Section 3.6.4                  |
| SaLogFilterSetCallbackT()           | Yes                               | Section 3.6.5                  |
| saLogStreamClose()                  | Yes                               | Section 3.6.6                  |

Table Compliance Table – Limit fetch API

|                              |                                   |                                |
| ---------------------------- | --------------------------------- | ------------------------------ |
| **Limit API function name ** | **Statement of** **Compliance  ** | Chapter in SAI-AIS-LOG-A.02.01 |
| saLogLimitGet()              | No                                | Section 3.7.1                  |

# OpenSAF Log Service API

## Application Interface Specification

The document SAI-AIS-LOG-A.02.01, available at the following link, is a
SAF-standard document. It provides the service definition for the Log
Service:

[https://opensaf.sourceforge.io/documentation.html](http://www.saforum.org/)

The following information can be found in the referenced document:

  - Service concept definitions and descriptions
  - Functional behaviors and relationships
  - The complete set of service data types exposed to the service user
  - The set of service APIs available to the service user

## Service Extensions

OpenSAF does not have any service extensions other than the
standards-based service definition explained in the document
SAI-AIS-LOG-A.02.01.

A non SAF-standard command line program 'saflogger' is provided. The
'saflogger' program can be used in script or on the command line to
enter log records to any stream.

### saflogger

saflogger is a SAF LOG client used to write a log record into a
specified log stream.

saflogger \[options\] \[message ...\]

OPTIONS

\-l or --alarm                  write to alarm stream

\-n or --notification           write to notification stream

\-y or --system                 write to system stream (default)

\-a NAME or ---application=NAME write to application stream NAME

\-f FILENAME                    write log record to FILENAME

\-s SEV or --severity=SEV       use severity SEV, default INFO  
valid severity names: emerg, alert,  
crit, error, warn, notice, info

\-t second or --timeout=second  waiting time for acknowledgment

NOTES

1\) -f is only applicable for app stream.

2\) \<FILENAME\> length must not be longer than 255 characters.

EXAMPLES

saflogger --alarm --severity=crit "test critical message"

Will make alarm log messages with severity critical with logbuffer set
to “test critical message”.

saflogger -a myApp “myApp message”

Will make one application stream named “myApp” with logbuffer set to
“myApp message”.

saflogger -a myApp

Will make one application stream named “myApp” with empty message. i.e.
zero sized log record.

saflogger -a safLgStr=Test -f testLogFile "Hello world"

Will make an application stream named “safLgStr=Test” and send “Hello
world” logbuffer to the log file name “testLogFile”.

## Implementation Notes

### Handling of SA\_AIS\_ERR\_TRY\_AGAIN

If Log Service API returns SA\_AIS\_ERR\_TRY\_AGAIN, application should
attempt the API call only after couple of milliseconds. The suggested
wait time is 100 milliseconds.

### Zero Value for the maxLogFileSize

A value of zero for the maxLogFileSize (as defined in the section
3.4.6.2 of the LOG service specifications) attribute is not supported
and an error code SA\_AIS\_ERR\_INVALID\_PARAM is returned.

### Zero Value for the saLogStreamFixedLogRecordSize

A value of zero for the saLogStreamFixedLogRecordSize is allowed. This
means that the log records will be of variable length. No padding is
added as described in section 3.1.6.1 of the Log Service AIS. Truncation
is done if the log record size is larger than logMaxLogrecsize (see
section [3.5.2](#anchor-25))

### Handling Loss of Both System Controller Nodes

Loss of both system controller(SC) nodes means that the log service has
lost its server (director). This situation will be named as “headless”
in the rest of this document.

After headless state, all information that existed in the server on
SC-nodes are lost. When the SC node(s) are started again after being in
the headless state, the LOG server attempts to recover information about
the LOG streams and LOG clients. This recovery is performed by using
information left in IMM stream objects together with information in log
files to recover streams, and using information obtained from agents to
recover connected clients.

The Log service reads the "scAbsenceAllowed" attribute, if the attribute
is not empty, the Log service will perform recovery when SC-nodes is up
after headless. If the attribute is empty the Log service will still be
able to restart after headless but all handles are invalidated, meaning
that all APIs except saLogInitialize() will return
SA\_AIS\_ERR\_BAD\_HANDLE.

During the headless state, the saLogStreamClose() and saLogFinalize()
API return SA\_AIS\_OK, the remaining LOG APIs return
SA\_AIS\_ERR\_TRY\_AGAIN.

All LOG APIs return SA\_AIS\_ERR\_BAD\_HANDLE if recovery fails, except
saLogInitialize() and saLogFinalize().

During the headless state, if an application
log stream is closed and there are no more users of that stream, then
the IMM objects associated with that stream are deleted only after the
SC node(s) is restarted. In addition to that, the names of the log files
and the associated .cfg files for that stream are renamed by appending
the time stamp when the stream was closed.

### Long DN Support

*SaNameT *type originally held data up to 256 bytes in length. With
*extended SaNameT*, it is now up to 2048 bytes (long DN support).

To utilize long DN support, a LOG client must do the following
things\[1\]:

  - Compile the client with the *SA\_EXTENDED\_NAME\_SOURCE*
    preprocessor macro defined.
  - Set the environment variable *SA\_ENABLE\_EXTENDED\_NAMES* to 1
    before the first call to any SAF API.
  - Use the *SaNameT t*unneling primitives *saAisNameLend(),*
    *saAisNameBorrow()* to manipulate *SaNameT* elements.

Besides, if LOG client wants to create a long application stream name,
make sure IMM long DN is enabled, otherwise *saLogStreamOpen\_2()*
returns *SA\_AIS\_ERR\_INVALID\_PARAM*.

During upgrade from *OpenSAF 5.0* to *OpenSAF 5.1*, the standby node
will be rebooted if IMM long DN is enabled and a LOG client opens an
application stream with a long name or writes a notification with a long
DN in *notificationObject or* *notifyingObject*.

### Cluster Membership (CLM) Support

  - CLM integration is supported from Log Service A.02.02.

  - At-least a A.02.02 LGA client will check CLM membership status of
    client's  
    node, old LGA clients A.02.01 are always CLM members.

  - Log Service API on a Non-Member Node will fail with
    SA\_AIS\_ERR\_UNAVAILABLE on cluster nodes that are not in the
    cluster membership.

    Invocation of APIs that operate on handles acquired by any process
    before the node left the membership will continue to fail with
    SA\_AIS\_ERR\_UNAVAILABLE exception of saLogFinalize(). If the node
    rejoins the cluster membership, processes executing on the node will
    be able to reinitialize new library handles and use the entire set
    of Log Service APIs that operate on these new handles.

### Filter Change Callback

This is implemented according to and is supported from Log Service
A.02.03.  
Note: When initializing with this version also CLM support is included.

### Administrative Operation To Rotate Or Remove Log Files

Administrative Operation Id = 2

The admin operation has two options:

1.  Force a log file rotation.  
    Admin operation with no parameter.  
    The current log file is closed even if it has not reached the
    maximum log file size. A new current log file is created. The oldest
    log file is removed if the maximum number of log files to rotate is
    reached. For more information about log file rotation, see section
    ‘3.1.6 Log File Properties’ in ‘SAI-AIS-LOG-A.02.01’.

    Example: Force a log file rotation of system stream:  
    immadm -o 2 safLgStrCfg=saLogSystem,safApp=safLogService

2.  Remove requested number of log files  
    Admin operation with parameter ‘*numberOfFilesToRemove*’ set to the
    number of files to remove. No log file rotation is done. The
    requested numbers of files will be removed starting with the oldest.
    Current log file is not closed.

    The '*numberOfFilesToRemove' *parameter* *must be given a value
    between* *1 and the value of IMM attribute
    '*saLogStreamMaxFilesRotated' - 1.*
    SA\_AIS\_ERROR\_INVALID\_PARAMETER is returned in case of an invalid
    value.  
    If the value is bigger than or equal to the number of log files, no
    log file is removed and the operation returns SA\_AIS\_OK.

    Example: Delete the three oldest log files of system stream:  
    immadm -o 2 -p numberOfFilesToRemove:SA\_UINT32\_T:3
    safLgStrCfg=saLogSystem,safApp=safLogService

    Example: Remove all log files including the current of system
    stream. Assume there is a current and three closed log files. Use
    two steps:

## Service Dependencies

The Log Service has internal interfaces to the following other services:

  - Layered Enhanced Accelerated Portability (LEAP)— LOGSv uses LEAP for
    portability. It uses timers, encode-decode utility, and handle
    manager services provided by LEAP.
  - Message Distribution Service (MDS)—for messaging interaction between
    the subparts of LOGSv takes place using MDS messaging.
  - Availability Management Framework (AMF)—for high availability LGS is
    modelled as an AMF component employing the 2N redundancy model.
  - Message Based Checkpoint Service (MBCSv)— The active LGS uses MBCSv
    to checkpoint the state information with a standby LGS.

## Management Interface

This section describes how the Log Service can be accessed for
management purposes.

### Command Line Interface

The Log Service does not provide any management CLI commands.

### Configuration of Log Service

There are OpenSAF specific configuration attributes extensions
applicable to the LOG service. These extended configurations can be
specified through an object of the *OpenSafLogConfig* class. The object
name is *logConfig=1,safApp=safLogService*

There are three sources where the log server may get configuration data;
the configuration object, environment variables or hard coded default
values. The main source for configuration data is the configuration
object but if this object does not exist attributes are empty or
contains incorrect values data will be fetched from environment
variables if exist. If no valid configuration data is available a hard
coded default value will be used. The reason for not only using a
configuration object is that the log service must always be able to
start. Environment variables are supported in order to be backwards
compatible. Before a configuration object was introduced environment
variables were used.

Configuration using environment variables is deprecated and shall no
longer be used. In the future when new configuration parameters are
added no corresponding environment variables will be supported. See .

#### Configuration using a configuration object in IMM

The default imm.xml contains the object
"*logConfig=1,safApp=safLogService*" of the *OpenSafLogConfig* config
class.

The offline (in the OpenSAF distribution) location of the IMM
configuration files is: *opensaf/osaf/services/saf/logsv/config/ *and
the filenames are* logsv\_classes.xml *and* logsv\_objects.xml.* These
files are used when the imm.xml file is generated. The online (in an
executing system) location of the configuration based on these files is:
*/etc/opensaf/imm.xml*

**

|                                                            |                                                                                                                                                                                                                                                                                                          |
| ---------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **NOTE** | This configuration object may not show the actual configuration since configuration may come from three sources; configuration object, environment variables and hard coded default values. The actual configuration can be read using a corresponding runtime object. See section [3.5.2.4](#anchor-37) |

**

Configuration attributes:

<table>
<tbody>
<tr class="odd">
<td>logRootDirectory</td>
<td>/var/log/opensaf/saflog<sup>[2]</sup></td>
<td>Yes</td>
<td>Must be an existing directory with read and write access</td>
</tr>
<tr class="even">
<td>logMaxLogrecsize</td>
<td>1024</td>
<td>Yes</td>
<td>150 - 65535 (UINT16_MAX)</td>
</tr>
<tr class="odd">
<td>logStreamFileFormat</td>
<td>&lt;empty&gt;</td>
<td>Yes</td>
<td>Tokens must be valid. A character sequence starting with @ is considered a token.<br />
Value can not be set to empty in runtime but can be empty in preconfigured configuration object.<br />
If empty value the in AIS defined format will be used.</td>
</tr>
<tr class="even">
<td>logStreamSystemHighLimit</td>
<td>0</td>
<td>Yes</td>
<td>&gt; logStreamSystemLowLimit[3]</td>
</tr>
<tr class="odd">
<td>logStreamSystemLowLimit</td>
<td>0</td>
<td>Yes</td>
<td>&lt; logStreamSystemHighLimit</td>
</tr>
<tr class="even">
<td>logStreamAppHighLimit</td>
<td>0</td>
<td>Yes</td>
<td>&gt; logStreamAppLowLimit</td>
</tr>
<tr class="odd">
<td>logStreamAppLowLimit</td>
<td>0</td>
<td>Yes</td>
<td>&lt; logStreamAppHighLimit</td>
</tr>
<tr class="even">
<td>logMaxApplicationStreams</td>
<td>64</td>
<td>No</td>
<td>&gt; 0 </td>
</tr>
<tr class="odd">
<td>logFileIoTimeout</td>
<td>500</td>
<td>Yes</td>
<td>500 - 5000</td>
</tr>
<tr class="even">
<td>logFileSysConfig</td>
<td>1</td>
<td>No</td>
<td>1 or 2</td>
</tr>
<tr class="odd">
<td>logDataGroupName</td>
<td>&lt;empty&gt;</td>
<td>Yes</td>
<td>Log server must be member of group or empty. Empty string means function not used</td>
</tr>
<tr class="even">
<td>logRecordDestinationConfiguration</td>
<td>&lt;empty&gt;</td>
<td>Yes</td>
<td>See section [4.1](#destination-configuration)</td>
</tr>
</tbody>
</table>

<div class="caption">

Table 4 Configuration attributes in configuration IMM class. See also

</div>

#### Attributes in configuration object that does not have a corresponding environment variable

<table>
<tbody>
<tr class="odd">
<td>logStreamFileFormat</td>
<td>If this attribute contains a log record format string it will be used as default format for application log streams. That is when no format string is given when the stream is created the default format is used. This applies for streams both with a runtime object and with a configuration object. If this attribute is empty a hard coded default format as specified in the log service AIS is used.</td>
</tr>
<tr class="even">
<td>logFileIoTimeout</td>
<td><p>LOG service file IO operations may “hang” under some circumstances. In order to protect the log server main thread from being blocked on file IO, all file IO operations are handled in a separate "file" thread. However this “file” thread is not running in parallel with the main thread. Instead the main thread interacts with the file thread for a file IO operation to complete. If a timeout occurs asynchronous requests (write a log record) will return SA_AIS_ERR_TRYAGAIN in the corresponding callback in case the resilience of OpenSAF Log feature is disabled. Refer to chapter 5 for more information</p>
<p>Synchronous requests may not return before timeout.</p>
<p>This attribute defines the time period for which the main thread has to wait for the file IO operation to complete. The value is in milliseconds. Default value is 500 milliseconds.</p></td>
</tr>
<tr class="odd">
<td>logFileSysConfig</td>
<td><p>This attribute defines how the LOG service creates log files accordingly when OpenSAF is deployed to use a shared filed system or when deployed to use split/independent file system on the system controllers.</p>
<p>The supported values for this attribute are 1, 2.</p>
<p>The default value is 1, i.e. As the default behavior, the LOG service shall create log files only on the ACTIVE system controller node (similar to the previous OpenSAF releases).</p>
<p>LOG service will use the default value of 1 in the following scenarios:</p>
<p>a) When one of the system controller nodes is running an OpenSAF release that is lesser than 4.4, then the LOG service will assume the default value to be 1. This is the behaviour even if the logConfig=1,safApp=safLogService object exists with the attribute logFileSysConfig attribute set to 2.</p>
<p>b) When the attribute logFileSysConfig attribute is not configured in the logConfig=1,safApp=safLogService object, then the LOG service will assume the default value to be 1.</p></td>
</tr>
<tr class="even">
<td>logDataGroupname</td>
<td><p>Can be changed to an existing group which is currently a supplementary group of the user as which LOG service is running. This new group then will be used to own all log files. Also the ownership of all current log files will be changed to this group for consistent purpose. Users who are member of this group then can read the log files.</p>
<p>After the deletion of the value of this attribute, the ownership of all current log files won't be changed and behavior will be kept as previous, i.e: new opened log file will be owned by the primary group as which LOG service is running.</p></td>
</tr>
<tr class="odd">
<td>logRecordDestinationConfiguration</td>
<td>See section [4](#alternative-log-record-destination)</td>
</tr>
<tr class="even">
<td>logMaxPendingWriteRequests</td>
<td>See <a href="#5.1.1.logMaxPendingWriteRequests attribute|outline">5.1.1.logMaxPendingWriteRequests </a>attribute</td>
</tr>
<tr class="odd">
<td>logResilienceTimeout</td>
<td>See <a href="#5.1.2.logResilienceTimeout attribute|outline">5.1.2.logResilienceTimeout attribute</a></td>
</tr>
</tbody>
</table>

<div class="caption">

Table 5 Configuration attributes in configuration IMM class. Detailed
description. See also

</div>

#### Environment variables

Environment variables for configuration is deprecated and shall no
longer be used. Instead the configuration object shall be used. See
section [3.5.2.1](#anchor-35)

|                                                            |                                                                                                                                                                                             |
| ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **NOTE** | Environment variables has no effect if the corresponding attribute in the configuration object has a valid value. The configuration object has always precedence over environment variables |

This configuration method is kept for backwards compatibility reasons
and will be used if no OpenSafLogConfig object exists in IMM when the
LOG service is started.

The offline (in the OpenSAF distribution) location of the configuration
file is: *opensaf/osaf/services/saf/logsv/config/logd.conf*. The online
(in an executing system) location of the configuration file is:
*/etc/opensaf/logd.conf*.

The following table list environment variables and their function :

|                                  |                                                                                                                                                                                                                                                                                                                           |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| LOGSV\_ROOT\_DIRECTORY           | This variable is used to configure the "implementation defined root directory" as mentioned in the spec. All log files will be stored directly in this directory or in a subdirectory. If HA characteristics are needed for LOG streams, configure this directory to point to replicated (shared & persistent) partition. |
| LOGSV\_MAX\_LOGRECSIZE           | The maximum size of a log record. The size must be at least 150 byte.                                                                                                                                                                                                                                                     |
| LOG\_STREAM\_SYSTEM\_HIGH\_LIMIT | The high limit for the system/notification streams. Default unlimited. A reasonable value would be 300 (messages).                                                                                                                                                                                                        |
| LOG\_STREAM\_SYSTEM\_LOW\_LIMIT  | The low limit for the system/notification streams. Only valid and used if a high limit has been defined. Defaults to some percent of the high limit (if not specified).                                                                                                                                                   |
| LOG\_STREAM\_APP\_HIGH\_LIMIT    | The high limit for all application streams. Default unlimited. A reasonable value would be 300 (messages).                                                                                                                                                                                                                |
| LOG\_STREAM\_APP\_LOW\_LIMIT     | The low limit for the all application streams. Only valid and used if a high limit has been defined. Defaults to some percent of the high limit (if not specified).                                                                                                                                                       |
| LOG\_MAX\_APPLICATION\_STREAMS   | The maximum number of application log streams that can be created.                                                                                                                                                                                                                                                        |

<div class="caption">

Table 6 Environment variables that can be used for configuration. Note:
Deprecated

</div>

#### Reading log service configuration

The default imm.xml contains the *OpenSafLogCurrentConfig* runtime
class.

The log service will create a runtime IMM object named
“*logConfig=currentConfig, safApp=safLogService” *if the
*OpenSafLogCurrentConfig *class exist. This object can be used to read
the actual configuration of the log service.

Example:  
Reading all configuration attributes:  
\> immlist logConfig=currentConfig,safApp=safLogService  
NameTypeValue(s)  
\=============================================================  
logStreamSystemLowLimitSA\_UINT32\_T0 (0x0)  
logStreamSystemHighLimitSA\_UINT32\_T0 (0x0)  
logStreamFileFormatSA\_STRING\_T@Cr@Ch:@Cn:@Cs@Cm/@Cd/@CY @Sv @Sl
"@Cb"  
logStreamAppLowLimitSA\_UINT32\_T0 (0x0)  
logStreamAppHighLimitSA\_UINT32\_T0 (0x0)  
logRootDirectorySA\_STRING\_T/repl\_opensaf/saflog  
logMaxLogrecsizeSA\_UINT32\_T1024 (0x400)  
logMaxApplicationStreamsSA\_UINT32\_T64 (0x40)  
logFileSysConfigSA\_UINT32\_T1 (0x1)  
logFileIoTimeoutSA\_UINT32\_T500 (0x1f4)  
logDataGroupnameSA\_STRING\_T

logRecordDestinationConfigurationSA\_STRING\_T \<Empty\>  
logRecordDestinationStatusSA\_STRING\_T \<Empty\>

logMaxPendingWriteRequestsSA\_UINT32\_T 0 (0x0)  
logResilienceTimeoutSA\_UINT32\_T 15 (0xf)  
logConfigSA\_STRING\_TlogConfig=currentConfig  
SaImmAttrImplementerNameSA\_STRING\_TsafLogService  
SaImmAttrClassNameSA\_STRING\_TOpenSafLogCurrentConfig  
SaImmAttrAdminOwnerNameSA\_STRING\_T \<Empty\>  

### Configuration of Log streams

The LOG specification describes the configuration object class
SaLogStreamConfig which requires the three streams alarm, notification
and system to exist (in IMM). The two LOG classes and objects for the
three default streams are included in the OpenSAF IMM configuration. It
is also possible to add IMM configuration objects for application Log
streams. If such an object exists when the Log service is started an
application log stream will be created. A persistent application Log
stream can be created in runtime by using IMM interface to create an
object of SaLogStreamConfig class.

For more information about configuring persistent Log streams see the
SAI-AIS-LOG-A.02.1 Log specification.

#### Format tokens

The SAI-AIS-LOG-A.02.1 is ambiguous in its definition of the tokens @Cd
and @Nd and token @CD and @ND is missing.

|                 |                                                                                                               |                |
| --------------- | ------------------------------------------------------------------------------------------------------------- | -------------- |
| Token           | Description                                                                                                   | Example Output |
| @Cd             | The two digits of the implied day of the month from logTimeStamp.                                             | 05             |
| @CD\[4\]        | The first three letters (the first one capitalized) of the implied day of the week in English from eventTime. | Mon            |
| @Nd             | The two digits of the implied day of the month from logTimeStamp.                                             | 05             |
| @ND<sup>4</sup> | The first three letters (the first one capitalized) of the implied day of the week in English from eventTime. | Mon            |

Table 7 Date tokens.

The following tokens is an extension not specified in the AIS document:

|       |                                                                                                                                                 |                |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| Token | Description                                                                                                                                     | Example Output |
| @Cz   | Displayed as an offset from UTC. The format is a + or – sign and four digits; preceding and trailing zeroes are used; ±mmss                     | \+0200         |
| @Ck   | Displayed as milliseconds of last second. The format is three digits; preceding and trailing zeroes are used. Can be used as fraction of second | 010            |
| @Nz   | Displayed as an offset from UTC. The format is a + or – sign and four digits; preceding and trailing zeroes are used; ±mmss                     | \+0100         |
| @Nk   | Displayed as milliseconds of last second. The format is three digits; preceding and trailing zeroes are used. Can be used as fraction of second | 010            |
| @Cp   | Network name(\*) of type string                                                                                                                 | Network A      |
| @Cq   | Node name of type string. Show the node name (as configured in /etc/opensaf/node\_name) of the node where the log record is written from        | SC-1           |

<div class="caption">

Table 8 Extended token set

</div>

(\*) The Network name is read from the configuration attribute
“*opensafNetworkName*” of the OpenSAF global configuration object
“*opensafConfigId=opensafGlobalConfig,safApp=OpenSAF”*.

Examples:

1\) Using tokens to create an ISO 8601 time string for an application
log including milliseconds and timezone:

“@Y-@Cm-@CdT@Ch:@Cn:@Cs.@Ck@Cz” will create a time string like:

2015-08-27T14:06:40.010+0100

2\) Using tokens to log record origin for system log record written by
AMF running on SC-1 node in network “Network A”:

Format string:“@Cp:@Cq:@Sl”

Output:“Network A:SC-1:safApp=safAmfService”

# Alternative log record destination

The log service is writing log records to file and this has so far been
the only destination for log records. Several attributes in the
configuration object and log stream objects are dedicated for
configuration of this file handling. This is legacy.

Log records can also be sent to an alternative destination. In this
version log records will always be written to file in the legacy way
regardless whether an alternative destination is defined and used.

## Destination Configuration

A multivalue attribute, l*ogRecordDestinationConfiguration,* in the log
service configuration class, *OpenSafLogConfig* is used to configure
destinations. More than one destination can be configured. A
configuration is a string containing three fields where the first field
is a name given to the destination.

An OpenSAF Log stream with a configuration object (cfg stream) has an
attribute, saL*ogRecordDestination, *that can hold a name of a
destination. If a valid configuration with this name exists in the
l*ogRecordDestinationConfiguration *all log records written to this
stream will be sent to this destination as well as being written to
file.

A runtime attribute, *logRecordDestinationStatus*, in the
*OpenSafLogCurrentConfig* object can be read to get the current status
of all configured destinations. Note that this attribute cannot be found
in the *OpenSafLogConfig *object.

## OpenSafLogConfig class

### *logRecordDestinationConfiguration attribute*

*logRecordDestinationConfiguration*: *SA\_STRING\_T {CONFIG, WRITEABLE,
MULTI\_VALUE, NO\_DUPLICATES}* default (empty)

Value format: *“Name;Type;Setting”*

One destination per value. Each field is separated by “;” delimiter.

<table>
<tbody>
<tr class="odd">
<td>Name</td>
<td>Tag given to stream and destination handler for identification of destination. Can be any name. A name must consist of A-Z, a-z and numbers '_' and '-' are also allowed. Length of a name is limited to 255 characters</td>
</tr>
<tr class="even">
<td>Type</td>
<td><p>Type of destination. Defines the meaning of the Setting.</p>
<p>e.g. UNIX_SOCKET</p></td>
</tr>
<tr class="odd">
<td>Setting</td>
<td>Setting relevant to the given type of destination.</td>
</tr>
</tbody>
</table>

<div class="caption">

Table 9: Destination Configuration fields

</div>

This attribute is used to configure destinations. Each destination is
given a name that can be used with log streams. It can be configured
during runtime.

Example: To stream to local Unix Domain Socket, the value could be:
*logRecordDestinationConfiguration* =
“*MyName;UNIX\_SOCKET;/var/run/local.sock”*

Attribute validation:  
This attribute must contain a valid Name, a valid Type and a Setting. If
these criteria are not fulfilled the configuration change is rejected.
However it is not checked that the setting is valid. This means that the
configuration operation will succeed but if the Setting is invalid the
destination status will be FAILED.

  - Each configuration must have a unique name
  - There cannot be more than one configuration with the same setting

The following types are supported:

<table>
<tbody>
<tr class="odd">
<td>UNIX_SOCKET</td>
<td>A local Unix socket<br />
Setting must be a valid path to a socket</td>
</tr>
<tr class="even">
<td>NILDEST</td>
<td><p>A “dev null” destination. Log records are just dropped. This type can be used to “disconnect” a destination.<br />
For this Type Setting can have any value (not used) also an empty value.<br />
Example: “MyName;NILDEST;” is valid. Note that the trailing ‘;’ is needed.</p>
<p>A destination can also be “disconnected” by removing its configuration. In this case also the status value is removed. Another way can be to change the Setting part of the destination for example make it empty. This will result in a FAILED status</p></td>
</tr>
</tbody>
</table>

<div class="caption">

Table 10: Destination Types

</div>

## OpenSafLogCurrentConfig class

### logRecordDestinationStatus attribute

*logRecordDestinationStatus* : SA\_STRING\_T {RUNTIME, MULTI\_VALUE}
default none (empty)

Value format: “Name,Status”

This attribute is used to make it possible to see the current status of
a destination.

|        |                                                                                   |
| ------ | --------------------------------------------------------------------------------- |
| Name   | Destination name                                                                  |
| Status | Show the status of the destination. The exact meaning depends on destination Type |

<div class="caption">

Table 11: Destination Status fields

</div>

Example: “MyName,CONNECTED”

### Status values for UNIX\_SOCKET type

|           |                                                                                                                                                                                                                                                                                                                  |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CONNECTED | The socket could be opened using the given settings                                                                                                                                                                                                                                                              |
| FAILED    | The socket could not be opened when configuration was set or an error was detected during a write operation. If status is FAILED the destination handler will try to handle the problem e.g. re-open the socket at each write request. If the write request succeeds status will change from FAILED to CONNECTED |

<div class="caption">

Table 12: Type UNIX\_SOCKET Status

</div>

### Status values for NILDEST type

|                |                                                                                                 |
| -------------- | ----------------------------------------------------------------------------------------------- |
| NOT\_CONNECTED | This is the only status for NILDEST since this type can never fail or be connected to anything. |

<div class="caption">

Table 13: Type NILDEST Status

</div>

## SaLogStreamConfig class

Alternative destination is not supported for OpenSAF alarm stream and
notification stream which are using the *SaLogNtfLogHeaderT* see AIS
section 3.4.5.2

### saLogRecordDestination attribute

saLogRecordDestination: SA\_STRING\_T {CONFIG, WRITABLE, MULTI\_VALUE,
NO\_DUPLICATES} default none (empty)

Value format: “Name”

This attribute is used to set destinations for a log stream. It can be
configured by Object Manager during runtime.

Example: “MyName”

A valid destination is the Name of a destination configuration. It is
allowed to add a destination Name here before a corresponding
configuration exists in the logRecordDestinationConfiguration attribute,
see section [4.2.1](#anchor-48).

Multiple destinations can be used. All log records will be written to
all destinations.

## SaLogStream class

Alternative destination is not supported

## UNIX\_SOCKET destination type

This type of destination is a UNIX domain client socket. The “Setting”
part shall be a valid setting for such a socket

Example:

sa*LogRecordDestinationConfiguration* =
“*MyName;UNIX\_SOCKET;/var/run/local.sock”*

A log record sent to this destination is based on the RFC 5424 protocol
according to the following table:

<table>
<tbody>
<tr class="odd">
<td>PRI</td>
<td>Severity taken from the SaLogSeverityT field in the SAF log message header, and 16 - "local use 0"</td>
</tr>
<tr class="even">
<td>VERSION</td>
<td>1 (as it is currently specified in RFC 5424)</td>
</tr>
<tr class="odd">
<td>TIMESTAMP</td>
<td>Same timestamp as in the log record message. This time is either provided by the client or the time when the write request was received.</td>
</tr>
<tr class="even">
<td>HOSTNAME</td>
<td><p>&lt;Hostname&gt;[.&lt;Networkname&gt;]</p>
<p>Hostname of the node where the log record comes from<br />
Networkname is the value in OpensafConfig class attribute opensafNetworkName</p>
<p>If there is no value in opensafNetworkName attribute the [.&lt;Networkname&gt;] part is omitted</p>
<p>See also token @Cp and @Cq</p></td>
</tr>
<tr class="odd">
<td>APP-NAME</td>
<td>logSvcUsrName (token @Sl)</td>
</tr>
<tr class="even">
<td>PROCID</td>
<td>NILVALUE</td>
</tr>
<tr class="odd">
<td>MSGID</td>
<td><p>If the stream name consists of less than 32 characters and parent part pf the DN is “safApp=safLogService” this field will contain the stream name plus a trailing ‘C’ (for configuration stream)</p>
<p>In other cases MSGID will contain a hash calculated over the whole DN of the stream object.</p>
<p>Hash algorithm used is: SHA-512/192 </p></td>
</tr>
<tr class="even">
<td>STRUCTURED-DATA</td>
<td>Not used</td>
</tr>
<tr class="odd">
<td>MSG</td>
<td>The log record exactly as it has been written to log file.</td>
</tr>
</tbody>
</table>

<div class="caption">

Table 14: RFC 5424 protocol field usage

</div>

# Increase the resilience of OpenSAF Log service

In legacy, log client gets try-again from write async callback if
OpenSAF Log server has not finished the write request before
\`logFileIoTimeout\` value; the attribute is configurable and its valid
value is within the range \[500ms – 5000ms\].

The resilience of OpenSAF Log service has been improved since OpenSAF
5.20.01; the service is now able to cache the async write requests up to
a configurable time which is between 15-30 seconds before returning
status to the log client via the write async callback. This improvement
is disabled by default.

Three new attributes are introduced in this improvement; Two
configurable attributes belong to \`OpenSafLogConfig\` class, and the
other is a runtime attribute that belongs to \`OpenSafLogCurrentConfig\`
class.

If cluster goes to headless, log client will get informed about lost
invocations with SA\_AIS\_ERR\_TRY\_AGAIN code via async write callback.

## OpenSafLogConfig class

### logMaxPendingWriteRequests attribute

logMaxPendingWriteRequests: SA\_UINT32\_T {CONFIG, WRITEABLE,} default
(0)

This attribute represents the queue or the cache capacity. The valid
value is in the range \[current number of pending write requests –
1000\]; it has zero value by default – means this improvement is
disabled.

When the number of pending async write requests in the cache reaches the
given capacity, all coming async write requests will get acknowledgment
with SA\_AIS\_ERR\_TRY\_AGAIN code immediately via async write callback.

Once enable this improvement, you can disable it by setting zero value
to this attribute provided that no pending write request is staying in
the queue. To find the current number of pending write requests, fetch
the pure runtime attribute value \`*logCurrentPendingWriteRequests*\`
(refer to 5.2.1).

### logResilienceTimeout attribute

logResilienceTimeout: SA\_UINT32\_T {CONFIG, WRITEABLE,} default (15)

The attribute represents the resilience of OpenSAF Log service - how
long an async write request can stay in the queue. The valid value is in
the range \[15 seconds – 30 seconds\]. If a request has stayed over that
given time or the target stream has been closed, OpenSAF Log will drop
the request and return SA\_AIS\_ERR\_TRY\_AGAIN to the client via async
write callback.

## OpenSafLogCurrentConfig class

### logCurrentPendingWriteRequests attribute

logCurrentPendingWriteRequests: SA\_UINT32\_T {RUNTIME}

The attribute shows the number of currently pending requests in the
queue.

# Make facility id configurable

In the base functionality, streaming log records are packaged in RFC5424
format and they all carry a fixed facility ID (of numerical value 16).
The log destination is not able to differentiate regular log records to
security log records based on the fixed facility ID.

A new attribute is introduced to \`SaLogStreamConfig\` class. This new
attribute is used to configure that facility ID.

The log server is now able to differentiate log records based on
facility ID.

## SaLogStreamConfig class

### saLogStreamFacilityId attribute

*saLogStreamFacilityId* : SA\_UINT32\_T {CONFIG, WRITEABLE,
STRONG\_DEFAULT}, default(16)

This attribute is used to set facility ID for every streaming log
record. It has the numerical value of 16 (local use 0) by default and a
valid value is within the range \[0 – 23\]. The facility ID will go back
to the default value when this attribute is deleted.

# Sample Application

## Overview

The previously described *saflogger *CLI command, is used as sample
application. There is no need for special sample application since
*saflogger *command:

  - uses LOGSv API as “normal” LOGSv application,
  - is already available (following with distribution) so it does not
    need to be specifically built for demo.

The process of sending a log record consists of the following steps:

1.  Initialize the log service library
2.  Obtain a selection object
3.  Open the log stream
4.  write the log record
5.  Close the stream
6.  Finalize the log service library

## Running the Sample Application

The *saflogger* command is used as a sample application. The offline
location of the source code (in the OpenSAF distribution) is:
*opensaf/osaf/tools/saflog/saflogger/saf\_logger.c*.

EXAMPLE 1

saflogger --alarm --severity=crit "sample alarm message"

This command will make an alarm log record with severity critical and
send it to the LGS. The logbuffer message is set to “sample alarm
message”.

Check for the output in the log file located and named according to the
configuration the system as explained in previous chapter.  
(*default*:          *  
/var/log/opensaf/saflog/saLogAlarm\_\<creation\_date\>\_\<creation\_time\>.log*)

EXAMPLE 2

saflogger -a myApp “myApp message”

This command will make one application stream named “myApp” with
logbuffer set to “myApp message” and send it to the LGS. The output is
found in the following directory: *\<LOGSV\_ROOT\_DIRECTORY\>/saflogger/
(default: /var/log/opensaf/saflog/saflogger)*.

1.   Refer to chapter 4. Extended SaNameT Type in
    OpenSAF\_Extensions\_PR.odt for details.

2.  This path is not set as a default value in the logsv\_classes.xml
    file as the rest of the parameters. Instead it is  
    created from $pkglogdir/saflog when the imm.xml file is created.

3.  Both high and low for both system and app stream limit can be 0.
    This means unlimited message queue

4.  Extension. Not specified in SAI-AIS-LOG-A.02.1
