# monit_bin cookbook
[![Build Status](https://secure.travis-ci.org/higanworks-cookbooks/monit_bin.png?branch=master)](https://travis-ci.org/higanworks-cookbooks/monit_bin)

* Install monit from source.
* Include setting tools monitensite, monitdisite.
* Add LWRP for created configs.

# Requirements

* make (build-essential)

# Platform

- ubuntu
- SmartOS

# Usage

`recipe[monit]` to default install.

### call from other recipe

```
include_recipe "monit_binaries"

----
  put config from template to /etc/monit/conf.avail/
----

# enable
monit_binaries "myapp.conf"

# disable
monit_binaries "myapp.conf" do
  enable false
end
```


### monitensite monitdisite

These tools control monit setting like `a2ensite`, `a2disite`.

Put your config to `/etc/monit/conf.avail/` and...

*Enable*

```
monitensite postfix.conf  
monit reload
```

*Disable*

```
monitdisite postfix.conf
monit reload
```

# Attributes

TODO: Write attributes...

# Resources and Providers

### monit_bin

Call monitensite and monit disite.

*Example*
```
monit_bin "postfix" do
  action :enable
end
```

### monit_bin_check_system

Build config for system resource with policy strings.

*Example*
```
monit_bin_check_system "localperf" do
  action :create
  policies ["if memory usage > 70 % then alert"]
end
```

### monit_bin_check_filesystem

Build config for filesystem resource with policy strings.

*Example*
```
monit_bin_check_filesystem "rootfs" do
  action :create
  path "/"
  policies ["if space usage > 70 % then alert"]
end
```

### monit_bin_check_process

Build config for process resource with policy strings.

*Example*
```
monit_bin_check_process "sshd" do
  action :create
  type "pid"
  pidfile "/var/run/sshd.pid "
  start_program "/usr/sbin/service ssh start"
  stop_program "/usr/sbin/service ssh stop"
end
```


# Recipes

* default: install monit bins using Chef Ark.
* source:  install monit from source.
* include: define monit as service resource.
* services: monitoring services. setting from attributes.
* smartos_inittab: install and regist inittab for smartos. Smartos use this insted of default.

# FAQ

## monit status/summary doesn't work?

When using monit without `m/monit`, You should set `127.0.0.1` or `0.0.0.0` to `node[:monit][:monitrc][:'http_address]` attribute.


# Test

`kitchen test`

# Author

Author:: HiganWorks LLC (<sawanoboriyu@higanworks.com>)
Maintainer:: Myles Steinhauser (<myles.steinhauser@gmail.com>)
