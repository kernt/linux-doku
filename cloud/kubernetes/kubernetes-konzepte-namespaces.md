---
title: kubernetes-konzepte-namespaces
description: 
published: true
date: 2021-06-09T15:33:30.339Z
tags: 
editor: markdown
dateCreated: 2021-06-09T15:33:24.696Z
---

# Kubernates Konzept: namespaces

Der Name einer Ressource ist eine eindeutige Kennung innerhalb eines Namespaces im Kubernetes-Cluster. Die Verwendung eines Kubernetes-Namespaces kann Namespaces für verschiedene Umgebungen im selben Cluster isolieren.
Es gibt Ihnen die Flexibilität, eine isolierte Umgebung zu schaffen und Ressourcen auf verschiedene Projekte und Teams zu verteilen.

Pods, Services, replication controller sind in einem bestimmten Namespace enthalten. Einige Ressourcen wie Nodes und PVs gehören nicht zu einem Namespace.

## Fertig werden

Standardmäßig hat Kubernetes einen Namensraum namens `default` erstellt. Alle Objekte, die ohne Angabe von Namespaces erstellt wurden, werden in den default Namespaces gesetzt. Sie können kubectl verwenden, um Namespaces aufzulisten:

```sh
// check all namespaces
# kubectl get namespaces
NAME      LABELS    STATUS    AGE
default   <none>    Active    8d
```

Kubernetes wird auch einen weiteren ersten Namespace namens kube-System für die Lokalisierung von Kubernetes Systemobjekten, wie eine Kubernetes UI pod.

Der Name eines Namespaces muss ein DNS-Label sein und den folgenden Regeln folgen:

* Höchstens 63 Zeichen
* Passender regex [a-z0-9]([-a-z0-9]*[a-z0-9])

### Wie es geht…

1. Nachdem wir unseren gewünschten Namen ausgewählt haben, erstellen wir einen Namensraum namens `new-namespace` mit der Konfigurationsdatei:

```sh
# cat newNamespace.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: new-namespace

// create the resource by kubectl
# kubectl create -f newNamespace.yaml
```

2.Nachdem der Namespace erfolgreich erstellt wurde, listen Sie den Namespace erneut auf:

```sh
// list namespaces
# kubectl get namespaces
NAME            LABELS    STATUS    AGE
default         <none>    Active    8d
new-namespace   <none>    Active    12m
```

Wir können jetzt sehen, dass wir zwei Namespaces haben.

3.Lass uns den nginx Replikations controller ausführen, der in [Richte dein eigenes kubernetes ein](../kubernetes-einrichten) in einem neuen Namespace beschrieben ist:

```sh
// run a nginx RC in namespace=new-namespace
# kubectl run nginx --image=nginx --namespace=new-namespace

```

4.Dann lass uns die pods auflisten:

```sh
# kubectl get pods
NAME                                    READY     STATUS        RESTARTS   AGE
```

5.Es laufen keine pods ! Lass uns wieder mit dem Parameter `--namespace` nachsehen:

```sh
// to list pods in all namespaces
# kubectl get pods --all-namespaces
NAMESPACE      NAME          READY     STATUS    RESTARTS   AGE
new-namespace  nginx-ns0ig   1/1       Running   0          17m

// to get pods from new-namespace
# kubectl get pods --namespace=new-namespace
NAME          READY     STATUS    RESTARTS   AGE
nginx-ns0ig   1/1       Running   0          18m
```

Wir können jetzt unsere pods sehen.

Wenn Sie in der Befehlszeile kein Namensraum angeben, wird Kubernetes die Ressourcen im Standard-Namespace erstellen. Wenn du Ressourcen nach der Konfigurationsdatei erstellen möchtest, kannst du es einfach bei der Erstellung von `kubectl create` angeben:

```sh
# kubectl create -f myResource.yaml --namespace=new-namespace
```

### Ändern des Standard-Namespaces

Es ist möglich, den Standard-Namespace in Kubernetes umzuschalten:

1. Finden Sie Ihren aktuellen Kontext:

