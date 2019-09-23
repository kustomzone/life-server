# Life Server _(life-server)_

> A decentralized personal data server inspired by Solid

## Table of Contents

- [Background](#background)
- [Differences from Solid Server](#differences-from-solid-server)
- [Install](#install)
- [Usage](#usage)
- [Security](#security)
- [Contribute](#contribute)
- [License](#license)

## Background

Life Server is personal data server written in Node.js, originally
based on MIT's [Solid Server](https://github.com/solid/node-solid-server).

### Differences from `node-solid-server` (NSS)

This is an experimental server focusing on interop exploration and rapid 
feature iteration ("move fast and break things").

Since [`node-solid-server`](https://github.com/solid/node-solid-server) is being
deprecated in favor of [`inrupt/pod-server`](https://github.com/inrupt/pod-server),
this repo intends to be another compatible implementation (the more the merrier!).

#### Technical Differences from NSS

* The various built-in apps (account homepage, data viewing and file management,
  sharing and permission management, etc) are done on the server side.
* Experimental integration with [CouchDB](http://docs.couchdb.org/en/latest/intro/)
  (for synchronizing of graphs and documents to mobile and offline-first clients).
* Ongoing refactoring of the LDP backend to support pluggable storage (such as
  a NoSQL document store, a graph store and others).
* General cleanup and feature streamlining.
* Not published to `npm`, intended to be installed and run from git.

**Does not support:**

* `acl:origin` checking or Trusted Apps (uses [`solid-permissions`](https://github.com/interop-alliance/solid-permissions)  
    instead of [`acl-check.js`](https://github.com/solid/acl-check) for access control)
* Password strength checking on account signup.
* Enforcement of storage space quotas
* WebID-TLS local authentication
* WebSockets
* Globbing

#### Audience

This server is intended for the following audiences:

1. (primarily) Developers of user centric offline-first decentralized applications.
1. (to a much smaller extent) End-users interested in running their own file
   sharing server (a minimal Dropbox/Google Drive sort of setup).
1. (almost not at all) For system administrators / potential service providers
   interested in running their own multi-user data server.

To put it another way, due to a shortage of engineering resources, the
priorities will be: Developer QoL (Quality of Life) over User QoL over DevOps QoL.

### Value Proposition for Developers

Features:

* Self-service signup and account management
* Single user mode (for personal use) and multi-user mode (for teams,
  organizations and hosting providers)
* Cross-domain authentication based on a decentralized version of WebID +
  OAuth2/OpenID Connect
* Cross-domain access control (great for collaboration or document sharing
  between different companies or organizations)
* Provides each user with read/write data storage (that is accessible from any
  Javascript and server-side app). Think of it as an Amazon S3 service that is
  simpler to use, has nested folders, and has the option of being self-hosted.

Benefits for creating your web apps with this architecture:

1. A flexible cross-domain authentication and access control system is great for
   social-enabled apps and group collaborations. Also reduces account fatigue /
   password fatigue for users.
1. Data Ownership moves into the hands of your users, which reduces
   compliance risks for data storage.
1. Cross-app data sharing (with users' consent). Enables innovative horizontal
   use cases and apps.
1. "Warm Start" -- your app immediately has access to rich existing user data
   and social graph (great for AI/Machine Learning applications).
1. Offline-first (with synchronization to the user's storage servers) means
   a better user experience (reduced perceived response latency) and the ability
   to function in low-connectivity environments.
1. Integrates with your existing app Javascript development frameworks and tools
   (React, Vue.js, Ember.js, Express, and so on).

## Install

### Pre-requisites: Node.js v10

* Linux or Mac OS X
* Node 10
* [Optional] OpenSSL (for certificate generation)

**Operating System:** Linux and Mac OS X. Windows is currently not supported
for this project.

To run the Life Server server, you will first need to install
Node.js version 10 or higher. (The developers recommend using
[`nvm`](https://github.com/creationix/nvm) to install Node.)

(Optional) If you intend to create a self-signed certificate (for local testing),
you will also need OpenSSL.

### Install `life-server` from Github

```bash
git clone https://github.com/interop-alliance/life-server.git
cd life-server
npm install
```

### Prepare the SSL certificate

**Local/Development:** Installing the server for local development and testing
will require an SSL certificate. You can generate a self-signed certificate
yourself (see [Generating a self-signed SSL certificate](docs/ssl-certificates.md)
in `docs/`), but remember to launch the server using `./bin/solid-test` rather
than `./bin/solid`.

**Production:** Installing `life-server` in a production environment will
require a valid SSL certificate (self-signed certs will not work). In addition,
if you're running the server in Multi User mode, you will need a
[Wildcard Certificate](https://en.wikipedia.org/wiki/Wildcard_certificate).

### Edit `/etc/hosts` (development/testing only)

To run the account creation on unit tests, `life-server`'s test suite
uses the following localhost domains: `nic.localhost`, `tim.localhost`, and
`nicola.localhost`. You will need to create host file entries for these, in
order for the tests to pass.

Edit your `/etc/hosts` file, and append:

```
# Used for unit testing
127.0.0.1 nic.localhost
127.0.0.1 tim.localhost
127.0.0.1 nicola.localhost
```

### Generate a config file

The easiest way to setup `life-server` is by running the `init` wizard.
This will create a `config.json` in your current folder:

```
./bin/solid init
```

Going with the defaults is fine, but note that you will need the paths
(relative paths are ok) to your SSL key and certificate.

## Usage

To run your server (once you've generated a config file):

```
./bin/solid start
```

or when using a self-signed certificate:

```
./bin/solid-test start
```

After startup, the server is available at the configured server URL (by default,
`https://localhost:8443`).

## Security

TBD

## Contribute

Life Server is only possible because of a large community of [Solid contributors](https://github.com/solid/node-solid-server/blob/master/CONTRIBUTORS.md).
A heartfelt thank you to everyone for all of your efforts!

## License

[The MIT License](LICENSE.md)
