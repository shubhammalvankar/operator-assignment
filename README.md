# operator-assignment

Steps to follow to create the application:

    1: sudo make docker-build docker-push IMG=<image_name:tag>

    2: make install

    3: make deploy IMG=<image_name:tag>

--
Check if the resources are created:

    kubectl get deployment -n operator-assignment-system
    kubectl get pods -n operator-assignment-system
--
Check the logs of ansible tasks:

    kubectl logs -f -n operator-assignment-system deployment/operator-assignment-controller-manager -c manager
--

Create the mysql CR

    4: kubectl apply -f config/samples/cache_v1alpha1_appdb.yaml

--
Wait for the ansible tasks to finish executing 
Check the logs of mysql pod and check if it displays 'waiting for connections'
--

Create the nodejs CR

    5: kubectl apply -f config/samples/cache_v1alpha1_app.yaml

--
Again wait for the ansible tasks to finish executing 
Check the logs of nodejs pod and make sure it is connected to the database and the table is created
--
Get all the resources

    kubectl get configmap,secret,pvc,all
--

Now start the service using minikube

    6: minikube service nodejs-app

--

TO delete the CRs
    
    7: kubectl delete -f config/samples/cache_v1alpha1_app.yaml

    8: kubectl delete -f config/samples/cache_v1alpha1_appdb.yaml

--
To remove the application

    9: make undeploy