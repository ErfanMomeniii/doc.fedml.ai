---
sidebar_position: 3
---
# FEDML Launch APIs

Simple launcher apis for running any AI job across multiple public and/or decentralized GPU clouds, offering lower prices without cloud vendor lock-in, the highest GPU availability, training across distributed low-end GPUs, and user-friendly Ops to save time on environment setup.


### launch_job()

Launch a job on the FedML AI Nexus platform

```py
fedml.api.launch_job(yaml_file, api_key=None, resource_id=None, device_server=None, device_edges=None)
```

**Arguments**  
- `yaml_file (str)`: Full path of your job yaml file.
- `api_key (str=None)`: Your API key from FedML AI Nexus platform (if not configured already).
- `resource_id (str=None)`: Specific `resource_id` to use. Typically, you won't need to specify a specific `resource_id`. Instead, we will match resources based on your job yaml, and then automatically launch the job using matched resources.
- `device_server (str=None)`: `device_server` to use. Only needed when you want to launch a federated learning job with specific `device_server` and `device_edges`
- `device_edges (List[str]=None): List of `device_edges` to use. Only needed when you want to launch a federated learning job with specific `device_server` and `device_edges`

**Returns**  
`LaunchResult` object with the following attributes:
- `result_code (int)`: API result code. `0` means success. Full list of result codes can be found [here](./api-result-codes.md).
- `result_msg (str)`: API status message.
- `run_id (str)`: Run ID of the launched job.
- `project_id (str)`: Project Id of the launched job. This is default assigned if not specified in your job yaml file
- `inner_id (str)`: Serving endpoint id of launched job. Only applicable for Deploy / Serve Job tasks, and will be `None` otherwise. 


**Example Usage**
```py

import fedml
api_key="YOUR_API_KEY"
yaml_file = "/home/fedml/train.yaml"
login_ret = fedml.api.fedml_login(api_key)
if login_ret == 0:
    launch_result = fedml.api.launch_job(yaml_file)
    if launch_result.result_code == 0:
        print("Job launched successfully")
    else:
        print("Failed to launch job")
```


### `launch_job_on_cluster()`

Launch a job on a cluster on the FedML AI Nexus platform

```py
fedml.api.launch_job_on_cluster(yaml_file, cluster, api_key=None, resource_id=None, device_server=None, device_edges=None)
```

**Arguments**  
- `yaml_file (str)`: Full path of your job yaml file.
- `cluster (str)`: Cluster name to use. If a cluster with provided name doesn't exist, one will be created.
- `api_key (str=None)`: Your API key from FedML AI Nexus platform (if not configured already).
- `resource_id (str=None)`: Specific `resource_id` to use. Typically, you won't need to specify a specific `resource_id`. Instead, we will match resources based on your job yaml, and then automatically launch the job using matched resources.
- `device_server (str=None)`: `device_server` to use. Only needed when you want to launch a federated learning job with specific `device_server` and `device_edges`
- `device_edges (List[str]=None): List of `device_edges` to use. Only needed when you want to launch a federated learning job with specific `device_server` and `device_edges`


**Returns**  
`LaunchResult` object with the following attributes:
- `result_code (int)`: API result code. `0` means success. Full list of result codes can be found [here](./api-result-codes.md).
- `result_msg (str)`: API status message.
- `run_id (str)`: Run ID of the launched job.
- `project_id (str)`: Project Id of the launched job.
- `inner_id (str)`: Serving endpoint id of launched job. Only applicable for Deploy / Serve Job tasks, and will be `None` otherwise. 


**Example Usage**
```py
import fedml
api_key="YOUR_API_KEY"
yaml_file = "/home/fedml/train.yaml"
login_ret = fedml.api.fedml_login(api_key)
if login_ret == 0:
    launch_result = fedml.api.launch_job_on_cluster(yaml_file, cluster="my_cluster")
    if launch_result.result_code == 0:
        print("Job launched successfully")
    else:
        print("Failed to launch job")
```

