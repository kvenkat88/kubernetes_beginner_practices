## Apply Command
- To create all objects/Update objects, except those that already exist, defined by configuration files in a specified directory

  kubectl apply -f|-R ${filname.yaml} -ns ${namespace_name}
  
  Where -R - recursively process directories and namepace may be optional and by default we will have default namespace.

## Diff command
-  Print the object that will be created

   kubectl apply -f|-R ${filname.yaml}

## Get Command and View Live Configuration

    kubectl get -f|-R ${filname.yaml} -o yaml

## Scale the deployment replicas
    Write directly to the live configuration without modifying the configuration file.

    kubectl scale deployment ${deployment_name} --replicas=2

## Delete Command
   
    kubectl delete -f ${file.yaml}

## Create Object commands
  - run, expose and autoscale

## Update object commands
  - scale, annotate, label, set, edit and patch

## Delete Objects
  - delete

## View an Object
  - get, describe and logs

## Set Command
  - set and create to specify a value for the field before object creation.  This is done by piping the output of the create command to the set command, and then back to the create command.

    kubectl create service clusterip my-svc --clusterip="None" -o yaml --dry-run=client | kubectl set selector --local -f - 'environment=qa' -o yaml | kubectl create -f -

    - The kubectl create service -o yaml --dry-run=client command creates the configuration for the Service, but prints it to stdout as YAML instead of sending it to the Kubernetes API server.

    - The kubectl set selector --local -f - -o yaml command reads the configuration from stdin, and writes the updated configuration to stdout as YAML.

    - The kubectl create -f - command creates the object using the configuration provided via stdin.

## Managing Secrets using kubectl
   
    echo -n 'admin' > ./username.txt
    echo -n '1f2d1e2e67df' > ./password.txt

    echo -n 'my-app' | base64
    
    echo -n '39528$vdg7Jb' | base64

    kubectl create secret generic db-user-pass --from-file=./username.txt --from-file=./password.txt

    kubectl create secret generic db-user-pass --from-file=username=./username.txt --from-file=password=./password.txt

    kubectl create secret generic db-user-pass --from-literal=username=devuser --from-literal=password='S!B\*d$zDsb='

    kubectl get secret db-user-pass -o jsonpath='{.data}'

    kubectl get secret db-user-pass -o jsonpath='{.data.password}' | base64 --decode

    kubectl delete secret db-user-pass

## Create and Set Cluster
    kubectl config --kubeconfig=config-demo set-cluster development --server=https://127.0.0.1:52739 --certificate-authority=$HOME/.minikube/ca.crt
    
    kubectl config --kubeconfig=config-demo set-cluster scratch --server=https://127.0.0.1:52739 --insecure-skip-tls-verify

    kubectl config --kubeconfig=config-demo set-credentials developer --client-certificate=$HOME/.minikube/profiles/minikube/client.crt --client-key=$HOME/.minikube/profiles/minikube/client.key

    kubectl config --kubeconfig=config-demo set-credentials experimenter --username=exp --password=test@123

## Port Forward
    
    kubectl port-forward mongo-75f59d57f4-4nd6q 28015:27017

## Filter
    https://cloud.redhat.com/blog/customizing-oc-output-with-go-templates

    https://gist.github.com/so0k/42313dbb3b547a0f51a547bb968696ba


    kubectl get pods -o go-template='{{.apiVersion}}{{"\n"}}'
    
    ### Get Specific index item
    kubectl get pods -o go-template='{{(index .items 0).metadata.name}}{{"\n"}}'

    ### Get all items by range like for loop
    kubectl get pods -o go-template='{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}'
    
    ### Get all items by range like for loop using Go template --template
     kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}'

    ### With multiple range
    kubectl get pods -o go-template --template '{{range .items}}{{range .spec.containers}}{{.name}}{{"\n"}}{{end}}{{end}}'

     kubectl get pods -o go-template --template '{{range .items}}{{range .spec.containers}}{{range .volumeMounts}}{{.mountPath}}{{"\n"}}{{end}}{{end}}{{end}}'


    ### using jsonpath - retrieve multiple items
    kubectl get pods -o jsonpath='{.items[*].apiVersion}'
    kubectl get pods -o jsonpath='{.items[0].metadata.name}{"\n"}'


    ### Using jsonpath specific item retrieval
    kubectl get pods -o jsonpath='{.items[*].apiVersion}' # retrieve first item

    kubectl get pods -o jsonpath='{.items[0].apiVersion}'

    kubectl get pods -o jsonpath='{.items[0].spec.containers[0].volumeMounts[0].mountPath}'

     ### Using jsonpath multiple item retrieval

    kubectl get pods -o jsonpath='{range .items[*]}{.metadata.name}{"\n"}'

     kubectl get pods -o jsonpath='{.items[*].metadata.name}{"\t"}.items[*].metadata.name}{"\n"}'

