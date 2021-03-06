### JSONPath Support
```
kubectl get service -n kube-system  -o=custom-columns=NAME:.metadata.name,IP:.spec.clusterIP,PORT:.spec.ports[*].targetPort #Get POD name and IP and target Port of Service`
kubectl get pods -o custom-columns='DATA:spec.containers[*].image' # Get Container Image
kubectl get pods -o custom-columns='DATA:spec.containers[?(@.image!="nginx")].image' # filter the nginx image of Container
kubectl get pods -o json  # print json format output for pod
kubectl get pods -o=jsonpath='{@}'  #print all element of Current position
kubectl get pods -o=jsonpath='{.items[0]}' # print the 1st Items
kubectl get pods -o=jsonpath='{.items[0].metadata.name}'  #print the pod name of 1st object element of items
kubectl get pods -o=jsonpath="{.items[*]['metadata.name', 'status.capacity']}"
kubectl get pods -o=jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.startTime}{"\n"}{end}'
kubectl get pods -o=jsonpath="{range .items[*]}{.metadata.name}{'\t'}{.status.startTime}{'\n'}{end}"
kubectl get pods -o=jsonpath="{range .items[*]}{.metadata.name}{\"\t\"}{.status.startTime}{\"\n\"}{end}"

kubectl get pods -o=jsonpath='{.items[?(@.metadata.labels.app=="webapp")].metadata.name}'
kubectl get pods -l app=webapp -o=jsonpath='{.items..metadata.name}'
kubectl get pods -o=jsonpath=‘{.items[*].status.phase}’ --all-namespaces # get all POD status only
kubectl get po -o custom-columns='POD:metadata.name,NodeName:spec.nodeName,IMAGE:..image,STATUS:status.phase,CONTAINERPORT:spec.containers[*].ports[*].containerPort' # POD dtatus
kubectl get pods --field-selector=status.phase!=Running --all-namespaces # Gettign ALL POD not runing
kubectl get pods -o jsonpath='{.items[?(@.status.phase!="Running")].metadata.name}'  # get POD name which are not running
kubectl get pods -o jsonpath='{.items[?(@.status.phase!="Running")].metadata.name}' |xarg kubectl delete pod # delete POD which are not running
kubectl get pod --output='jsonpath={range .items[*]}{.metadata.name}{"\n"}{end}' # Get POD name in new line
kubectl get pod --template='{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}'  # Go tamplate get the POD name
kubectl  get pod  --output='go-template={{ range .items}}{{.metadata.name}}{{"\n"}}{{end}}' # get POD name with Go template
kubectl get nodes --sort-by=".status.conditions[?(@.reason == 'KubeletReady' )].lastTransitionTime" #sort by last transaction time
kubectl get services --sort-by=.metadata.name # sorted by name
kubectl get pods --sort-by='.status.containerStatuses[0].restartCount' # sorted by restart count
kubectl get pv --sort-by=.spec.capacity.storage # List PersistentVolumes sorted by capacity
kubectl get pods --selector=app=cassandra -o jsonpath='{.items[*].metadata.labels.version}' # Get the version label of all pods with label app=cassandra
kubectl get pods --field-selector=status.phase=Running #Get ExternalIPs of all nodes
kubectl get po --no-headers -n $i |awk -F" " '{ if ( $4 >0 ) print $1}' #Get Pod name which are restart count !=0

kubectl get po -A --no-headers  |awk -F" " '{ if ( $5 >0 ) print $1"/"$2 }' # get namespace/pod name if the restart count more than 0

kubectl get nodes -o json|jq ".items[]|{name:.metadata.name} +.status.capacity"
kubectl get po nginx1 -o jsonpath='{.metadata.annotations}{"\n"}'
kubectl get pods --field-selector=status.phase!=Running -A --no-headers |awk -F" " '{print $1"/"$2"/"$4}'   # print the pod if pod not running
kubectl get po -A -o jsonpath='{range .items[?(@.status.containerStatuses[0].restartCount>0)]}{.metadata.namespace} {"\t"}{.metadata.name}{"\n"}{end}'

```
