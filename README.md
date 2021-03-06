# What is CA Mobile App Services?
This repository contains the Docker Compose files to start CA Mobile App Services (this includes CA API Management OAuth Toolkit and CA Mobile API Gateway). By default it brings up the following containers:
  * CA Mobile App Services ([caapimcollab/mobile-app-services](https://hub.docker.com/r/caapimcollab/mobile-app-services/))
  * CA Mobile App Services Dev Console ([caapimcollab/mobile-app-services-dev-console](https://hub.docker.com/r/caapimcollab/mobile-app-services-dev-console/))
  * MySQL ([mysql:5.5](https://hub.docker.com/_/mysql/))
  * Moquitto MQTT Broker ([caapimcollab/mosquitto](https://hub.docker.com/r/caapimcollab/mosquitto-broker/))

> These docker compose files provide a reference implementation. If you require different configurations of the images it is recommended that you create your own implementation.

## About CA Mobile App Services and CA Mobile API Gateway
CA Mobile App Services (MAS) is a set of tightly integrated mobile services for enterprise developers. The services include: Messaging, User and Group Management, and Storage. As an infrastructure for mobile and IoT apps, MAS hides the difficult security programming from developers so they can focus on app design.

For more information, see [http://mas.ca.com/][mas.ca.com]

CA Mobile API Gateway (MAG) provides enterprises with a secure mobile backend that integrates systems and allows control over which users, devices and applications can access your APIs. The gateway enables developers to leverage standards for API security such as (OAuth2, OpenID Connect, PKI) through client SDKs for Android, iOS and Apache Cordova.

For more information, see [http://docops.ca.com/mag](https://docops.ca.com/mag)


# Getting Started Quick Start Guide - MAC and Linux

Prerequisites:
 * Install Docker: [https://www.docker.com](https://www.docker.com/)

1) Run `./MAS-start`
   * This starts CA Mobile App Services, MySQL, and Mosquitto Broker.

2) Update your hosts file and add `127.0.0.1 mas` 
   * On a MAC or Linux machine the hosts file is /etc/hosts
   * Note if you are using Windows 7 or non-beta version of docker in MAC you will need to run `docker-machine ip` to get the ip address of the docker machine. See [Networking](#networking) for more info.

3) Start creating. You can login using the *'admin'* user with password *'password'*
   * https://mas:8443/oauth/manager
   * https://mas:8443/mag/manager
   * https://mas (Developer Console)
   
# Getting Started Quick Start Guide - Windows

Prerequisites:
  * Install Docker: [https://www.docker.com](https://www.docker.com/)
  * If you are using version below Windows 10, install Cygwin: (https://www.cygwin.com/)

1) Run `attrib +R .\sql\config\my.cnf`
   * This sets my.cnf to read-only. MySql will ignore this file if it is World-Wide Writable.
    
2) Make sure your Docker directory is either inside a virtual machine share folder or marked as one. 

3) Run `bash .\MAS-start`
   * This will start CA Mobile App Services, MySQL, and Mosquitto Broker.

4) Update your hosts file and add `127.0.0.1 mas` 
   * On Windows the hosts file is located at c:\windows\System32\drivers\etc\hosts
   * Note if you are using Windows 7 or non-beta version of docker in MAC, you will need to run `docker-machine ip` to get the ip address of the docker machine. See [Networking](#networking) for more info.

5) Start creating. You can login using the *'admin'* user with password *'password'*
   * https://mas:8443/oauth/manager
   * https://mas:8443/mag/manager
   * https://mas (Developer Console)

# Working with Volumes
By default, MySQL uses a named volume `mysql-data`. This volume is used to persist data between start and stops of the docker images. To start with a clean instance, remove the volume. To remove the database volume run `docker volume rm dockermobile_mysql-data`

# Networking
To access the CA Mobile App Services you must map the hostname to the IP of your running docker instance.

## Getting the tenant's hostname
The hostname by default is "mas". However, it can be overridden using the following environment variable `MAS_HOSTNAME`

## Getting the IP of docker instance
On Linux, Mac and Windows 10 using a beta version this is `127.0.0.1`

On Windows 7 or non-beta of docker in MAC you can find this IP by using the docker-machine command.  To do this use the following command:
```
docker-machine ip
```
If your docker machine is not called 'default',  specify the custom name at the end of the command. You can now add the host entry pointing to that address.

The location of the hosts files depends on your OS. On a Windows machine with UAC enabled you can only edit this file as an administrator.

|OS|Location|
|---|---|
|Windows|c:\windows\System32\drivers\etc\hosts|
|Mac|/etc/hosts|
|Linux|/etc/hosts|

Now that both the hostname and the IP of the running docker instance are obtained, add this entry to the host file. For example: `127.0.0.1 mas`

Once the hosts file has been edited, open your browser and access the Developer Console.  The username and password to log in are 'admin/password'.

# Configuration
Customize your MAS installation by exporting one or more environment variables before running *MAS-start*. For example, to start MAS with a custom host name "my-mas.example.com":
```
export MAS_HOSTNAME="my-mas.example.com"
./MAS-start
```

## Environment Variables

|Environment Variable|Default Value|Description|
|---|---|---|
|MAS_HOSTNAME|mas|The hostname for MAS.|
|ADD_TEST_CLIENTS|false|Setting this to **true**, test apps are added to the database.|
|ADD_TEST_USERS_GROUPS|true|Setting this to **true**, users and groups are added to the database.|
|IMAGE_TAG|latest|The image tag used.|

# Test Data
You can install test data by setting one of the ADD_TEST environment variables to true. Test apps can be installed by setting **ADD_TEST_CLIENTS** to **true**. Test users and groups can be installed by setting **ADD_TEST_USERS_GROUPS** to **true** (this is the default). 

## Test Users and Groups
The default user that is always available is the **admin** user with the password **password**. Setting **ADD_TEST_USERS_GROUPS** to **true** will add the following users:

|User Name|Login|Password|
|---|---|---|
|Aaron Rose|arose|StRonG5^)|
|Ana Steeves|asteeves|StRonG5^)|
|Andy Diaz|adiaz|StRonG5^)|
|Brayden White|bwhite|StRonG5^)|
|Christina Griffin|cgriffin|StRonG5^)|
|Gilbert Rice|grice|StRonG5^)|
|Hazel Jones|hjones|StRonG5^)|
|Jamie Torres|jtorres|StRonG5^)|
|Tammy Richards|trichards|StRonG5^)|
|Vicki Peters|vpeters|StRonG5^)|
|Zachary Shelton|zshelton|StRonG5^)|

