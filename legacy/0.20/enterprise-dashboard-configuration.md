---
version: 0.20
category: "Enterprise Dashboard Docs"
title: "Enterprise Dashboard Reference Documentation"
next:
  url: "enterprise-dashboard-collections"
  text: "Enterprise Dashboard Collections"
---

# Overview

This reference document provides information to help you:

* Configure the Sensu Enterprise Dashboard
* Enable and configure the optional [Role-Based Access Controls
  (RBAC)](#role-based-access-controls-rbac)
  * Configure the [GitHub driver for RBAC](#github-driver-for-rbac)
  * Configure the [LDAP driver for RBAC](#ldap-driver-for-rbac)
* Configure or disable [Audit Logging](#audit-logging)

## Example configurations

### Minimal configuration

The following is the bare minimum that should be included in your Sensu
Enterprise Dashboard configuration.

~~~ json
{
  "sensu": [
    {
      "name": "sensu-server-1",
      "host": "api1.example.com",
      "port": 4567
    }
  ],
  "dashboard": {
    "host": "0.0.0.0",
    "port": 3000
  }
}
~~~

# Configuration attributes

sensu
: description
  : An array of hashes containing [Sensu API endpoint attributes](#sensu-api-endpoint-attributes).
: required
  : true
: type
  : Array
: example
  : ~~~ shell
    "sensu": [
        {
            "name": "sensu-server-1",
            "host": "127.0.0.1",
            "port": 4567
        }
    ]
    ~~~

dashboard
: description
  : A hash of [dashboard configuration attributes](#dashboard-attributes).
: required
  : true
: type
  : Hash
: example
  : ~~~ shell
    "dashboard": {
        "host": "0.0.0.0",
        "port": 3000,
        "refresh": 5
    }
    ~~~

## Sensu API endpoint attributes

name
: description
  : The name of the Sensu API (used elsewhere as the `datacenter` name)_NOTE: In High Availability (HA) configurations, multiple Sensu API endpoints can be grouped together by using the same datacenter name.__NOTE: In High Availability (HA) configurations, multiple Sensu API endpoints can be grouped together by using the same datacenter name._.
: required
  : false
: type
  : String
: default
  : randomly generated
: example
  : ~~~ shell
    "name": "us-west-1"
    ~~~

host
: description
  : The hostname or IP address of the Sensu API.
: required
  : true
: type
  : String
: example
  : ~~~ shell
    "host": "127.0.0.1"
    ~~~

port
: description
  : The port of the Sensu API.
: required
  : false
: type
  : Integer
: default
  : 4567
: example
  : ~~~ shell
    "port": 4567
    ~~~

ssl
: description
  : Determines whether or not to use the HTTPS protocol.
: required
  : false
: type
  : Boolean
: default
  : false
: example
  : ~~~ shell
    "ssl": true
    ~~~

insecure
: description
  : Determines whether or not to accept an insecure SSL certificate.
: required
  : false
: type
  : Boolean
: default
  : false
: example
  : ~~~ shell
    "insecure": true
    ~~~

path
: description
  : The path of the Sensu API. Leave empty unless your Sensu API is not mounted to `/`.
: required
  : false
: type
  : String
: example
  : ~~~ shell
    "path": "/my_api"
    ~~~

timeout
: description
  : The timeout for the Sensu API, in seconds.
: required
  : false
: type
  : Integer
: default
  : 5
: example
  : ~~~ shell
    "timeout": 15
    ~~~

user
: description
  : The username of the Sensu API. Leave empty for no authentication.
: required
  : false
: type
  : String
: example
  : ~~~ shell
    "user": "my_sensu_api_username"
    ~~~

pass
: description
  : The password of the Sensu API. Leave empty for no authentication.
: required
  : false
: type
  : String
: example
  : ~~~ shell
    "pass": "my_sensu_api_password"
    ~~~

## Dashboard attributes

host
: description
  : The hostname or IP address on which Sensu Enterprise Dashboard will listen on.
: required
  : false
: type
  : String
: default
  : "0.0.0.0"
: example
  : ~~~ shell
    "host": "1.2.3.4"
    ~~~

port
: description
  : The port on which Sensu Enterprise Dashboard will listen on.
: required
  : false
: type
  : Integer
: default
  : 3000
: example
  : ~~~ shell
    "port": 3000
    ~~~

refresh
: description
  : Determines the interval to poll the Sensu APIs, in seconds.
: required
  : false
: type
  : Integer
: default
  : 5
: example
  : ~~~ shell
    "refresh": 5
    ~~~

user
: description
  : A username to enable simple authentication and restrict access to the
    dashboard. Leave blank along with `pass` to disable simple authentication.
: required
  : false
: type
  : String
: example
  : ~~~ shell
    "user": "admin"
    ~~~

pass
: description
  : A password to enable simple authentication and restrict access to the
    dashboard. Leave blank along with `user` to disable simple authentication.
: required
  : false
: type
  : String
: example
  : ~~~ shell
    "pass": "secret"
    ~~~

github
: description
  : A hash of [GitHub authentication attributes](#github-authentication-attributes) to enable
    GitHub authentication via OAuth. Overrides simple authentication.
    _NOTE: GitHub authentication is only available in the [Sensu
    Enterprise](https://sensuapp.org/enterprise) Dashboard._
: required
  : false
: type
  : Hash
: example
  : ~~~ shell
    "github": {
      "clientId": "a8e43af034e7f2608780",
      "clientSecret": "b63968394be6ed2edb61c93847ee792f31bf6216",
      "server": "https://github.com",
      "roles": [
        {
          "name": "guests",
          "members": [
            "myorganization/devs"
          ],
          "datacenters": [
            "us-west-1"
          ],
          "subscriptions": [
            "webserver"
          ],
          "readonly": true
        },
        {
          "name": "operators",
          "members": [
            "myorganization/owners"
          ],
          "datacenters": [],
          "subscriptions": [],
          "readonly": false
        }
      ]
    }
    ~~~

ldap
: description
  : A hash of [Lightweight Directory Access Protocol (LDAP) authentication
    attributes](#ldap-authentication-attributes) to enable LDAP authentication.
    Overrides simple authentication.
: required
  : false
: type
  : Hash
: example
  : ~~~shell
    "ldap": {
      "server": "localhost",
      "port": 389,
      "basedn": "cn=users,dc=domain,dc=tld",
      "binduser": "cn=binder,cn=users,dc=domain,dc=tld",
      "bindpass": "secret",
      "roles": [
        {
          "name": "guests",
          "members": [
            "guests_group"
          ],
          "datacenters": [
            "us-west-1"
          ],
          "subscriptions": [
            "webserver"
          ],
          "readonly": true
        },
        {
          "name": "operators",
          "members": [
            "operators_group"
          ],
          "datacenters": [],
          "subscriptions": [],
          "readonly": false
        }
      ],
      "insecure": false,
      "security": "none",
      "userattribute": "sAMAccountName"
    }
    ~~~

audit
: description
  : A hash of [Audit Logging attributes](#audit-logging-attributes) to configure
    Audit Logging.
: required
  : false
: type
  : Hash
: example
  : ~~~shell
    "audit": {
      "logfile": "/var/log/sensu/sensu-enterprise-dashboard-audit.log",
      "level": "default"
    }
    ~~~


## Role-Based Access Controls (RBAC)

The Sensu Enterprise Dashboard provides comprehensive and granular Role-Based
Access Controls (RBAC), with support for using [GitHub.com](https://github.com),
a GitHub Enterprise installation, and/or a Lightweight Access Directory Provider
(LDAP) for authentication. RBAC for Sensu Enterprise enables administrators to
grant the correct level access to many different development and operations
teams, without requiring them to maintain yet another user registry.

Sensu Enterprise currently includes the following authentication drivers for
RBAC:

* [GitHub](#github-driver-for-rbac)
* [LDAP](#ldap-driver-for-rbac)

### GitHub Driver for RBAC

The Sensu Enterprise Dashboard ships with integrated support for using
[GitHub.com](https://github.com) or a [GitHub Enterprise](https://\
enterprise.github.com/home) installation for RBAC authentication. Please note
the following configuration attributes for configuring the GitHub driver:

#### GitHub authentication attributes

clientId
: description
  : The GitHub OAuth Application "Client ID"
    _NOTE: requires [registration of an OAuth application in GitHub](#register-an-oauth-application-in-github)._
: required
  : true
: type
  : String
: example
  : ~~~shell
    "clientId": "a8e43af034e7f2608780"
    ~~~

clientSecret
: description
  : The GitHub OAuth Application "Client Secret"
  _NOTE: requires [registration of an OAuth application in GitHub](#register-an-oauth-application-in-github)._
: required
  : true
: type
  : String
: example
  : ~~~shell
    "clientSecret": "b63968394be6ed2edb61c93847ee792f31bf6216"
    ~~~

server
: description
  : The location of the GitHub server you wish to authenticate against.
    _NOTE: currently, only GitHub.com is supported; there are known issues when
    attempting to connect to GitHub Enterprise servers that we are working on
    resolving and should have a fix for soon._
: required
  : true
: type
  : String
: example
  : ~~~shell
    "server": "https://github.com"`
    ~~~

roles
: description
  : An array of [Role attributes for GitHub
    Teams](#role-attributes-for-github-teams)
: required
  : true
: type
  : Array
: example
  : ~~~shell
    "roles": [
      {
        "name": "guests",
        "members": [
          "myorganization/guests"
        ],
        "datacenters": [
          "us-west-1"
        ],
        "subscriptions": [
          "webserver"
        ],
        "readonly": true
      },
      {
        "name": "operators",
        "members": [
          "myorganization/operators"
        ],
        "datacenters": [],
        "subscriptions": [],
        "readonly": false
      }
    ]
    ~~~

#### Role attributes for GitHub Teams

name
: description
  : The name of the role.
: required
  : true
: type
  : String
: example
  : ~~~shell
    "name": "operators"
    ~~~

members
: description
  : An array of the GitHub Teams that should be included as members of the role.
: required
  : true
: type
  : Array
: allowed values
  : any valid `organization/team` pair. For example, the team located at
    [https://github.com/orgs/sensu/teams/owners](https://github.com/orgs/sensu/\
    teams/owners) would be entered as `sensu/owners`.
: example
  : ~~~shell
    "members": ["myorganization/devs"]
    ~~~

datacenters
: description
  : An array of the `datacenters` (i.e. matching a defined [Sensu API endpoint
    `name`](#sensu-api-endpoint-attributes) value) that members of the role
    should have access to. Provided values will be used to filter which
    `datacenters` members of the role will have access to.
    _NOTE: omitting this configuration attribute or providing an empty array
    will allow members of the role access to all configured `datacenters`._
: required
  : false
: type
  : Array
: example
  : ~~~shell
    "datacenters": ["us-west-1"]
    ~~~

subscriptions
: description
  : An array of the subscriptions that members of the role should have access
    to. Provided values will be used to filter which subscriptions members of
    the role will have access to.
    _NOTE: omitting this configuration attribute or providing an empty array
    will allow members of the role access to all subscriptions._  
: required
  : false
: type
  : Array
: example
  : ~~~shell
    "subscriptions": ["webserver"]
    ~~~

readonly
: description
  : Used to restrict "write" access (i.e. preventing members of the role from
    being able to create stashes, silence checks, etc).
: required
  : false
: type
  : Boolean
: default
  : false
: example
  : ~~~ shell
    "readonly": true
    ~~~

#### Register an OAuth Application in GitHub

To use GitHub for authentication requires registration of your Sensu Enterprise
Dashboard as a GitHub "application". Please note the following instructions:

1. To register a GitHub OAuth application, please navigate to your GitHub
   organization settings page (e.g.
   `github.com/organizations/YOUR-GITHUB-ORGANIZATION/settings/applications`),
   and selection "Applications" => "Register new application".

   ![](img/enterprise-dashboard-github-app.png)

2. Give your application a name (e.g. "Sensu Enterprise Dashboard")

3. Provide the Authorization callback URL (e.g. `{HOSTNAME}/login/callback`)

   _NOTE: this URL does not need to be publicly accessible - as long as a user
   has network access to **both** GitHub.com **and** the callback URL, s/he will
   be able to authenticate; for example, this will allow users to authenticate
   to a Sensu Enterprise Dashboard service running on a private network as long
   as the user has access to the network (e.g. locally or via VPN)._

4. Select "Register application" and note the application Client ID and Client
   Secret.

   ![](img/enterprise-dashboard-github-secret.png)


## LDAP driver for RBAC

The Sensu Enterprise Dashboard ships with integrated support for using a
Lightweight Directory Access Protocol (LDAP) provider for RBAC authentication.
The LDAP driver for Sensu Enterprise has been tested with **Microsoft Active
Directory (AD)**, and should be compatible with any standards-compliant LDAP
provider. Please note the following configuration attributes for configuring the
LDAP driver:

### LDAP authentication attributes

This driver is tested with **Microsoft Active Directory** (AD) and should be
compatible with any standards-compliant Lightweight Directory Access Protocol
(LDAP) provider.

server
: description
  : **IP address** or **FQDN** of the LDAP directory or the Microsoft Active
    Directory domain controller.
: required
  : true
: type
  : String
: example
  : ~~~shell
    "server": "localhost"
    ~~~

port
: description
  : Port of the LDAP/AD service (usually `389` or `636`)
: required
  : true
: type
  : Integer
: example
  : ~~~ shell
    "port": 389
    ~~~

basedn
: description
  : Tells which part of the directory tree to search. For example,
    `cn=users,dc=domain,dc=tld` will search into all `users` of the
    `domain.tld` directory.
: required
  : true
: type
  : String
: example
  : ~~~ shell
    "basedn": "cn=users,dc=domain,dc=tld"
    ~~~

binduser
: description
  : The LDAP account that performs user lookups. We recommend to
    use a read-only account. Use the distinguished name (DN) format,
    such as `cn=binder,cn=users,dc=domain,dc=tld`.
    _NOTE: using a binder account is not required with Active Directory,
    although it is highly recommended._
: required
  : true
: type
  : String
: example
  : ~~~ shell
    "binduser": "cn=binder,cn=users,dc=domain,dc=tld"
    ~~~

bindpass
: description
  : The password for the binduser.
: required
  : true
: type
  : String
: example
  : ~~~ shell
    "bindpass": "secret"
    ~~~

insecure
: description
  : Determines whether or not to skip SSL certificate verification (e.g. for
    self-signed certificates).
: required
  : false
: type
  : Boolean
: default
  : false
: example
  : ~~~ shell
    "insecure": true
    ~~~

security
: description
  : Determines the encryption type to be used for the connection to the LDAP
    server.
: required
  : true
: type
  : String
: allowed values
  : `none`, `starttls`, or `tls`
: example
  : ~~~ shell
    "security": "none"
    ~~~

userattribute
: description
  : The LDAP attribute used to identify an account. You should typically use
    `sAMAccountName` for Active Directory and `uid` for other LDAP softwares,
    such as OpenLDAP, but it may vary.
: required
  : false
: type
  : String
: default
  : `sAMAccountName`
: example
  : ~~~ shell
    "userattribute": "uid"
    ~~~

userobjectclass
: description
  : The LDAP object class used for the user accounts.
: required
  : false
: type
  : String
: default
  : `person`
: example
  : ~~~ shell
    "userobjectclass": "inetOrgPerson"
    ~~~

groupobjectclass
: description
  : The LDAP object class used for the groups.
: required
  : false
: type
  : String
: default
  : `groupOfNames`
: example
  : ~~~ shell
    "groupobjectclass": "posixGroup"
    ~~~

roles
: description
  : An array of [Role attributes for LDAP groups](#role-attributes-for-ldap-groups)
: required
  : true
: type
  : Array
: example
  : ~~~shell
    "roles": [
      {
        "name": "guests",
        "members": [
          "guests_group"
        ],
        "datacenters": [
          "us-west-1"
        ],
        "subscriptions": [
          "webserver"
        ],
        "readonly": true
      },
      {
        "name": "operators",
        "members": [
          "operators_group"
        ],
        "datacenters": [],
        "subscriptions": [],
        "readonly": false
      }
    ]
    ~~~

#### Role attributes for LDAP groups

name
: description
  : The name of the role.
: required
  : true
: type
  : String
: example
  : ~~~shell
    "name": "operators"
    ~~~

members
: description
  : An array of LDAP groups that should be included as members of the role.
    _NOTE: Sensu Enterprise Dashboard supports the following LDAP group object
    classes: `group`, `groupOfNames`, `groupOfUniqueNames` and `posixGroup`._
: required
  : true
: type
  : Array
: allowed values
  : any LDAP group name
: example
  : ~~~shell
    "members": ["guests_group"]
    ~~~

datacenters
: description
  : An array of the `datacenters` (i.e. matching a defined [Sensu API endpoint
    `name`](#sensu-api-endpoint-attributes) value) that members of the role
    should have access to. Provided values will be used to filter which
    `datacenters` members of the role will have access to.
    _NOTE: omitting this configuration attribute or providing an empty array
    will allow members of the role access to all configured `datacenters`._
: required
  : false
: type
  : Array
: example
  : ~~~shell
    "datacenters": ["us-west-1"]
    ~~~

subscriptions
: description
  : An array of the subscriptions that members of the role should have access
    to. Provided values will be used to filter which subscriptions members of
    the role will have access to.
    _NOTE: omitting this configuration attribute or providing an empty array
    will allow members of the role access to all subscriptions._  
: required
  : false
: type
  : Array
: example
  : ~~~shell
    "subscriptions": ["webserver"]
    ~~~

readonly
: description
  : Used to restrict "write" access (i.e. preventing members of the role from
    being able to create stashes, silence checks, etc).
: required
  : false
: type
  : Boolean
: default
  : false
: example
  : ~~~ shell
    "readonly": true
    ~~~

## Audit Logging

As of Sensu Enterprise Dashboard version 1.3, Audit Logging is enabled by
default. Audit Logging captures user events in the dashboard such as user
login/logout, and any user "write" actions in the dashboard (i.e. silencing
checks, deleting clients, deleting stashes). Optionally, it is also possible to
log all `HTTP GET` requests (i.e. every view requested by the user, and every
search query performed by the user).

### Example configuration

~~~ shell
"audit": {
  "logfile": "/var/log/sensu/sensu-enterprise-dashboard-audit.log",
  "level": "default"
}
~~~

### Audit Logging attributes

logfile
: description
  : The location of the audit logging logfile.
: required
  : false
: type
  : String
: default
  : `/var/log/sensu/sensu-enterprise-dashboard-audit.log`
: example
  : ~~~shell
    "logfile": "/var/log/sensu/sensu-enterprise-dashboard-audit.log"
    ~~~

level
: description
  : The audit logging level.
: required
  : false
: type
  : String
: default
  : `default`
: allowed values
  : `default`, `verbose`, `disabled`
  _NOTE: `default` log level events are user login/logout, and any user "write"
  actions in the dashboard (i.e. silencing checks, deleting clients, deleting
  stashes). The `verbose` log level includes all of the `default` log level
  events, plus all `HTTP GET` requests (i.e. every view requested by the user,
  and every search query performed by the user)._
: example
  : ~~~shell
    "level": "verbose"
    ~~~