### `run_stop()`

Stop a run on FedML AI Nexus platform. 

```PY
fedml.api.run_stop(run_id, platform="falcon", api_key=None)
```

**Arguments**  
- `run_id (str)`: Id of the run to stop. Each run has a unique identifier that should have been returned LaunchResult after launching a job and can also be found out from the Runs page on FedML AI Nexus Platform.
- `platform (str=falcon)`: The platform name at the FedML® Nexus AI Platform (options: octopus, parrot, spider, beehive, falcon, launch, default is falcon)
- `api_key (str=None)`: Your API key from FedML AI Nexus platform (if not configured already).

**Returns**  
Boolean indicating whether the run was successfully stopped or not.


### `run_list()`

List a run on FedML AI Nexus platform.

```py
fedml.api.run_list(run_name, run_id=None, platform="falcon", api_key=None)
```

**Arguments**
- `run_name (str)`:Name of the run. This can also be found out from the Runs page on FedML AI Nexus Platform.
- `run_id (str=None)`: Id of the run to list (Only required if run_name is not provided). Each run has a unique identifier that should have been returned LaunchResult after launching a job and can also be found out from the Runs page on FedML AI Nexus Platform.
- `platform (str=falcon)`: The platform name at the FedML® Nexus AI Platform (options: octopus, parrot, spider, beehive, falcon, launch, default is falcon)
- `api_key (str=None)`: Your API key from FedML AI Nexus platform (if not configured already).

**Returns**  
`FedMLRunModelList` object which is a list of `FedMLRunModel` objects with attributes like `status`, `running_time`, `cost`, `run_url` etc.


### `run_status()`

Get status a run on FedML AI Nexus platform.

```py
fedml.api.run_status(run_name, run_id, platform: str = "falcon", api_key: str = None)
```

**Arguments**
- `run_name (str)`:Name of the run. This can also be found out from the Runs page on FedML AI Nexus Platform.
- `run_id (str)`: Id of the run to get status of (Only required if run_name is not provided). Each run has a unique identifier that should have been returned LaunchResult after launching a job and can also be found out from the Runs page on FedML AI Nexus Platform.
- `platform (str=falcon)`: The platform name at the FedML® Nexus AI Platform (options: octopus, parrot, spider, beehive, falcon, launch, default is falcon)
- `api_key (str=None)`: Your API key from FedML AI Nexus platform (if not configured already).

**Returns**  
Tuple of `FedMLRunModelList` and `status (str)` denoting status of the run.


### `run_logs()`

Fetches logs of run from FedML AI Nexus platform.

```py
fedml.api.run_logs(run_id, page_num, page_size, need_all_logs=False, platform="falcon", api_key=None)
```

**Arguments**
- `run_id (str)`: Id of the run to fetch logs of. Each run has a unique identifier that should have been returned LaunchResult after launching a job and can also be found out from the Runs page on FedML AI Nexus Platform.
- `platform (str=falcon)`: The platform name at the FedML® Nexus AI Platform (options: octopus, parrot, spider, beehive, falcon, launch, default is falcon)
- `api_key (str=None)`: Your API key from FedML AI Nexus platform (if not configured already).

**Returns**  
`RunLogResult` object with the following attributes:
- `run_status (str)`: Status of the run.
- `total_log_lines (int)`: Total number of log lines.
- `total_log_pages(int)`: Total number of log pages.
- `log_line_lise (List[str])`: Full List of log lines.
- `run_logs (FedMLRunLogModelList)`: Object with attributes like `log_lines`, `log_full_url` and `log_devices` etc.


### `cluster_list()`

List clusters associated with your account on FedML AI Nexus platform.

```py
fedml.api.cluster_list(cluster_names=(), api_key=None)
```

