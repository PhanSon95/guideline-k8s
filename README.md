# Add your public key into all servers
- SSH into server
  ```
  ssh root@38.242.213.229
  ssh root@173.249.60.41
  ```
- Copy your public key (`~/.ssh/id_rsa.pub`) at local, and paste content to `~/.ssh/authorized_keys` and make sure you can ssh to 2 servers without password

# Install Ansible
- Reference this link to install: https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible

# Install k8s cluster
- Go to `kubespray` folder
  ```
  cd kubespray/
  ```
- Edit file [hosts.yaml](kubespray/inventory/local/hosts.yaml) and make sure list servers was defined correct
- Run playbook to install (this command can take 30m or longer)
  ```
  ansible-playbook -i inventory/local/hosts.yaml cluster.yml --become --become-user=root -vvv
  ```
- After command execute successed without error, ssh to the master node and get kubeconfig file
  ```
  ssh root@38.242.213.229
  cat /etc/kubernetes/admin.conf
  ```
- Here is example `admin.conf` file:
  ```yaml
  apiVersion: v1
  clusters:
  - cluster:
      certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUMvakNDQWVhZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJek1EVXhPVEEzTlRFME4xb1hEVE16TURVeE5qQTNOVEUwTjFvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTmxNCkxrQTN5Vm9kbUFoQU0xQ0k2R0VKRVp2YjRkTFdhaU9KRWZjYzIzNDRLSE1zdlk5bkR4VWNla0lvclc5d2kza2kKNlZOY1h6Mlg2RTRuSVcwZkpaNW5zVE9JaS9taWZMS2g5WlEwUlZWR0MrbmNBR3hucjRGVTAvTGlTSFZSbGRmSwo4SXMzT2d2bWYxNGwvSzltcklERnRLclpnVzZPQTNXbHl1dFlzRWRoL21DcnQ5eDdtTUQ0WWN1K3FaZmFnSGxXCmtycWdZcVZ4QU14VENCUTJPMWFuakMvU2FySS9UVzcyQ2hENlhBNGZzVnFNbjB0dnRUem44UnMwUldZNWpSTDcKdTNxNlhlNGFUb255MDZ1WFZGdVh5THNDaFdXU3o5MUNKMnNSVjZlbjh1UldUTG5lWUtXd2dxSUZERENVYW5pZQphNlYyQTlGSVdQVjVHTmNUM1JzQ0F3RUFBYU5aTUZjd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0hRWURWUjBPQkJZRUZFZ2JmNG14Q1JTRG1GUXBoYVVmbDBndzcydytNQlVHQTFVZEVRUU8KTUF5Q0NtdDFZbVZ5Ym1WMFpYTXdEUVlKS29aSWh2Y05BUUVMQlFBRGdnRUJBR0hHSWl0SWlSYklSdlN2ek5XcwpOVVBqNjUreXdjVmNSQ0t6V21OTXIzL2RCQzlZdUh2ekJZU29WU3FyWkZwQUZYNzYveXNLSU1WQWI0aGRGRWYvClRGNWNaa2FKZ044cE80b0s2MjAxK2RtOXVpVFdVV3Y1bzUrWFdzR2JnU0JkVnpxaVJOUGtiZTBXaitVaWJ5QmIKdkQ2cVhlUVA5dDVBM2tNNXlQVkJGMGUyT3pBTnpOQjdLRDJFU2cwbXlISmhJMlJiUU5lcGcrejhCYXRITHE2dApWWEhUVUJWWkJUa1VCcVlpczA5NmU2dmhiNUVtZ2svaG1aOTB0a1UyZVJvb1FUOG5VN2FvaTlQbWtOMUxXaHorCnJkeXFOc3AyT1FsZ0hIczRJS0ZMUnZTUisxSEhuTlVweG12L25PNjliN2VmV1lCRzZ5RHBxK2dSSVFVWmVJUzQKM0FjPQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
      server: https://127.0.0.1:6443
    name: cluster.local
  contexts:
  - context:
      cluster: cluster.local
      user: kubernetes-admin
    name: kubernetes-admin@cluster.local
  current-context: kubernetes-admin@cluster.local
  kind: Config
  preferences: {}
  users:
  - name: kubernetes-admin
    user:
      client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURJVENDQWdtZ0F3SUJBZ0lJTTlOUlJ5elUyN2t3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TXpBMU1Ua3dOelV4TkRkYUZ3MHlOREExTVRnd056VXhORGhhTURReApGekFWQmdOVkJBb1REbk41YzNSbGJUcHRZWE4wWlhKek1Sa3dGd1lEVlFRREV4QnJkV0psY201bGRHVnpMV0ZrCmJXbHVNSUlCSWpBTkJna3Foa2lHOXcwQkFRRUZBQU9DQVE4QU1JSUJDZ0tDQVFFQXFaYUV2NElOS1YxVW9LUHEKWGlhWmd6dHNlWk0wUW44cndZdDFPeG9kOWxMV0FMc0lEUDlFQllXSlZDYW9jalluWHdJc2ZkTE9oYndXUHBhNwpJZFJWMmV6aENiRzQ0Q29HR0p3R1FJaUMwYmtSQkxkeG56b0tYMC9jSFpEMXNldkN3L3EyZm81ZUJNRU0rWnpWCkZFblZHZzRDU01kUlBDczJicU5oV1RVM3g2cTdQbURSY24rYzBzL25XOUllL0pQWFhYUVNHQXJ5ZFRRSWtSK1AKVFpBZVRUbTJMSlRkNHdXek5pWlZ2SXV0RVVBRTFHejdFTUdNd00zQ1FrQklHVm93NktWYTJuWHpBQ0VKcE5qWQpMK092eWhucVFqbDhuZ1h2K2hFbUNFRmhNbUxnWFYyL3ltbnVpVlBIUVN4Tm5Xd2FBV0RkVDJ3UVo5MzUxQ2thClhkWktOd0lEQVFBQm8xWXdWREFPQmdOVkhROEJBZjhFQkFNQ0JhQXdFd1lEVlIwbEJBd3dDZ1lJS3dZQkJRVUgKQXdJd0RBWURWUjBUQVFIL0JBSXdBREFmQmdOVkhTTUVHREFXZ0JSSUczK0pzUWtVZzVoVUtZV2xINWRJTU85cwpQakFOQmdrcWhraUc5dzBCQVFzRkFBT0NBUUVBcDJBRVM3OEZSbjFsamRUOGN5OHJRRVVLY3ZJWWlYZmdTKytaCktDQmVjOWZveHlYQzF3eG9Oa1ozNFNNV0lHR2x3dFYvYVFWU0x4REVFdTVBZ0h2RzNxU1VNY2Q2SzdKYjdseEYKTFJIUDZEdGU0Vlc4Z2xPK2IycDBmQ3J6Zk1BWSttanhmTUEzZVVjNHBDODNVdlVUMUZ3eEVxN3l0dFJHdGwwNwp6Q2VFK1pqeGVrcGlSV0lFMzlFWG83YkRNeFg3OTAzTnZKUnRTVERGa2dVSkZUbkVJbkpCd0k3ZW9RVnBORTk2CkhabVVYV2dNYnNSRDhDdDYrdWRiUGgyYXUyaEJuTnpKcllIRTdoUVdzZzdJRmlDN3dLS0ZXdC9LUEJQMktsOHcKSEkvajBZQTNZQ3JTWm5oTjVNMDNkeGJlclhPajNuYU1OREF0Q3gzSkNmcmN2WEl4M0E9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
      client-key-data: LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFb3dJQkFBS0NBUUVBcVphRXY0SU5LVjFVb0tQcVhpYVpnenRzZVpNMFFuOHJ3WXQxT3hvZDlsTFdBTHNJCkRQOUVCWVdKVkNhb2NqWW5Yd0lzZmRMT2hid1dQcGE3SWRSVjJlemhDYkc0NENvR0dKd0dRSWlDMGJrUkJMZHgKbnpvS1gwL2NIWkQxc2V2Q3cvcTJmbzVlQk1FTStaelZGRW5WR2c0Q1NNZFJQQ3MyYnFOaFdUVTN4NnE3UG1EUgpjbitjMHMvblc5SWUvSlBYWFhRU0dBcnlkVFFJa1IrUFRaQWVUVG0yTEpUZDR3V3pOaVpWdkl1dEVVQUUxR3o3CkVNR013TTNDUWtCSUdWb3c2S1ZhMm5YekFDRUpwTmpZTCtPdnlobnFRamw4bmdYditoRW1DRUZoTW1MZ1hWMi8KeW1udWlWUEhRU3hObld3YUFXRGRUMndRWjkzNTFDa2FYZFpLTndJREFRQUJBb0lCQUMwanAwbmlMK2FtdFBQZApMWERLRFdwazBzYTVhOXVYUmVwS1dIWFd5Y3JhMmFOd0pRQndvWVptdU5yeFB6ekhOVXVRcEk4SklYZHZUT3h6CjZuTml0VWRBU2RYdXZDck9oTGpnTHJuV25CdCtpdzBhVnQvdTd2dlhvZHNzem5rQksrSkdSWFVDSi94VDlrUXEKZm56YitpRHBRYXBsZ3pYa3VxUlFVSVVTS1RxM3hCR3JScnBJdU0rdXJFTG1RQXpycm9TNGNOazZtd25mTmh5YQp5Z29wK3RWaUxjNmRPSWxKOVdxbFpsNEJKOERlUnUyYWxxNEV0bnBtanZtZnVOVFFpWVJpQmovRkEydXJWZlB3Ckw5N3prSlNiLzBxOEVOMXQxdWltTlVkRTNCZFEwQjYwb1dJS3JWSWNzT1ZlbWN6UU90alp3c1NjSTI4YkpUWWYKZUQ1LytVRUNnWUVBMWVEK2tvajhBekJpMTQ3K0xNUEJUTlVJRzJUVE44Zys5MW1PRjJHNjZpUXBLM3YwTHVWcgpFOG9BRzhyREduVEc2WHRZNDZGTnVsdm91RnE4ZlB2dHFTbTM2cjZudjNiTkw5cjhhVGpvbUg2b2dNT3NkWnYrCmNkUUJ0UGozU3dIZVUvRTZYRDZieTR1K1d5M1ZGUHJLNWplQUxvVXBGek9obkZ5TTdTQmV5WThDZ1lFQXl2eUkKN0xUT3dJeE5paHkrVE8zT0ZvZ1Z0UXZzdERZY2RPVmZQVU9JbFRSU0VMSDkrVVZOSjZsR2ptenlrOTk3S2hWOQpkU3g3Y2lLNDM5NzJvekNLYnlMWVlrbUxsaFpaS3pnU2dEeGdBYitiSnBldlVMZERBd09HN2s3ektJQ0QrRVBxCmtGcjIwNTBiUnhBZWxmcnlWV2kzZXI3RnBNMHMrd0ZRdGJHVGtOa0NnWUJFWHJaQ2RaUEtHUytmcE5CUDB3djUKMmFmTHlnRlROYW9iTy8rV0xlR29jNExOcWNRM01aTjNNZjJ5NUxCVnhyckhiR3pQNTRLSGJsWUlpTWowVXp4Mwo5U1FiNEhLVFhCRGZtV3RTMzZSTEMwSW1WMmJMK2QvdTEwenNZeUg2VWxDV0dPa0ZEK2FSdk14UExXbU9XVlYvCkhvTFZYQy8wZ01iS3l6TXBvazFxdFFLQmdHVmRYS2NQMTF0UjlQcXVSZDIydHo2TE1JUGJjNVcrbTlGSUpabzcKV2o1Z0JVY3ZDMHZxYnBBS3VTQXdpYWFNYUQzb3cyQzdHTjR6TnEwYzgzOGtvMHpDVXRvcUxkbUNTWDhLbmpxSgphOHdUdWMrNDJhUnVEN20yamkvOUh2SXYwemNyK3p1aElUY2xjbFMzV1A2K2RUdHZjb2lLTWJxTmR4UFZZVStLCllNMXBBb0dCQUtFVmNyZlI1TlJKdExjTWhUaTBLTVNhdWRpNjlMZU9BaXpER0h3aFRLOHFEUXZ6RTgzbTEvbVMKWVZDKzdIN3hBbk9lWURqclpzems3Z1hzQm02c3AzOFVSTWxiTzJJTENYbnRxZ0JtYWlqcVY5VDVxRUdHRjVGYwpzRG9VTHF1dm4xK3RWalBGemFoRXRCdjBQaVZZdVlKRS9obUhFaHR5cDJ3ZUpWZitsOE5VCi0tLS0tRU5EIFJTQSBQUklWQVRFIEtFWS0tLS0tCg==
  ```
- Change the IP `127.0.0.1` to your master IP, example is `38.242.213.229`
- Save this file to your local at `~/.kube/kubeconfig`

# Connect to your cluster

- Install `kubectl`, guide install [here](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)
  ```
  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

  ```
- Connect
  ```
  export KUBECONFIG=~/.kube/kubeconfig
  ```
- Verify connection is OK:
  ```
  $ kubectl get node
  NAME   STATUS   ROLES           AGE   VERSION
  k8s1   Ready    control-plane   8h    v1.25.6
  k8s2   Ready    control-plane   8h    v1.25.6
  ```
# Install helm (local)
- Document https://helm.sh/docs/intro/install/#from-the-binary-releases
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