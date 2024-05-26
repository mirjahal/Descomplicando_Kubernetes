# Node Affinity 

#### Warning  FailedScheduling  4m6s   default-scheduler  0/4 nodes are available: 1 node(s) had untolerated taint {node-role.kubernetes.io/control-plane: }, 3 node(s) didn't find available persistent volumes to bind. preemption: 0/4 nodes are available: 4 Preemption is not helpful for scheduling.

Note: For most volume types, you do not need to set this field. You need to explicitly set this for local volumes.
https://kubernetes.io/docs/concepts/storage/persistent-volumes/#node-affinity