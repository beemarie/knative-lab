## Deploy vnext Version Using knctl

Did you notice that the Fibonacci sequence started with 1? Most would argue that the sequence should actually start with 0. There's a vnext version of the application at the vnext branch in the github project. This container has been built and deployed to dockerhub, and tagged as vnext. We'll deploy that as v2 of our app.

### Deploy vnext
1. Let's deploy vnext, again using a docker image on dockerhub. Maybe we want to slowly roll users over from our old version to the new version, or do some A/B testing of the new version. We can update the Route section of our `.yaml` file to do so. Let's look at our new `.yaml`

    ```
    cat fib-service0.yaml
    ```

    Expected Output:
    ```
    apiVersion: serving.knative.dev/v1alpha1
    kind: Configuration
    metadata:
    name: fib-knative-0
    namespace: default
    spec:
    revisionTemplate:
        spec:
        container:
            image: docker.io/ibmcom/fib-knative:vnext
    ---
    apiVersion: serving.knative.dev/v1alpha1
    kind: Route
    metadata:
    name: fib-knative
    namespace: default 
    spec:
    traffic:
    - configurationName: fib-knative-0
        percent: 50
    - configurationName: fib-knative-1
        percent: 50
    ```

	Notice that we're defining a new Configuration, this time with a name of `fib-knative-0`, since the sequence will start with 0. This configuration references the `vnext` version of our image on dockerhub. In the `Route` block, we're using this configuration name, and our previous configuration name of `fib-knative-1` to route different traffic percentages to each revision.

2. Let's deploy this app into our cluster. Apply the `fib-service0.yaml` file.

    ```
    kubectl apply --filename fib-service0.yaml
    ```

3. Let's run some load against the app, just asking for the first number in the Fibonacci sequence so that we can clearly see which revision is being called.

	```
	while sleep 0.5; do curl "$MY_DOMAIN/1"; done
	```

4. We should see that the curl requests are routed approximately 50/50 between the two applications. Let's kill this process using `ctrl + c`.


Continue on to [exercise 6](../exercise-6/README.md).
