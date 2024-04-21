# kubectl attach

Attach to a process that is running inside an existing container.
https://kubernetes.io/docs/reference/kubectl/generated/kubectl_attach/

```
kubectl attach (POD | TYPE/NAME) -c CONTAINER [options]
```

Examples:
```
# Get output from running pod mypod; use the 'kubectl.kubernetes.io/default-container' annotation
# for selecting the container to be attached or the first container in the pod will be chosen
kubectl attach mypod

# Get output from ruby-container from pod mypod
kubectl attach mypod -c ruby-container

# Switch to raw terminal mode; sends stdin to 'bash' in ruby-container from pod mypod
# and sends stdout/stderr from 'bash' back to the client
kubectl attach mypod -c ruby-container -i -t

# Get output from the first pod of a replica set named nginx
kubectl attach rs/nginx
Options
```

# kubectl exec

Execute a command in a container.
https://kubernetes.io/docs/reference/kubectl/generated/kubectl_exec/

```
kubectl exec (POD | TYPE/NAME) [-c CONTAINER] [flags] -- COMMAND [args...]
```

Examples:
```
# Get output from running the 'date' command from pod mypod, using the first container by default
kubectl exec mypod -- date

# Get output from running the 'date' command in ruby-container from pod mypod
kubectl exec mypod -c ruby-container -- date

# Switch to raw terminal mode; sends stdin to 'bash' in ruby-container from pod mypod
# and sends stdout/stderr from 'bash' back to the client
kubectl exec mypod -c ruby-container -i -t -- bash -il

# List contents of /usr from the first container of pod mypod and sort by modification time
# If the command you want to execute in the pod has any flags in common (e.g. -i),
# you must use two dashes (--) to separate your command's flags/arguments
# Also note, do not surround your command and its flags/arguments with quotes
# unless that is how you would execute it normally (i.e., do ls -t /usr, not "ls -t /usr")
kubectl exec mypod -i -t -- ls -t /usr

# Get output from running 'date' command from the first pod of the deployment mydeployment, using the first container by default
kubectl exec deploy/mydeployment -- date

# Get output from running 'date' command from the first pod of the service myservice, using the first container by default
kubectl exec svc/myservice -- date
```