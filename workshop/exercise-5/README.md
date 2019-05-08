## Deploy vnext Version and Apply Traffic Shifting

Did you notice that the Fibonacci sequence started with 1? Most would argue that the sequence should actually start with 0. There's a vnext version of the application at the vnext branch in the github project that starts the sequence with 0 instead of 1. This container image has been built and deployed to dockerhub, and tagged as vnext. We'll deploy that as v2 of our app.

### Deploy vnext
1. Let's deploy vnext, again using a docker image on dockerhub. Maybe we want to slowly roll users over from our old version to the new version, or do some A/B testing of the new version to see what users like better. Let's see what the yaml for this looks like.

    ```
    cat fib-service2.yaml
    ```

    Expected Output:
    ```
    apiVersion: serving.knative.dev/v1alpha1
    kind: Service
    metadata:
    name: fib-knative
    namespace: default
    spec:
    release:
        revisions: ["fib-knative-xxxxx", "@latest"]
        rolloutPercent: 10
        configuration:
        revisionTemplate:
            spec:
            container:
                image: docker.io/ibmcom/fib-knative:vnext
    ```

	Notice that we've added a rolloutPercent of 10. We've also added our old revision name to the revisions array. Your revision name will be different, so you will need to update this file.

2. Get the name of your first revision.

    ```
    kubectl get revision
    ```

3. Edit the `fib-service2.yaml` file to change `fib-knative-xxxxx` to your own revision name. To edit the file you need to click the pencil icon, edit the file at `fib-knative/fib-service2.yaml` and then save the file. After updating `fib-knative-xxxxx` to your own value, return to the cloudshell.

    ![](../README_images/pencil.png)

4. Apply your new configuration to the cluster.

    ```
    kubectl apply -f fib-service2.yaml
    ```

5. Let's run some load against the app, just asking for the first number in the Fibonacci sequence so that we can clearly see which revision is being called.

	```
	while sleep 0.5; do curl "$MY_DOMAIN/1"; done
	```

6. We should see that the curl requests are routed approximately 90/10 between the two revisions. Let's kill this process using `ctrl + c`.


Congratulations! You've completed the lab!  If you have tons of time left over, and are interested in diving deeper, we've included 2 advanced exercises you could complete.

Continue on to [exercise 6](../exercise-6/README.md).
