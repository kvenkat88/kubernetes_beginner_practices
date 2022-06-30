## Debugging Pods
kubectl describe pods/po/pod ${POD_NAME}

kubectl get pods --selector=name=nginx,type=frontend

kubectl get pods -l app=hostnames

kubectl get pods -l app=hostnames \
    -o go-template='{{range .items}}{{.status.podIP}}{{"\n"}}{{end}}'

kubectl get pod termination-demo -o go-template="{{range .status.containerStatuses}}{{.lastState.terminated.message}}{{end}}"

kubectl get pod multi-container-pod -o go-template='{{range .status.containerStatuses}}{{printf "%s:\n%s\n\n" .name .lastState.terminated.message}}{{end}}'


## Access the previous container's crash log
kubectl logs --previous ${POD_NAME} ${CONTAINER_NAME}

## Multiple Containers available and want to navigate:
kubectl exec ${POD-NAME} -c ${CONTAINER-NAME} -- ${COMMAND}

## My pod is running but not doing what I told it to do:
1. Delete the pod and recreate it.
2. kubectl apply --validate -f mypod.yaml

## Pod on the apiserver matches the pod you meant to create:
kubectl get pods/mypod -o yaml > mypod-on-apiserver.yaml

## Debugging Replication Controllers: 
kubectl describe rc ${CONTROLLER_NAME}

## Debugging Services
kubectl describe service/services/svc ${SERVICE_NAME}

wget -O- hostnames

## Service work by DNS name
nslookup hostnames

nslookup hostnames.default

nslookup hostnames.default.svc.cluster.loca

## Endpoints for the service:
kubectl get endpoints ${SERVICE_NAME}

kubectl get endpoints hostnames

## kube-proxy working
ps auxw | grep kube-proxy

iptables-save | grep hostnames

ipvsadm -ln

Assuming you do see one the above cases, try again to access your Service by IP from one of your Nodes - curl 10.0.1.175:80

## Pod fails to reach itself via the Service IP
ps auxw | grep kubelet

for intf in /sys/devices/virtual/net/cbr0/brif/*; do cat $intf/hairpin_mode; done

ifconfig cbr0 |grep PROMISC

## Debugging a StatefulSet
kubectl get pods -l app=myapp

## List all events
kubectl get events

## Debugging with container exec
kubectl exec ${POD_NAME} -c ${CONTAINER_NAME} -- ${CMD} ${ARG1} ${ARG2} ... ${ARGN}

## Example debugging using ephemeral containers
kubectl debug -it ephemeral-demo --image=busybox:1.28 --target=ephemeral-demo

## Writing the root page for nginx
echo 'Hello shell demo' > /usr/share/nginx/html/index.html


## Resource metrics pipeline
kubectl top node/pod ${NAME}