```sh
# kubectl config view | grep current-context
current-context: ""
```

Es zeigt, dass wir jetzt keine Kontexteinstellung haben.

2.Egal ob es sich um einen aktuellen Kontext handelt oder nicht, mit `set-context` könnte wir ein neues erstellen oder das bestehende überschreiben:
`# kubectl config set-context <current context or new context name> --namespace=new-namespace`

3.Nachdem Sie den Kontext mit einem neuen Namespace gesetzt haben, können wir die aktuelle Konfiguration überprüfen:

```sh
# kubectl config view
apiVersion: v1
clusters: []
contexts:
- context:
    cluster: ""
    namespace: new-namespace
    user: ""
  name: new-context
current-context: ""
kind: Config
preferences: {}
users: []
```

Wir können sehen, dass der Namespace im Kontextbereich richtig eingestellt ist.

4.Wechseln Sie den Kontext zu dem, den wir gerade erstellt haben:
`# kubectl config use-context new-context`

5.Überprüfen Sie dann den aktuellen Kontext erneut:

```sh
# kubectl config view | grep current-context
current-context: new-context
```

Wir können sehen, dass aktueller Kontext jetzt Neukontext ist.

6.Lass uns die aktuellen Pods wieder auflisten. Es ist nicht nötig, den Namespace-Parameter anzugeben, da wir die Pods im `new-namespace` auflisten können:

```sh
# kubectl get pods
NAME          READY     STATUS    RESTARTS   AGE
nginx-ns0ig   1/1       Running   0          54m
```

7.Der Namespace ist auch in der Pod-Beschreibung aufgeführt:

```sh
# kubectl describe pod nginx-ns0ig
Name:        nginx-ns0ig
Namespace:      new-namespace
Image(s):      nginx
Node:        ip-10-96-219-156/10.96.219.156
Start Time:      Sun, 20 Dec 2015 15:03:40 +0000
Labels:        run=nginx
Status:        Running
```

### Löschen eines Namensraums

1.Mit `kubectl delete` können Sie die Ressourcen einschließlich des Namespaces löschen. Wenn Sie einen Namespace löschen, werden alle Ressourcen unter diesem Namespace gelöscht:

```sh
# kubectl delete namespaces new-namespace
namespace "new-namespace" deleted

```

2.Nachdem der Namespace gelöscht wurde, ist unser nginx pod weg:

```sh
# kubectl get pods
NAME      READY     STATUS    RESTARTS   AGE
```

3.Der Standard-Namespace im Kontext ist jedoch immer noch als `new-namespace` gesetzt:

```sh
# kubectl config view | grep current-context
current-context: new-context

```

Wird es ein Problem sein?

4.Lass uns einen `nginx` replication controller  wieder laufen.

```sh
# kubectl run nginx --image=nginx
Error from server: namespaces "new-namespace" not found
```

Es wird versuchen, eine `nginx` Replikation Controller und Replik Pod in den aktuellen Namespace, die wir gerade gelöscht zu erstellen. Kubernetes wird einen Fehler auslösen, wenn der Namespace nicht gefunden wird.

5.Wechseln wir zum Standard-Namespace.

```sh
# kubectl config set-context new-context --namespace=""
context "new-context" set.
```

6.Jetzt können wir `nginx` wieder laufen lassen.

```sh
# kubectl run nginx --image=nginx
replicationcontroller "nginx" created
Does it real run in default namespace? Let's describe the pod.
# kubectl describe pods nginx-ymqeh
Name:        nginx-ymqeh
Namespace:      default
Image(s):      nginx
Node:        ip-10-96-219-156/10.96.219.156
Start Time:      Sun, 20 Dec 2015 16:13:33 +0000
Labels:        run=nginx
Status:        Running
...
```

Wir können sehen, dass der Pod derzeit im Namespace: default läuft. Alles sieht gut aus

### Es gibt mehr…

Manchmal müssen Sie die Ressourcen quota für jedes Team einschränken, indem Sie den Namespace unterscheiden. Nachdem du einen neuen Namespace erstellt hast, sehen die Details wie folgt aus:

