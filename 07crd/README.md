# 사용자 정의 리소스

```yaml
# mypod-crd.yaml
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  name: mypods.crd.example.com
spec:
  group: crd.example.com
  version: v1
  scope: Namespaced
  names:
    plural: mypods   # 복수 이름
    singular: mypod  # 단수 이름
    kind: MyPod      # Kind 이름
    shortNames:      # 축약 이름
    - mp
```

```bash
# crd 생성
kubectl apply -f mypod-crd.yaml
# customresourcedefinition.apiextensions.k8s.io/mypods.crd.example.com created

# crd 리소스 확인
kubectl get crd | grep mypods
# NAME                     CREATED AT          
# mypods.crd.example.com   2020-06-14T09:33:32Z          
```

```bash
# MyPod 리소스 생성
cat << EOF | kubectl apply -f -
apiVersion: "crd.example.com/v1"
kind: MyPod
metadata:
  name: mypod-test
spec:
  podName: "hello"
  command: "echo hello"
  image: nginx
EOF
# mypod.crd.example.com/mypod-test created

kubectl get mypod
# NAME         AGE
# mypod-test   3s

# 축약형인, mp로도 조회가 가능합니다.
kubectl get mp
# NAME         AGE
# mypod-test   3s

# MyPod의 상세 정보를 조회합니다.
kubectl get mypod mypod-test -oyaml
# apiVersion: crd.example.com/v1
# kind: MyPod
# metadata:
#   ...
#   name: mypod-test
#   namespace: default
#   resourceVersion: "723476"
#   selfLink: /apis/crd.example.com/v1/namespaces/default/mypods/mypod-test
#   uid: 50dd0cc8-0c1a-4f43-854b-a9c212e2046d
# spec:
#   podName: "hello"
#   command: "echo hello"
#   image: nginx

# MyPod를 삭제합니다.
kubectl delete mp mypod-test
# mypod.crd.example.com "mypod-test" deleted
```

### Custom Controller

```python
import requests
import sys 
import json

server = sys.argv[1]
token = sys.argv[2]

headers = {"Authorization": "Bearer %s" % token}


def add_pod(name):
    pod_spec = { 
      "apiVersion": "v1",
      "kind": "Pod",
      "metadata": {
         "name" : name
      },  
      "spec": {
        "containers": [{
           "image": "nginx",
           "name": "mynginx"
          }   
        ]   
      }   
    }   
    r = requests.post("https://{server}/api/v1/namespaces/default/pods".format(server=server), headers=headers, verify=False, json=pod_spec)
    print(r.text)


def delete_pod(name):
    r = requests.delete("https://{server}/api/v1/namespaces/default/pods/{name}".format(server=server, name=name), headers=headers, verify=False)
    print(r.text)


def run_controller():
    r = requests.get("https://{server}/apis/crd.example.com/v1/namespaces/default/mypods?watch=true".format(server=server), headers=headers, verify=False, stream=True)
    for line in r.iter_lines():
        j = json.loads(line)
        if j['type'] == 'ADDED':
            name = j['object']['spec']['podName']
            add_pod(name)
            print('add pod name: {name}'.format(name=name))
        elif j['type'] == 'DELETED':
            name = j['object']['spec']['podName']
            delete_pod(name)
            print('delete pod name: {name}'.format(name=name))


run_controller()
```
