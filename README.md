What's this?
------------

This repository contains a sample application for the [Sailor MVC](http://sailorproject.org/) framework, set up to work with the [Lwan web server](https://lwan.ws).  In a nutshell:

  - It contains the output of `sailor create sailor-hello-lwan`, without any changes whatsoever;
  - Contains a list of dependencies as [Git submodules](https://git-scm.com/docs/git-submodule), including Sailor MVC itself;
  - A configuration file for Lwan.

Getting started
---------------

  1. Clone Lwan: `git clone https://github.com/lpereira/lwan`;
  2. Follow its build instructions. You'll need Lua 5.1 (from PUC-Rio) or LuaJIT 2.0. Note: although Lwan will build on OS X and FreeBSD, it uses [pkg-config](https://www.freedesktop.org/wiki/Software/pkg-config/) to find the Lua libraries; while this tool is available for these operating systems, the `.pc` file necessary to find the Lua libraries is not usually shipped with Lua itself;
  3. There's no need to install Lwan, but note the path to the generated `lwan` binary.  It's located in the build directory, under the `lwan` directory;
  3. Clone this repository: `git clone https://github.com/lpereira/sailor-hello-lwan`;
  4. Fetch the submodules: `git submodule init` and then `git submodule update`;
  5. Execute Lwan from the `sailor-hello-lwan` directory: `/path/to/build/lwan/lwan -c conf/lwan.conf`;
  6. It should be working! [Click here to test](http://localhost:9090);
  7. Now that it's working, consult the **Things to fix** section below. That's where the fun starts. *Happy hacking!*

Things to fix
-------------

Not everything is working in Lwan yet. Some things are easier to fix than others, and all help is welcome. It would be nice to support the following features:

  * Support Friendly URLs. Lwan already support rewriting requests using Lua functions; it's just the matter of adapting the set up in `nginx.conf` to `lwan.conf`;
  * Cookie support (for sessions, etc) is working, but currently requires minor changes inside Sailor. Ideally these abstractions should go to Remy;
  * Only GET requests are supported.  Lwan supports POST as well, but the Remy shim only reports that the request is a GET request through its `method` member in the `request` table;
  * The `hostname`, `useragent_ip`, and `port` fields in the Remy object are hardcoded to `localhost`, `127.0.0.1`, and `8080` respectively.  In fact, most constants in the `request` table are wrong.  This table should have a metatable and fetch the necessary fields on demand;
  * While it's possible to send arbitrary header values with the response, it's not possible to read arbitrary header values from the request. It should be possible to create a metatable in Remy to offer this capability;
  * Make Lwan find the Lua libraries without `pkg-config` as well;
  * There are many others. Please [open an Issue](https://github.com/lpereira/sailor-hello-lwan/issues) should you find other things worth fixing.

License
-------

The generated code is licensed under the same terms as Sailor MVC itself. The Lwan configuration file is licensed under [CC0](https://wiki.creativecommons.org/wiki/CC0).
