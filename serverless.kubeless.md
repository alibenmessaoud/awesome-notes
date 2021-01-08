

# kubeless

Kubeless is a Kubernetes-native serverless framework that lets you deploy small bits of code without having to worry about the underlying infrastructure plumbing. It leverages Kubernetes resources to provide auto-scaling, API routing, monitoring, troubleshooting and more.

*Github:* https://github.com/kubeless/kubeless

*Scenario:* https://www.katacoda.com/courses/serverless/getting-started-with-kubeless

### Install Kubeless and configure the Controller

```bash
master $ launch.sh
Waiting for Kubernetes to start...
Kubernetes started
master $ ##### Create a kubeless namespace where you will install the controller.
master $ kubectl create ns kubeless
namespace/kubeless created
master $ ##### Install the latest stable version with a kubectl create command
master $ #####  The controller which will watch for function objects to be created and also two Pods to enabled PubSub function.
master $ kubectl create -f https://github.com/kubeless/kubeless/releases/download/v1.0.0-alpha.8/kubeless-v1.0.0-alpha.8.yaml
configmap/kubeless-config created
deployment.apps/kubeless-controller-manager created
serviceaccount/controller-acct created
clusterrole.rbac.authorization.k8s.io/kubeless-controller-deployer created
clusterrolebinding.rbac.authorization.k8s.io/kubeless-controller-deployer created
customresourcedefinition.apiextensions.k8s.io/functions.kubeless.io created
customresourcedefinition.apiextensions.k8s.io/httptriggers.kubeless.io created
customresourcedefinition.apiextensions.k8s.io/cronjobtriggers.kubeless.io created
master $ kubectl get pods -n kubeless
NAME                                          READY     STATUS              RESTARTS   AGE
kubeless-controller-manager-66868fb689-lcx7m   0/3       ContainerCreating   0          2s
master $
```

### Deploy a Python Function

To deploy a function you use the `kubeless` CLI. You need to specify a *runtime*which specifies which language your function is in. You need to specify the file that contains your function, how the function will get triggered (here we use an HTTP trigger) and finally you specify the function name as a *handler*.

```bash
master $ ##### To deploy a function you use the kubeless CLI.
master $ kubeless function deploy toy --runtime python2.7 \
>                               --handler toy.handler \
>                               --from-file toy.py
INFO[0000] Deploying function...
INFO[0000] Function toy submitted for deployment
INFO[0000] Check the deployment status executing 'kubeless function ls toy'
master $ ##### You can then list the function with the kubeless CLI:
master $ kubeless function ls
NAME    NAMESPACE       HANDLER         RUNTIME         DEPENDENCIES    STATUS
toy     default         toy.handler     python2.7                       0/1 NOT READY
master $ cat toy.py
def handler(event, context):
   print event
   return event['data']
master $ You can check that a Pod containing your function is running:
master $ kubectl get pods
NAME                   READY     STATUS            RESTARTS   AGE
toy-86c8d54bd5-2jsk2   0/1       PodInitializing   0          5s
master $
```

### Call The Function

```bash
master $
master $ kubeless function call toy --data '{"hello":"world"}'
{"hello": "world"}
master $ kubectl proxy --port 8080 &
[1] 8129
master $ Starting to serve on 127.0.0.1:8080

master $ curl --data '{"hello":"world"}' localhost:8080/api/v1/namespaces/default/services/toy:8080/proxy/ --header "Content-Type:application/json"
{"hello": "world"}
master $
```

### View The Logs, Describe and Update a Function

```bash
master $ kubeless function describe toy
Name:           toy
Namespace:      default
Handler:        toy.handler
Runtime:        python2.7
Label:          {"created-by":"kubeless","function":"toy"}
Envvar:         null
Memory:         0
Dependencies:
master $
```