```sh
$ kubectl describe namespaces new-namespace
Name:  new-namespace
Labels:  <none>
Status:  Active

No resource quota.

No resource limits.
```

Ressourcen quota und Limits sind nicht standardmäßig festgelegt. Kubernetes unterstützt die Einschränkung für einen Container oder Pod. `LimitRanger` im Kubernetes API Server muss aktiviert werden, bevor die Constraint gesetzt wird. Sie können entweder eine Befehlszeile oder eine Konfigurationsdatei verwenden, um es zu aktivieren:

```sh
// using command line-
# kube-apiserver --admission-control=LimitRanger

// using configuration file
# cat /etc/kubernetes/apiserver
...
# default admission control policies
KUBE_ADMISSION_CONTROL="--admission_control=NamespaceLifecycle,NamespaceExists,LimitRanger,SecurityContextDeny,ResourceQuota"
...
```

Das folgende ist ein gutes Beispiel für das Erstellen einer Grenze in einem Namespace.

Wir werden dann die Ressourcen in einem Pod mit den Werten `2` als `max` und `200m` als `min` für `cpu` und `1Gi` als `max` und `6Mi` als `min` für `memory` beschränken.
Für den Container ist die `cpu` zwischen `100m` - `2` begrenzt und der Speicher liegt zwischen `3Mi` - `1Gi`. Wenn das `max` gesetzt ist, müssen Sie bei der Ressourcenerstellung das Limit in der Pod / Container-Spezifikation angeben. Wenn die `min` gesetzt ist, muss die anforderung während der pod / container-anstellung angegeben werden. Der `default`- und `defaultRequest`-Abschnitt in `LimitRange` wird verwendet, um die Standardgrenze und die Anforderung in der Containerspezifikation anzugeben:

```sh
# cat limits.yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: limits
  namespace: new-namespace
spec:
  limits:
  - max:
      cpu: "2"
      memory: 1Gi
    min:
      cpu: 200m
      memory: 6Mi
    type: Pod
  - default:
      cpu: 300m
      memory: 200Mi
    defaultRequest:
      cpu: 200m
      memory: 100Mi
    max:
      cpu: "2"
      memory: 1Gi
    min:
      cpu: 100m
      memory: 3Mi
    type: Container

// create LimitRange
# kubectl create -f limits.yaml
limitrange "limits" created
```

Nachdem das `LimitRange` erstellt wurde, können wir diese wie bei jeder anderen Ressource auflisten:

```sh
// list LimitRange
# kubectl get LimitRange --namespace=new-namespace
NAME      AGE
limits    22m
```

Wenn du den neuen Namespace beschreibst hast, kannst du nun die Einschränkung sehen:

```sh
# kubectl describe namespace new-namespace
Name:  new-namespace
Labels:  <none>
Status:  Active

No resource quota.

Resource Limits
 Type      Resource  Min   Max   Request  Limit   Limit/Request
 ----      --------  ---     ---    -------  -----   -------------
 Pod      memory    6Mi      1Gi  -         -      -
 Pod      cpu       200m   2     -         -      -
 Container  cpu       100m   2     200m      300m   -
 Container  memory    3Mi      1Gi  100Mi      200Mi   -
```

Alle in diesem Namensraum erstellten Pods und Container müssen den hier aufgeführten Ressourcen Grenzen folgen. Wenn die Definitionen gegen die Regel verstoßen, wird ein Validierungsfehler entsprechend ausgeworfen.

### LimitRange löschen

Wir können die `LimitRange` Ressource über:
`# kubectl delete LimitRange <limit name> --namespace=<namespace>`

Hier ist der Grenzname `limits` und der Namespace ist `new-namespace`. Danach, wenn du den Namespace beschreibst, ist die Einschränkung weg:

```sh
# kubectl describe namespace <namespace>
Name:  new-namespace
Labels:  <none>
Status:  Active

No resource quota.

No resource limits.
```