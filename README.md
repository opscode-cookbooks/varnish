varnish Cookbook
================
Installs and configures varnish.


Requirements
------------
### Platforms

Tested on:

* Ubuntu 12.04
* Ubuntu 14.04
* Debian 6.0
* Centos 5.9
* Centos 6.5
* Centos 7.0

Attributes
----------
* `node['varnish']['dir']` - location of the varnish configuration
  directory
* `node['varnish']['default']` - location of the `default` file that
  controls the varnish init script on Debian/Ubuntu systems.
* `node['varnish']['version']` - If retrieving from official Varnish project repository, may choose 3.0 or 4.0.
* `node['varnish']['start']` - Should we start varnishd at boot?  Set to "no" to disable (yes)
* `node['varnish']['nfiles']` -  Open files (131072)
* `node['varnish']['memlock']` -  Maxiumum locked memory size for shared memory log (82000)
* `node['varnish']['instance']` - Default varnish instance name (node['fqdn'])
* `node['varnish']['listen_address']` -  Default address to bind to. Blank address (the default) means all IPv4 and IPv6 interfaces, otherwise specify a host name, an IPv4 dotted quad, or an IPv6 address in brackets
* `node['varnish']['listen_port']` - Default port to listen on (6081)
* `node['varnish']['vcl_conf']` - Name to use for main configuration file. (default.vcl.erb)
* `node['varnish']['vcl_source']` - Name for default configuration file template. (default.vcl)
* `node['varnish']['vcl_cookbook']` - Cookbook in which to look for the default.vcl.erb (or 'vcl_source' filename) template. This is used to specify custom template without modifying community cookbook files. (varnish)
* `node['varnish']['vcl_generated']` - Generate the varnish configuration using the supplied template. (true)
* `node['varnish']['conf_source']` - Name of the default system configuration file. (default.erb)
* `node['varnish']['conf_cookbook']` - Cookbook in which the default system configuration file is located. (varnish)
* `node['varnish']['secret_file']` - Path to a file containing a secret used for authorizing access to the management port. (/etc/varnish/secret)
* `node['varnish']['admin_listen_address']` - Telnet admin interface listen address (127.0.0.1)
* `node['varnish']['admin_listen_port']` - Telnet admin interface listen port (6082)
* `node['varnish']['user']` - Specifies the name of an unprivileged user to which the child process should switch before it starts  accepting  connections (varnish)
* `node['varnish']['group']` - Specifies  the name of an unprivileged group to which the child process should switch before it starts accepting connections (varnish)
* `node['varnish']['ttl']` - Specifies  a hard minimum time to live for cached documents. (120)
* `node['varnish']['storage']` - The storage type used ('file')
* `node['varnish']['storage_file']` -  Specifies either the path to the backing file or the path to a directory in which varnishd will create the backing file. Only used if using file storage. ('/var/lib/varnish/$INSTANCE/varnish_storage.bin')
* `node['varnish']['storage_size']` -  Specifies the size of the backing file or max memory allocation.  The size is assumed to be in bytes, unless followed by one of the following suffixes: K,k,M,m,G,g,T,g,% (1G)
* `node['varnish']['log_daemon']` - Specifies if the system `varnishlog` daemon dumping all the varnish logs into node['varnish']['log_file'] should be enabled. (false)
* `node['varnish']['log_file']` - Path to the log file (/var/log/varnish/varnish.log)
* `node['varnish']['log_pid_file']` - Path to the pid file (/var/run/varnishlog.pid)
* `node['varnish']['ncsa_daemon']` - Specifies if the system `varnishncsa` daemon dumping apache style logs into node['varnish']['ncsa_file'] should be enabled. (true)
* `node['varnish']['ncsa_file']` - Path to the ncsa log file (/var/log/varnish/varnishncsa.log)
* `node['varnish']['ncsa_pid_file']` - Path to the ncsa pid file (/var/run/varnishncsa.pid)
* `node['varnish']['ncsa_format']` - Specifies the ncsa log format ('%h %l %u %t \"%r\" %s %b \"%{Referer}i\" \"%{User-agent}i\"')
* `node['varnish']['logrotate.d_path']` - Path to the varnish log rotation configuration (/etc/logrotate.d/varnish)
* `node['varnish']['parameters']` - Set the parameter specified by param to the specified value. See Run-Time Parameters for a list of parameters. This option can be used multiple times to specifymultiple parameters.

