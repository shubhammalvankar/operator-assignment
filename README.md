# operator-assignment

Steps to follow to create the application:

1. `sudo make docker-build docker-push IMG=<image_name:tag>`

2. `make install`

3. `make deploy IMG=<image_name:tag>`

    To check if the resources are created:<br />
    `kubectl get deployment -n operator-assignment-system`<br />
    `kubectl get pods -n operator-assignment-system`

    To check the logs of ansible tasks:<br />
    `kubectl logs -f -n operator-assignment-system deployment/operator-assignment-controller-manager -c manager`<br />

4. To create the mysql CR<br />
`kubectl apply -f config/samples/cache_v1alpha1_appdb.yaml`<br />

    Wait for the ansible tasks to finish executing <br />
    Check the logs of mysql pod and check if it displays 'waiting for connections' <br />

5. To create the nodejs CR<br />
`kubectl apply -f config/samples/cache_v1alpha1_app.yaml`<br />

    Again wait for the ansible tasks to finish executing <br />
    Check the logs of nodejs pod and make sure it is connected to the database and the table is created <br />

    Get all the resources<br />
    `kubectl get configmap,secret,pvc,all`<br />

6. Now start the service using minikube <br />
`minikube service nodejs-app`<br />

7. To delete the CRs<br />
`kubectl delete -f config/samples/cache_v1alpha1_app.yaml` <br />
`kubectl delete -f config/samples/cache_v1alpha1_appdb.yaml` <br />

8. To remove the application <br />
`make undeploy`