**Arguments**
- `cluster_names (Tuple[str])`: List of cluster names. Defaults to empty, which means all clusters will be listed.
- `api_key (str=None)`: Your API key from FedML AI Nexus platform (if not configured already).

**Returns**  
`FedMLClusterModelList` object with the following attributes:
- `cluster_list (FedMLClusterModel)`: Object with following attribute
  - `cluster_name (str)`: Name of the cluster.
  - `cluster_id (str)`: Id of the cluster.
  - `status (str)`: Status of the cluster.


### `cluster_exists()`

Check whether cluster with provided name exists on your account on FedML AI Nexus platform.

```py
fedml.api.cluster_exists(cluster_name, api_key=None)
```

**Arguments**
- `cluster_name (str)`: Name of cluster
- `api_key (str=None)`: Your API key from FedML AI Nexus platform (if not configured already).

**Returns**  
Boolean indicating whether the cluster with provided name exists or not.

### `cluster_status()`

Check status of your cluster on FedML AI Nexus platform.

```py
fedml.api.cluster_status(cluster_name, api_key=None)
```

**Arguments**
- `cluster_name (str)`: Name of cluster
- `api_key (str=None)`: Your API key from FedML AI Nexus platform (if not configured already).

**Returns**  
Tuple (`str`(status), `FedMLClusterModelList`). More about `FedMLClusterModelList` can be found [here](#clusterlistclusternames-apikeynone).


### `cluster_start()`

Start selected clusters on FedML AI Nexus platform.

```py
fedml.api.cluster_start(cluster_names: Tuple[str], api_key=None)
```

**Arguments**
- `cluster_name (Tuple[str])`: Tuple of cluster names to start.
- `api_key (str=None)`: Your API key from FedML AI Nexus platform (if not configured already).

**Returns**  
Boolean indicating whether the clusters were successfully started or not.


### `cluster_startall()`

Start all existing clusters on your account on FedML AI Nexus platform.

```py
fedml.api.cluster_startall(api_key=None)
```

**Arguments**
- `api_key (str=None)`: Your API key from FedML AI Nexus platform (if not configured already).

**Returns**  
Boolean indicating whether the clusters were successfully started or not.


### `cluster_stop()`

Stop selected clusters on FedML AI Nexus platform.

```py
fedml.api.cluster_stop(cluster_names: Tuple[str], api_key=None)
```

**Arguments**
- `cluster_name (Tuple[str])`: Tuple of cluster names to stop.
- `api_key (str=None)`: Your API key from FedML AI Nexus platform (if not configured already).

**Returns**  
Boolean indicating whether the clusters were successfully stopped or not.


### `cluster_stopall()`

Stop all existing clusters on your account on FedML AI Nexus platform.

```py
fedml.api.cluster_stopall(api_key=None)
```

**Arguments**
- `api_key (str=None)`: Your API key from FedML AI Nexus platform (if not configured already).

**Returns**  
Boolean indicating whether the clusters were successfully stopped or not.


### `cluster_kill()`

Kill (Tear Down) selected clusters on FedML AI Nexus platform.

> **_NOTE:_** Note that kill is different from stop. Clusters once killed cannot be restarted.

```py
fedml.api.cluster_kill(cluster_names: Tuple[str], api_key=None)
```

**Arguments**
- `cluster_name (Tuple[str])`: Tuple of cluster names to stop.
- `api_key (str=None)`: Your API key from FedML AI Nexus platform (if not configured already).

**Returns**  
Boolean indicating whether the clusters were successfully killed or not.


### `cluster_killall()`

Kill (Tear Down) all existing clusters on your account on FedML AI Nexus platform. 

> **_NOTE:_** Note that kill is different from stop. Clusters once killed cannot be restarted.

```py
fedml.api.cluster_killall(api_key=None)
```
**Arguments**
- `api_key (str=None)`: Your API key from FedML AI Nexus platform (if not configured already).

**Returns**  
Boolean indicating whether the clusters were successfully killed or not.