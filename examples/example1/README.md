# Complete list what needs to be done

I used this sites to set up my "test" site:
https://opensource.com/article/20/3/kubernetes-traefik
and
https://github.com/Thoorium/kubernetes-local-cluster-flannel-metallb-traefik


# Prerequisites:
You have cluster :)
HELM installed - I do now did some time ago so you have to figure it out

# Installation

## MetalLB

kubectl apply -f https://raw.githubusercontent.com/google/metallb/v0.8.1/manifests/metallb.yaml
```
save in file: metallb-config.yaml
Set up IP somthing that your router can assign it through DHCP
```
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 192.168.0.50-192.168.0.99 # Change the range here - this is not my IPs :)
```
Run Command:
```
kubectl apply -f metallb-config.yaml
```

## Traefik
https://doc.traefik.io/traefik/getting-started/install-traefik/

Something like this(will do install again to verify):
```
helm repo add traefik https://helm.traefik.io/traefik
helm repo update
helm install traefik traefik/traefik
```

### Test it
```
kubectl port-forward $(kubectl get pods --selector "app.kubernetes.io/name=traefik" --output=name) 9000:9000
```

### Simple Website
Create index.html file
```
<html>
<head><title>K3S!</title>
  <style>
    html {
      font-size: 62.5%;
    }
    body {
      font-family: sans-serif;
      background-color: midnightblue;
      color: white;
      display: flex;
      flex-direction: column;
      justify-content: center;
      height: 100vh;
    }
    div {
      text-align: center;
      font-size: 8rem;
      text-shadow: 3px 3px 4px dimgrey;
    }
  </style>
</head>
<body>
  <div>Hello from K3S!</div>
</body>
</html>
```

Run:
```
kubectl create configmap mysite-html --from-file index.html
```

Create mysite.yaml file:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysite-nginx
  labels:
    app: mysite-nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysite-nginx
  template:
    metadata:
      labels:
        app: mysite-nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
        volumeMounts:
        - name: html-volume
          mountPath: /usr/share/nginx/html
      volumes:
      - name: html-volume
        configMap:
          name: mysite-html
```

Run:
```
kubectl apply -f mysite.yaml
```

Get service:
```
kubectl get service
```
You should get somthing like:
```
traefik                LoadBalancer   100.65.163.132   <YOUR FANCY IP>    80:31369/TCP,443:31622/TCP   1d
```

Point it to <YOUR IP>:80 in your browser and enjoy your site, which I have absolutely no idea how it works 


