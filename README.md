# Testing local-path-storage working
- Create a yaml file with name `pod.yaml`:
    ```yaml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: task-pv-claim
    spec:
      storageClassName: local-path
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
        storage: 1Gi
    ---
    apiVersion: v1
    kind: Pod
    metadata:
      name: task-pv-pod
    spec:
      volumes:
        - name: task-pv-storage
        persistentVolumeClaim:
          claimName: task-pv-claim
      containers:
        - name: task-pv-container
        image: nginx
        ports:
          - containerPort: 80
          name: "http-server"
        volumeMounts:
          - mountPath: "/usr/share/nginx/html"
          name: task-pv-storage
    ```
- Apply to your cluster
  ```
  kubectl apply -f pod.yaml
  ```
- Check the result
  ```
  $ kubectl get pod                     
  NAME          READY   STATUS    RESTARTS   AGE
  task-pv-pod   1/1     Running   0          128m

  $ kubectl get pvc
  kubectl get pvc                             
  NAME            STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
  task-pv-claim   Bound    pvc-713f53e7-918c-458a-a158-f2df362c2bd5   1Gi        RWO            local-path     129m
  ```
- The result mean: the PVC was bound and create by `local-path` storageclass and the POD is running mount PVC as normarlly

# Install helm (local)
- Document https://helm.sh/docs/intro/install/#from-the-binary-releases

# Install cert-manager
- Connect to correct cluster before action
    ```bash
    export KUBECONFIG=/path/to/kubeconfig
    ```

- Add helm repo
    ```bash
    helm repo add jetstack https://charts.jetstack.io
    ```
- Update repo
    ```bash
    helm repo update
    ```
- Install cert-manager
    ```bash
    helm upgrade --install \
    cert-manager jetstack/cert-manager \
    --namespace cert-manager \
    --create-namespace \
    --version v1.11.0 \
    --set installCRDs=true
    ```
- Create a ClusterIssuer for production mode, `cluster-issuer.yaml`:
    ```yaml
    apiVersion: cert-manager.io/v1
    kind: ClusterIssuer
    metadata:
      name: letsencrypt-prod
    spec:
      acme:
        server: https://acme-v02.api.letsencrypt.org/directory
        email: your-email@example.com
        privateKeySecretRef:
          name: letsencrypt-prod-private-key
        solvers:
        - http01:
            ingress:
                class: nginx
    ```
- Apply this file to your cluster
    ```bash
    kubectl apply -f cluster-issuer.yaml
    ```
# Install ingress-nginx

- Add helm repo and update
    ```bash
    helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
    helm repo update
    ```
- Create file `values-custom.yaml`:
    ```yaml
    controller:
      kind: DaemonSet
      service:
        type: ClusterIP
      hostNetwork: true
    ```
- Install ingress-nginx
    ```bash
    helm upgrade --install \
    ingress-nginx ingress-nginx/ingress-nginx \
    --namespace ingress-nginx \
    --create-namespace \
    --version 4.6.1 \
    -f values-custom.yaml \
    --wait --atomic --timeout 15m0s
    ```