If you don't specify your own vcl_conf file, then these attributes are used in the cookbook `default.vcl` template:

* `node['varnish']['backend_host']` = Host to serve/cache content from (localhost)
* `node['varnish']['backend_port']` = Port on backend host to access (8080)


Recipes
-------
### default
Installs the varnish package, manages the default varnish configuration file, and the init script defaults file.

### repo
If placed before the default recipe in the run list, the official Varnish project apt repository will offer access to more version and platform support.

Usage
-----
On systems that need a high performance caching server, use `recipe[varnish]`. Additional configuration can be done by modifying the `default.vcl.erb` and `default.erb` templates.

If running on a Redhat derivative then you may need to include yum-epel as it provides the jemalloc dependency that varnish needs

Resources
-----
See the distro_install and vendor_install recipes for examples of these resources in action.

### varnish_install
Installs Varnish with the default configuration supplied by the package.

The `:install` action handles package installation. By default, it
will install Varnish from your distro repositories. If you set the
`vendor_repo` parameter to `true`, then it will install Varnish
from the varnish-cache repositories.

#### Parameters
| Name | Type | Default Value |
-------|------|---------------|
| `package_name` | string | `'varnish'` |
| `vendor_repo` | `true` or `false` | `false` |
| `vendor_version` | string | `'4.0'` |

#### Actions
- `:install` - Installs and enables the Varnish service.

### varnish_default_config
Configures the Varnish service. If you do not include this, the config
files that come with your distro package will be used instead.

| Name | Type | Default Value |
|------|------|---------------|
|`start_on_boot` | `true` or `false` | `true` |
|`max_open_files` | integer | `131_072` |
| `max_locked_memory` | integer | `82_000` |
| `instance_name` | string | `nil` |
| `listen_address` | string | `nil` |
| `listen_port` | integer | `6081` |
| `admin_listen_address` | string | `'127.0.0.1'` |
| `admin_plisten_port` | integer | `6082` |
| `user` | string | `'varnish'` |
| `group` | string | `'varnish'` |
| `ttl` | integer | `120` |
| `storage` | `'malloc'` or `'file'` | `'file'` |
| `file_storage_path` | string | `nil` |
| `file_storage_size` | string | `'1G'` |
| `malloc_size` | string | `nil` |
| `path_to_secret` | string | `'/etc/varnish/secret'` |

You can also send a hash to `parameters` which will add additional parameters to the varnish daemon via the `-p` option. The default hash is:

```
{ 'thread_pools' => '4',
  'thread_pool_min' => '5',
  'thread_pool_max' => '500',
  'thread_pool_timeout' => '300' }
```

#### Actions
- `:configure` - Creates the varnish configuration file from template.

### varnish_default_vcl
| Name | Type | Default Value |
|------|------|---------------|
| `backend_host` | string | `'localhost'`
| `backend_port` | integer | `8080` |


#### Actions
- `:configure` - Creates a default.vcl file.

### varnish_log
Configures varnishlog or varnishncsa service. You can define multiple
logfiles by calling `varnish_log` more than once.

| Name | Type | Default Value |
|------|------|---------------|
| `file_name` | string | `'/var/log/varnish/varnishlog.log'` |
| `pid` | string | `'/var/run/varnishlog.pid'` |
| `log_format` | `'varnishlog'` or `'varnishncsa'` | `'varnishlog'` |
| `ncsa_format_string` | string | `'%h|%l|%u|%t|\"%r\"|%s|%b|\"%{Referer}i\"|\"%{User-agent}i\"'`
| `instance_name` | string | `nil` |

#### Actions
- `:configure` - configures the `varnishlog` or `varnishncsa` service.

License & Authors
-----------------
- Author:: Joe Williams <joe@joetify.com>
- Author:: Lew Goettner <lew@goettner.net>
- Author:: Matthew Thode <matt.thode@rackspace.com>
- Contributor:: Patrick Connolly <patrick@myplanetdigital.com>
- Contributor:: Antonio Fernández Vara <antoniofernandezvara@gmail.com>

```text
Copyright:: 2008-2009, Joe Williams

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
