# openstack-using-argo

## Few additional configmap and secret is required for external CEPH

~~~
root@master:~# cat ceph-admin-keyring.yaml
apiVersion: v1
kind: Secret
metadata:
  name: "pvc-ceph-client-key"
  namespace: openstack
type: kubernetes.io/rbd
data:
  key: QVFEazMvSmZNZzBCT3hBQTJCZVV5RmZyZTdTVXgzS1hZN01qb2c9PQ==
  
  ~~~
  
  ~~~
  root@master:~# cat cindar-configmp.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: ceph-etc
  namespace: openstack
data:
  ceph.conf: |
    [global]
    mon_host = 192.168.101.166:6789,192.168.101.169:6789,192.168.101.165:6789
  ~~~
  
  ~~~
  root@master:~# cat rabbit-svc.yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app.kubernetes.io/instance: rabbitmq
    manager: argocd-application-controller
  name: rabbitmq-nodeport-svc
  namespace: openstack
spec:
  ports:
  - name: http
    port: 15672
    protocol: TCP
    targetPort: 15672
  selector:
    application: rabbitmq
    component: server
    release_group: rabbitmq
  sessionAffinity: None
  type: NodePort

  ~~~