```bash
master $ kubeless function logs toy
Bottle v0.12.13 server starting up (using CherryPyServer())...
Listening on http://0.0.0.0:8080/
Hit Ctrl-C to quit.

10.32.0.1 - - [01/Mar/2019:14:52:14 +0000] "GET /healthz HTTP/1.1" 200 2 "" "kube-probe/1.11" 0/82
{'event-time': '2019-03-01 14:52:18.414860062 +0000 UTC', 'extensions': {'request': <LocalRequest: POST http://172.17.0.28:6443/>}, 'event-type': 'application/json', 'event-namespace': 'cli.kubeless.io', 'data': {u'hello': u'world'}, 'event-id': 'WyenX286FDwAOn0'}
10.32.0.1 - - [01/Mar/2019:14:52:18 +0000] "POST / HTTP/1.1" 200 18 "" "kubeless/v1.8.0+$Format:%h$ (linux/amd64) kubernetes/$Format" 0/11204
10.32.0.1 - - [01/Mar/2019:14:52:44 +0000] "GET /healthz HTTP/1.1" 200 2 "" "kube-probe/1.11" 0/128
{'event-time': None, 'extensions': {'request': <LocalRequest: POST http://localhost:8080/>}, 'event-type': None, 'event-namespace': None, 'data': {u'hello': u'world'}, 'event-id': None}
10.32.0.1 - - [01/Mar/2019:14:52:45 +0000] "POST / HTTP/1.1" 200 18 "" "curl/7.47.0" 0/8874
10.32.0.1 - - [01/Mar/2019:14:53:14 +0000] "GET /healthz HTTP/1.1" 200 2 "" "kube-probe/1.11" 0/91
```

```bash
master $ kubeless function update toy --from-file toy-update.py
INFO[0000] Redeploying function...
INFO[0000] Function toy submitted for deployment
INFO[0000] Check the deployment status executing 'kubeless function ls toy'
master $ cat toy-update.py
def handler(event, context):
   print "katacoda rocks"
   return {"katacoda":"rocks"}
master $ kubeless function call toy --data '{"hello":"world"}'
{"katacoda": "rocks"}
master $
```

### Create a Node Function

```bash
master $
master $ cat hello.js
module.exports = {
  handler: (event, context) => {
    console.log(event);
    return event.data;
  },
};
```

```bash
master $ kubeless function deploy hello --runtime nodejs6 \
>                               --handler hello.handler \
>                               --from-file hello.js
INFO[0000] Deploying function...
INFO[0000] Function hello submitted for deployment
INFO[0000] Check the deployment status executing 'kubeless function ls hello'
```

```bash
master $ kubeless function ls
NAME    NAMESPACE       HANDLER         RUNTIME         DEPENDENCIES    STATUS
hello   default         hello.handler   nodejs6                         0/1 NOT READY
toy     default         toy.handler     python2.7                       1/1 READY
master $ kubeless function call hello --data '{"kubeless":"rocks"}'
ERRO[0000] {"kind":"Status","apiVersion":"v1","metadata":{},"status":"Failure","message":"no endpoints available for service \"hello:http-function-port\"","reason":"ServiceUnavailable","code":503}

FATA[0000] an error on the server ("unknown") has prevented the request from succeeding
master $
```

### Delete Function

```bash
master $
master $ kubeless function delete toy
master $ kubeless function delete hello

master $ kubectl get deployments,services
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   14m
master $
```

# Bonus: go sls with aws, gcp and sps

**Same thing**: https://github.com/serverless/serverless 

```bash
npm install -g serverless
serverless login # `sls` is shorthand for the Serverless CLI command 
serverless create --template aws-nodejs --path time-service
cd time-service
serverless deploy -v
serverless deploy function -f hello # -f is shorthand for --function
serverless invoke -f hello -l
serverless logs -f hello -t
serverless remove

```

Tested https://github.com/serverless/examples/tree/master/google-node-simple-http-endpoint

Saw https://github.com/serverless/examples/tree/master/aws-node-heroku-postgres 

**About Riff:** Saw https://github.com/marios-code-path/intro-to-riff

Riff lets the developer write functions that respond to events. Functions are deployed as Kubernetes pods that contains a language-specific invoker for your custom function, and I/O bound sidecar for getting data in and out of the function scope.

Riff uses Custom Resource Definitions to enumerate functions and topics within kubernetes. In addition, it deploys a pair of controller pods to govern these resources — Topic and Function Controllers. The Topic Controller takes care of topic state changes with the underlying event broker. And the Function controller listens to topical events and manages function deployment, destruction, and scaling needs.

Riff employs an HTTP-gateway that lets you get data in and out of topics. Thus, there are 2 modes of communication: fire-and-forget => GET http://my-gateway-host:80/messages/name_of_topic; and request/response => GET http://my-gateway-host:80/requests/$name_of_topic