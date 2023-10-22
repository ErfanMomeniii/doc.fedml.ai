# Command Line Interfaces

# Launch default commands:
fedml launch job.yaml will trigger a job which has task_type: train, deploy, cross-cloud-train, federate, develop, will run on a cluster and then terminate the matched cluster afterwards. 
fedml launch -c cluster_name job.yaml 
fedml run -c cluster_name job.yamlwill trigger a job on cluster_name
fedml run -c cluster_name --env MY_ENV_VARIABLE python hello_world.py

# Run jobs in a specific server.
fedml run -sid $server_id job.yaml

# Cluster-related:
fedml cluster stop/start/shutdown/status/list $clustername
# (note here: status is included)

# Job-related:
fedml job stop/status/list/log $job_name
# (note: log is handled by fedml job log $job_name)

# Model-related:
fedml model
fedml model --resource_type(from fedml show-resource-type)

# Dataset-related:
fedml dataset

# Login-related:
fedml login
fedml logout

# Env-related:
fedml env
fedml network (changed from diagnosis )
fedml version