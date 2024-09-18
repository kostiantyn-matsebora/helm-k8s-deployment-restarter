# k8s deployment restarter

Helm charts for restarting kubernetes deployment resource by schedule.

Supports two strategies:

* `restart` - using `kubectl` command `kubectl rollout restart deployment/daemonset`, supports deployments and daemonsets.
  
* `scaleDownUp` - scales down to zero and up to particular number of replicas, could be useful when destroying pod also requires release of some resources (for instance mounted volumes), supports deployments only.

## Configuration

```yaml
restart: 
  - name: deployments-to-restart # name of batch
    schedule: "7 */3 * * *" # cron schedule definition
    deployments: # list of deployments needs to be restarted
    - nginx-ingress
    - oauth2-proxy
    daemonSets: [] # list of daemonSets needs to be restarted

scaleDownUp: 
  - name: deployments-to-scaledownup # name of batch
    schedule: "7 */3 * * *" # cron schedule definition
    sleepPeriodInSeconds: 30 # sleep period in seconds between scaling down and scaling up
    deployments: # list of deployments needs to be restarted
    - name: nginx-ingress # name of deployment resource
      replicas: 1 # replicas number
    - name: oauth2-proxy
      replicas: 2
    
```
