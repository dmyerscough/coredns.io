+++
title = "Compile Time Enabling or Disabling Plugins"
description = "Enable or Disable plugins when compiling CoreDNS"
tags = ["Documentation"]
draft = false
date = "2017-07-25T16:07:39+01:00"
author = "miek"
+++

CoreDNS' [plugins](/plugins) (or [external plugins](/explugins)) can be enabled or
disabled on the fly by specifying (or not specifying) it in the
[Corefile](/2017/07/23/corefile-explained/). But you can also compile CoreDNS with only the
plugin you *need* and leave the rest completely out.

All this is done via one compile-time configuration file,
[`plugin.cfg`](https://github.com/coredns/coredns/blob/master/plugin.cfg). It looks like this:

~~~
...
230:whoami:whoami
240:erratic:erratic
500:startup:github.com/mholt/caddy/startupshutdown
...
~~~

The number specifies the ordering of the plugin (they are called in this order - *if* enabled in
the [Corefile](/2017/07/23/corefile-explained/) - by CoreDNS). Then a **name** and a **repository**.
Just add or remove your plugin in this file.

Then do a `go get <plugin-repo-path>` if you need to get the external plugin's source code.
And then just compile CoreDNS with `go generate` and a `go build`. You can then check if CoreDNS has
the new plugin with `coredns -plugins`.
