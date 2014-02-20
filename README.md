[![Build Status](https://travis-ci.org/spheromak/etcd-cookbook.png)](https://travis-ci.org/spheromak/etcd-cookbook)

etcd cookbook - installs coreos/etcd on centos/ubuntu boxen

## Supported Platforms
Centos/rhat 6+ & ubuntu with upstart

## Recipes
| Name | Description |
|:-----|:------------|
| `default` | Install and setup the service
| `binary_install` | Installs the binary of etcd from github release tarballs
| `cluster` | Recipe to aide in the building of multi-node etcd clusters
| `compile_time` | Install and configure etcd during chef compile phase
| `source_install` | Compiles the binary of etcd from a specificed github repo/revision
| `_service` | Recipe used by default for setting up the service

## Attributes

| attribute | default setting | description |
|:---------------------------------|:---------------|:-----------------------------------------|
|`default[:etcd][:install_method]`| binary | Right now only binary and source are supported. In the future this will probably go away as there are actual distro packages |
|`default[:etcd][:args]`| | Extra arguments to pass to etcd when starting the service. |
|`default[:etcd][:version]` | 0.3.0 | The release versions to install. binary install will assemble a github url to grab this version |
|`default[:etcd][:sha256]` | 726bd3.. | The Sha256 hash of the tarball specified by the version or URL attribute|
|`default[:etcd][:url]` | nil |override the internal generated url to specify your own binary tarball. see the .kitchen.yml for example override |
|`default[:etcd][:state_dir]` | /var/cache/etcd/state | Where etcd will store its state |
|`default[:etcd][:search_cook]`| etcd\:\:cluster | The cookbook that should be searched for on the nodes recipes to detect if it is also running etcd |
|`default[:etcd][:nodes]`| [] | Array of hostnames in cluster. This provides an alternative method to using Chef's `partial_search` for specifying nodes in cluster (useful if using `chef-solo`)  |
|`default[:etcd][:snapshot]`| true | This is really important to get good memory usage. If you're running this in product, you probably want this set to `true` |
|`default[:etcd][:local]`| true | Etcd will bind to all interfaces (this will be renamed in a future release) |
|`default[:etcd][:seed_node]` | nil | The seed node for initial cluster setup. This node will start as the master, but restarts will rejoin the raft cluster. This needs to be set when using cluster recipe otherwise the cluster will not initialize.|
|`default[:etcd][:discovery]` | '' | Discovery address/token see: https://coreos.com/docs/cluster-management/setup/etcd-cluster-discovery/ for more info
|`default[:etcd][:env_scope]` | true | Set the search in cluster recipe to restrict to nodes in the same environment
|`default[:etcd][:trigger_restart]` | true | Make etcd restart if the init config is updated
|`default[:etcd][:upstart][:start_on]` | started networking | When to start the etcd service using upstart
|`default[:etcd][:upstart][:stop_on]` | shutdown | When to stop the etcd service using upstart

## Usage


#### Default single instance single node:
````
run_list[etcd]
````

#### Setup a cluster
In a role or wrapper cookbook setup the seed_node attribute and add the cluster recipe to each node in the cluster.

If you use a wrapper cookbook set `node[:etcd][:search_cook]` to the wrapper cookboks name
````
run_list[etcd::cluster]
````

example wrapper can be seen here [http://github.com/cloudware-cookbooks/ktc-etcd]

## License and Author

|                      |                                                |
|:---------------------|:-----------------------------------------------|
| **Original Author**  | [Jesse Nelson]( https://github.com/spheromak)  |
| **Contributor**      | [Soulou](https://github.com/Soulou)    |
| **Contributor**      | [Aaron O'Mullan](https://github.com/AaronO)    |
| **Contributor**      | [Anthony Scalisi](https://github.com/scalp42)  |
| **Contributor**      | [Robert Coleman](https://github.com/rjocoleman)|
| **Contributor**      | [James Gregory](https://github.com/jagregory)  |
| **Copyright**        | Copyright (c) 2013, Jesse Nelson               |

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
