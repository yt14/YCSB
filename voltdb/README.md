<!--
Copyright (c) 2012 - 2016 YCSB contributors. All rights reserved.

Licensed under the Apache License, Version 2.0 (the "License"); you
may not use this file except in compliance with the License. You
may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
implied. See the License for the specific language governing
permissions and limitations under the License. See accompanying
LICENSE file.
-->

# VoltDB Binding for YCSB

This section describes how to run YCSB on VoltDB. To know more about VoltDB, please visit our [Website](https://www.voltdb.com/) or [Github](https://github.com/VoltDB/voltdb).

Set Up YCSB
-------------------
To clone and build:

		git clone git://github.com/brianfrankcooper/YCSB.git
		cd YCSB
		mvn clean package 

Or, if you would like to use a released kit, you can do as follows:

		wget https://github.com/brianfrankcooper/YCSB/releases/download/0.10.0/ycsb-0.10.0.tar.gz
		tar -xfvz ycsb-0.10.0.tar.gz

Next you need to set YCSB_HOME:

		export YCSB_HOME="<ycsb directory>"

Start VoltDB
--------------------
First, you can get VoltDB from [https://github.com/VoltDB/voltdb](https://github.com/VoltDB/voltdb). Follow the instructions from the README.md file to build it.

Then cd to the ycsb folder:

		cd tests/test_apps/ycsb

To start or add a server to your cluster, invoke run.sh with the "server" parameter, passing the name of the leader host as the second parameter (You may need to invoke ```./run.sh clean``` to clean old VoltDB logs before starting the server):

    	./run.sh server [leader]

For example, to start a cluster with 3 nodes (host0(leader), host1, host2), set hostcount="3" in deployment.xml. Then type the following command on those hosts:

    	./run.sh server host0

If leader host is not provided, by default "localhost" is used.

You can also specify other server configurations such as hostcount, sitesperhost or kfactor in deployment.xml.

Lastly, let VoltDB load the schema and stored procedures:

    	./run.sh init

Run Workload
--------------------
Now you are ready to go! Go back to ycsb folder, you can load the data by typing:

		./bin/ycsb load voltdb -s -P workloads/workloada

Run a workload:

		./bin/ycsb run voltdb -s -P workloads/workloada

You can either specify the loading or running parameters by passing either parameters or a property file.   
For example, load 5000000 records to the server using 300 threads:

		./bin/ycsb load voltdb -s -P workloads/workloadc -p threadcount=300 -p recordcount=5000000

Here is another example: run workloadc using 300 threads. Client will stop running after operationcount reachs 100000000 or 120 seconds passed by, which ever comes first.

		./bin/ycsb run voltdb -s -P workloads/workloadc -p threadcount=300 -p operationcount=100000000 -p maxexecutiontime=120

All running properties can be found in [https://github.com/brianfrankcooper/YCSB/wiki/Core-Properties](https://github.com/brianfrankcooper/YCSB/wiki/Core-Properties)

More
--------------------
For more details about running a workload, see [https://github.com/brianfrankcooper/YCSB/wiki/Running-a-Workload](https://github.com/brianfrankcooper/YCSB/wiki/Running-a-Workload)
