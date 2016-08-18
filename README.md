# README

[![GitHub license](https://img.shields.io/badge/license-GPLv3-blue.svg)](https://raw.githubusercontent.com/pbowditch/dotfiles/master/COPYING)
[![GitHub release](https://img.shields.io/github/release/pbowditch/dotfiles.svg)](https://github.com/pbowditch/dotfiles/releases/latest)

The dotfiles project is intended to be used as a centralised store of role based configuration required in a UNIX environment.  This information consists (but is not limited to):

  - Environment Variables
  - Aliases
  - Functions
  - Files
  - Symbolic Links
  - Paths
  - Templates

The project came about in an organisation where there were 40+ developers working across 20+ servers, many of which shared multiple roles and responsibilities including architecture, development, database administration, gui development and so forth.  With the ease of spinning up virtual machines, the increased ingestion of "Big Data" and the demands it places on scalability a framework was required whereby users could share configuration
data in a version controlled manner.  With so many developers (multipled by the myriad of test and production environments) user configurations were almost always non standard and required significant effort at the time 
to setup and debug.

> The overriding design goal for dotfiles is to make it as easy as possible to maintain user configuration information
> in a simplistic, centralised and pure UNIX manner.  It is primarily designed for command line environments and builds
> on the concept of roles in an additive fashion.  

### Version
(https://raw.githubusercontent.com/pbowditch/dotfiles/master/VERSION)

### Tech

dotfiles is a pure shell based solution and has no other dependencies other than a POSIX compliant shell such as bash or ksh.  

### Installation

Install dotfiles from git into your local directory (we suggest using ~/projects as the parent folder so your home directory doesnt get too cluttered).  

```sh
$ git clone git@mybox:dotfiles.git
```

Add the following lines to your .profile (replacing the <role> marker with the name of the role you wish to inherit).  
```sh
$ export DOTFILES_HOME=$HOME/projects/dotfiles
$ . $DOTFILES_HOME/bin/dotfile request <role>
```

You can add multiple roles by simple chaining of commands
```sh
$ . $DOTFILES_HOME/bin/dotfile request <role> && . $DOTFILES_HOME/bin/dotfile request <role>
```

To list what roles are available you can run the following:
```sh
$ $DOTFILES_HOME/bin/dotfile list
```

### Products

It is recommended to add base software / technical configuration under the ~/projects/dotfiles/products folder
in a shell script that follows the naming convention 

```sh
.<product>.profile
```

The following are preconfigured for use:

* Ab Initio
* Cognos
* CVS
* GIT
* Hadoop
* Netezza
* Oracle

### Roles

The primary mechanism for storing role configuration is via thin shell wrappers under ~/projects/dotfiles/roles
In this directory each script should be named exactly as the role name, e.g. "dba" or "administrator".  These wrappers
in turn initialise specific product scripts mentioned above to fulfill their roles, but being shell scripts they can
also perform many more functions such as setting environment variables based on the hostname, conditional inclusion
of roles based on the date or defining simple aliases for use if a terminal is present.  

The following are preconfigured for use:

* ai_developer
* ai_operator
* dba
* ddl_developer
* hadoop_developer

### Todos

 - Diff roles
 - Support Bash
 - Support SPSS and Syncsort