The following groups are created:

|Group Name|Members|
|---|---|
|Marketing|arose, hjones, zshelton|
|Engineering|adiaz, bwhite, trichards|
|Legal|cgriffin, vpeters, asteeves|
|Company X|adiaz, bwhite, cgriffin, grice, hjones, jtorres, trichards, vpeters, zshelton, arose, asteeves|

# Troubleshooting
This section contains some common issues and problems.

## I am not able to start the Android Emulator
### Issue
Once I started Docker, I am not able to start Android Emulator. This is a known issue:

> emulator: ERROR: Unfortunately, there's an incompatibility between HAXM hypervisor and VirtualBox 4.3.30+ which doesn't allow multiple hypervisors to co-exist.  It is being actively worked on; you can find out more about the issue at [http://b.android.com/197915](http://b.android.com/197915) (Android) and [https://www.virtualbox.org/ticket/14294](https://www.virtualbox.org/ticket/14294) (VirtualBox)
> 
> Failed to sync vcpu reg
> 
> Internal error: initial hax sync failed

### Solution
There are two possible solutions:

- Run another image of the emulator which is built with arm instead of x86.
    > However, running arm has major performance degrade, it takes a while to deploy the App.
- Use a real device for development
    > However, rooting the device to modify the hosts file is required.


# License

The docker Mobile App Services image contains a license that will expire on January 7th, 2017. After this date this image will no longer be usable.

See the [LICENSE][license-link] file for details.

 [mag]: https://docops.ca.com/mag
 [mas.ca.com]: http://mas.ca.com/
 [docs]: http://mas.ca.com/docs/
 [blog]: http://mas.ca.com/blog/

 [license-link]: /LICENSE
