---
description: Expose Applications using Kubernetes Services
---

# Exposing Applications

## Internal Services

Internal, cluster local services should be configured as regular `ClusterIP` services.

## Public Services

Exposing services to the Internet is done by deploying a `LoadBalancer` service type with an annotation to allocate a public IP for the service.

{% code title="sshd-public-service.yaml" %}
```yaml
apiVersion: v1
kind: Service
metadata:
  annotations:
    metallb.universe.tf/address-pool: public
    metallb.universe.tf/allow-shared-ip: default
  name: sshd
spec:
  type: LoadBalancer
  externalTrafficPolicy: Local
  ports:
    - name: sshd
      port: 22
      protocol: TCP
      targetPort: sshd
  selector:
    app.kubernetes.io/name: sshd
```
{% endcode %}

{% hint style="info" %}
For most public services, ensure that `externalTrafficPolicy: Local` is set on the service. This ensures that ingress traffic from the interent is load balanced directly to nodes running the  application.
{% endhint %}


