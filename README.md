# BDS Hub Detect Instructions

## Table of Contents

1. [What is Hub Detect?](#whatisdetect)
1. [Running your first scan](#firstscan)
	* [Windows](#windows)
	* [Linux/Unix](#linux)
1. [Common Properties](#common)
1. [Package Manager Examples](#examples)


## What is Hub Detect? 
<a name="whatisdetect"></a>

Hub Detect is a utility (developed by Black Duck Software) to identify all open source contained within an application. It utilizes two primary methods of detection:

* **Package Manager Identification**

	If the project Hub Detect is inspecting uses a package manager, it will be invoked to reconcile all the parent and transitive dependences. 

* **Signature Scanning**

	Open source dependencies that are not declared in the build script, but present in the file system, will be identified by our signature scanning mechanism.

_Please note, if the project being evaluated does not utilize a package manager, only the signature scanning process will be executed._


## Running your first scan
<a name="firstscan"></a>


**Prerequisites**

* Run any commands used to gather open source dependencies prior to conducting a scan (if applicable).
* Typically, scans are run in the root directory of an application/project you would like scanned.
* Choose OS you will be running this scan on.

    * [Windows](#windows)
    * [Linux/Unix](#linux)




## Windows

<a name="windows"></a>

**Powershell**

These are meant to be run inside powershell. See the hub-detect.ps1 file for complete list of environment variables that can be utilized to modify the execution script.

Send results to the Hub:

```
[Net.ServicePointManager]::SecurityProtocol = 'tls12'; irm https://blackducksoftware.github.io/hub-detect/hub-detect.ps1?$(Get-Random) | iex; detect --blackduck.url=http://myhub.url --blackduck.username=myusername --blackduck.password=mypassword
```


Offline scan to create JSON:

```
[Net.ServicePointManager]::SecurityProtocol = 'tls12'; irm https://blackducksoftware.github.io/hub-detect/hub-detect.ps1?$(Get-Random) | iex; detect --detect.blackduck.signature.scanner.host.url=https://saleshub.blackducksoftware.com --detect.blackduck.signature.scanner.dry.run=true --blackduck.offline.mode=true
```

## Linux/Unix

<a name="linux"></a>

**Shell**

Send results to the Hub:

```
#!/bin/bash
bash <(curl -s https://blackducksoftware.github.io/hub-detect/hub-detect.sh) --blackduck.url=http://myhub.url --blackduck.username=myusername --blackduck.password=mypassword
```

Offline scan to create JSON:

```
#!/bin/bash
bash <(curl -s https://blackducksoftware.github.io/hub-detect/hub-detect.sh) --detect.blackduck.signature.scanner.host.url=https://saleshub.blackducksoftware.com --detect.signature.scanner.dry.run=true --blackduck.offline.mode=true
```

## Common Hub Detect properties

<a name="common"></a>

**Display full list of Hub Detect properties**

```
#!/bin/bash
bash <(curl -s https://blackducksoftware.github.io/hub-detect/hub-detect.sh) -hv
```

**Automatically import certificates**

You can automatically import certificates from your instance of the Hub. This is a convenience feature, and your certificates should be imported by your administrator. However, if the certificate is not imported, Hub Detect imports the certificate for you using the following property set to true.


Property: 

```
--blackduck.hub.trust.cert=true
```

Example:

```
#!/bin/bash
bash <(curl -s https://blackducksoftware.github.io/hub-detect/hub-detect.sh) --blackduck.url=http://myhub.url --blackduck.username=myusername --blackduck.trust.cert=true
```

## Hub Detect Examples

<a name="examples"></a>

**Running Hub Detect with Maven**

When running Hub Detect with Maven, make sure to run the appropriate maven command prior to executing Hub Detect command.

Maven build command:

```
mvn clean package
```

If you need to specify of the Maven executable add the following property to your Hub Detect command:

```
--detect.maven.path=/path/to/maven
``` 

Example:

```
#!/bin/bash
bash <(curl -s https://blackducksoftware.github.io/hub-detect/hub-detect.sh) --blackduck.url=http://myhub.url --blackduck.username=myusername --blackduck.trust.cert=true --detect.maven.path/path/to/maven
```


Additional documentation can be found here: https://blackducksoftware.atlassian.net/wiki/spaces/INTDOCS/pages/49131875/Hub+Detect
