# My Kubernetes Adventure: From Zero to a Working Deployment

This project documents my journey deploying an Nginx application on Kubernetes. I faced errors, troubleshooting, and firewall issues, but finally made it fully functional. Along the way, I captured **10 screenshots** to visualize each step.

---

## Table of Contents

1. [Introduction](#introduction)
2. [Deployment Creation](#deployment-creation)
3. [Troubleshooting Errors](#troubleshooting-errors)
4. [Service Exposure](#service-exposure)
5. [Firewall & NodePort](#firewall--nodeport)
6. [Conclusion & Screenshots](#conclusion--screenshots)

---

## Introduction

Deploying Kubernetes can be challenging. Matching labels, correct YAML structure, and network accessibility are all critical. This guide shows my journey from “it doesn’t work” to “live and accessible.”

---

## Deployment Creation

I created the Nginx Deployment with correct labels, replicas, and container ports:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels:
    app: nginx
    tier: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

| Deployment Applied                                     | Pods Running                                             |
| ------------------------------------------------------ | -------------------------------------------------------- |
| ![Deployment Applied](./images/1.png) | ![Pods Running](./images/2.png) |

*Pods successfully created, 3 replicas running.*

---

## Troubleshooting Errors

I encountered YAML validation errors due to mismatched labels and misconfigured ports. Fixing the Deployment allowed Pods to reach `Running` status.

| Error Example                                           | Fixed Deployment                                            |
| ------------------------------------------------------- | ----------------------------------------------------------- |
| ![Validation Error](./images/4.png) | ![Corrected Deployment](./images/5.png) |

---

## Service Exposure

To access Nginx externally, I created a NodePort Service:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30080
```

| Service Created                                     | Cluster & Pods Info                                    |
| --------------------------------------------------- | ------------------------------------------------------ |
| ![Service Created](./images/11.png) | ![Service Details](./images/20.png) |

---

## Firewall & NodePort

The NodePort was initially blocked by UFW. After opening the port:

```bash
sudo ufw allow 30080/tcp
sudo ufw status
```

I could access the application at `http://<node-ip>:30080`.

| NodePort Accessible                                     | Web Page Served                                     |
| ------------------------------------------------------- | --------------------------------------------------- |
| ![NodePort Access](./images/31.png) | ![Nginx Web Page](./images/50.png) |

---

## Conclusion & Screenshots

This journey taught me:

- Deployment and Pod labels must match  
- NodePort access requires firewall configuration  
- Kubernetes troubleshooting is iterative and rewarding

| Pod Details                                    | `kubectl get all`                                      |
| ---------------------------------------------- | ------------------------------------------------------ |
| ![Pod Details](./images/60.png) | ![kubectl get all](.images/80.png) |


---

**Enjoy exploring Kubernetes!**
