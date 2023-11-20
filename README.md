# Instruction

## Helm Chart
1. Open your teminal and run below commands.
   - kubectl create ns app-no-ops-ns
2. Update values in values.yaml.
   - Line 8: repository where you pushed the docker image from Task #2
   - Line 9: image tag
   - Line 26: namespace of serviceAccount i.e. app-no-ops-ns
3. Upload all files in myhelm folder to your repository.

## ArgoCD
1. Open app-no-ops-argocd.yaml, and update: 
   - Line 94: namespace i.e. app-no-ops-ns
   - Line 99: your repoURL where you pushed the docker image from Task #2
2. Open ArgoCD --> Login with your credential --> Applications --> NEW APP --> EDIT AS YAML.
3. Copy all information in app-no-ops-argocd.yaml and paste into EDIT AS YAML dialog, click at SAVE, and CREATE botton.
4. Wait until the Application status "Healthy", then run below command in your terminal.
    - export POD_NAME=$(kubectl get pods --namespace app-no-ops-ns -l "app.kubernetes.io/name=myhelm,app.kubernetes.io/instance=app-no-ops" -o jsonpath="{.items[0].metadata.name}")
    - export CONTAINER_PORT=$(kubectl get pod --namespace app-no-ops-ns $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
    - kubectl --namespace app-no-ops-ns port-forward $POD_NAME 8080:$CONTAINER_PORT
5. Open a browser and access to the application via 127.0.0.1:8080