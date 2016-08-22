# dotfiles

[![GitHub license](https://img.shields.io/badge/license-GPLv3-blue.svg)](https://raw.githubusercontent.com/NeoOrigin/dotfiles/master/COPYING)
[![GitHub release](https://img.shields.io/github/release/pbowditch/dotfiles.svg)](https://github.com/NeoOrigin/dotfiles/releases/latest)

The dotfiles project is intended to be used as a centralised store of role based configuration required in a UNIX environment.  Think, a virtualenv that can activated or deactivated to manage user configuration.  This information consists of (but is not limited to):

  - Environment Variables
  - Aliases
  - Functions
  - Files
  - Symbolic Links
  - Paths
  - Templates

It is not uncommon both within startups or well established I.T departments that a user may have multiple roles and responsibilities including architecture, development, database administration, gui development and so forth.  With the ease of spinning up virtual machines, the increased ingestion of "Big Data" and the demands it places on scalability a framework was required whereby users could share configuration data in a version controlled manner.  Especially so where multiple versions of that software need to co-exist or are distributed geographically.  With so many developers (multipled by the myriad of test and production environments) user configurations are almost always the last item to be version controlled, are therefore non standard and require significant effort to setup and debug.

> The overriding design goal for dotfiles is to make it as easy as possible to maintain user configuration information
> in a simplistic, centralised and pure UNIX manner.  It is primarily designed for command line environments and builds
> on the concept of roles in an additive fashion.  

### Tech

dotfiles is a pure shell based solution and has no other dependencies other than a POSIX compliant shell such as bash, zsh or ksh.  

### Installation

Install dotfiles from git into your local directory (we suggest using ~/projects as the parent folder so your home directory doesnt get too cluttered).  

```sh
$ git clone https://github.com/NeoOrigin/dotfiles
```

Add the following lines to your .profile (replacing the <role> marker with the name of the role you wish to inherit).  NOTE it is important to use the leading dot (.) to source the script when requesting or releasing roles.  This enables manipulation of the environment from the current shell.
```sh
$ export DOTFILES_HOME=$HOME/projects/dotfiles
$ . $DOTFILES_HOME/bin/dotfile request <role>
```

You can add multiple roles by simple chaining of commands, or the preferred approach would be to use a role that encapsulates the common dependencies.  Shown later.  
```sh
$ . $DOTFILES_HOME/bin/dotfile request <role> && . $DOTFILES_HOME/bin/dotfile request <role>
```

To list what roles are available you can run the following:
```sh
$ $DOTFILES_HOME/bin/dotfile list
```

To release a role, and to effectively reset your environment run:
```sh
$ . $DOTFILES_HOME/bin/dotfile release <role>
```
NOTE: Some actions cannot be easily reversed, however simple configurations such as symlinking, environment variables and aliases etc have very little impact when released.  Especially if version controlled and managed with this tool, you can then make a new request from the current or a new shell.  

### Roles

The primary mechanism for storing role configuration is via thin shell wrappers under ~/projects/dotfiles/roles (or in a users home directory under the ~/.roles directory).  

In this directory each script should be named exactly as the role name, e.g. "dba" or "administrator".  These wrappers
define environment characteristics to fulfill their roles, e.g. setting up environment variables, symlinking to a version controlled login script, deploying a template to a local directory etc.  

This is mostly defined in a modularized fashion, creating configuration files under the role.  For example an example role that defines both environment variables and a dependency on another role could look like the following:

.oracle
    env
    aliases
    templates
        tnsnames.ora.j2
        .odbc.ini.j2

oracle_dba
    dependencies
    env

### Roles Vs Software

Software can be used for different purposes.  For example a java virtual machine might be configured differently for development purposes compared to running a server based process.  For this purpose we recommend seperating out common software specifics into a distinct role and defining a new role to encapsulate your users role.  e.g.

```sh
.java
java_developer
java_operator
```

### Todos

 - Diff roles
 - Support Bash
 - Support SPSS and Syncsort
 - Comprehensive documentation
 - Option to spawn new shells with desired config rather than modify the existing.  
