## Setup: Confirm Istio & Knative were Installed on your Cluster

Knative is currently built on top of both Kubernetes and Istio.
If you want to learn more about Kubernetes or Istio, you can check out the
labs [Kube101](https://github.com/IBM/kube101/tree/master/workshop) and
[Istio101](https://github.com/IBM/istio101/tree/master/workshop).
When you install Knative on IKS, it will install Istio for you
automatically. 

### Confirm Knative Installation

1. For this lab, Knative and Istio have been installed on your cluster already. You can explore the various namespaces and pods that were created as a result of this installation. Look at the Istio & Knative namespaces:

   ```
   kubectl get namespace
   ```

   and you should see something like:

   ```
   NAME                 STATUS   AGE
   default              Active   7d18h
   ibm-cert-store       Active   7d18h
   ibm-system           Active   7d18h
   istio-system         Active   7d17h
   knative-build        Active   7d17h
   knative-eventing     Active   7d17h
   knative-monitoring   Active   7d17h
   knative-serving      Active   7d17h
   knative-sources      Active   7d17h
   kube-public          Active   7d18h
   kube-system          Active   7d18h
   ```

   Notice the `istio-system` namespace, and the `knative-...` namespaces.

2. Once the namespaces are there, you can also check to see if all of the Istio and
   Knative pods are running correctly:

   ```
   kubectl get pods --namespace istio-system
   kubectl get pods --namespace knative-serving
   kubectl get pods --namespace knative-build
   ```

   You could check the pods in all of the Knative namespaces, but for this
   workshop only "serving" and "build" are required.

   Example Ouput:

   ```
   NAME                          READY   STATUS    RESTARTS   AGE
   activator-df78cb6f9-jpvs7     2/2     Running   0          38s
   activator-df78cb6f9-nhzhf     2/2     Running   0          37s
   activator-df78cb6f9-qjg8w     2/2     Running   0          37s
   autoscaler-6fccb66768-m4f2q   2/2     Running   0          37s
   controller-56cf5965f5-8pwcg   1/1     Running   0          35s
   webhook-5dcbf967cd-lxzmk      1/1     Running   0          35s
   ```

   If all of the pods shown are in a `Running` or `Completed` state then you should be all set.

Continue on to [exercise 3](../exercise-3/README.md).