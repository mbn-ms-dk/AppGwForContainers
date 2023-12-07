# SSL offloading with Application Gateway for Containers

Application Gateway for Containers enables SSL offloading for better backend performance. See the following example scenario:

![SSL offloading](./ssl-offloading.png)

Deploy sample application

```bash
kubectl apply -f ssl-offloading.yaml
```

This command creates the following on your cluster:

* a namespace called test-ssl
* one service called echo in the test-ssl namespace
* one deployment called echo in the test-ssl namespace
* one secret called listener-tls-secret in the test-ssl namespace

## Deploy the required Gateway API resources

```bash
kubectl apply -f ssl-offloading-gateway.yaml
```

Verify the status of the gateway resource, ensure the status is valid, the listener is Programmed, and an address is assigned to the gateway

```bash
kubectl get gateway gateway-01 -n test-ssl -o yaml
```

create a HTTPRoute

```bash
kubectl apply -f - ssl-offloading-httproute.yaml
```

Once the HTTPRoute resource has been created, ensure the route has been Accepted and the Application Gateway for Containers resource has been Programmed.

```bash
kubectl get httproute https-route -n test-ssl -o yaml
```

Test access to the application

```bash
fqdn=$(kubectl get gateway gateway-01 -n test-ssl -o jsonpath='{.status.addresses[0].value}')
curl --insecure https://$fqdn/
```

## Clean up

```bash
kubectl delete -f ssl-offloading.yaml
```
