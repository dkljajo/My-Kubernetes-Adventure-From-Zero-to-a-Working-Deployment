# My-Kubernetes-Adventure-From-Zero-to-a-Working-Deployment

I recently embarked on a journey to deploy a simple Nginx application on Kubernetes, thinking it would be straightforward. Little did I know, it would turn into a full-on troubleshooting saga that tested my patience, persistence, and problem-solving skills.

It all began with creating a Deployment YAML. My initial attempt threw validation errors — apparently, my labels didn’t match between the Deployment selector and Pod template. After carefully correcting the metadata and ensuring the selector and Pod labels aligned, my Pods finally started running. I could see them spinning up with kubectl get pods, all three happily in the Running state. Victory? Not quite.

Next came the Service. I had assumed my Deployment alone would expose Nginx to the outside world. Spoiler: it didn’t. kubectl get svc showed only the internal ClusterIP, and trying to access the application in the browser led nowhere. This was when I realized I needed a NodePort to open access externally. I carefully wrote a Service YAML, specifying the correct port, targetPort, and nodePort.

Even after that, things weren’t smooth. My first attempt to access the NodePort failed — the firewall was blocking the port. I had to inspect UFW rules, allow the NodePort through, and reload the firewall. Then, at last, I could reach Nginx in the browser! Every single Pod was serving traffic, and my NodePort was responding perfectly.

The process took multiple rounds of trial and error, applying YAMLs, checking logs, verifying services, and adjusting firewall rules. I captured 10 screenshots along the way — each one telling a story of a small victory, a mistake corrected, or a concept learned. By the end, not only was my Nginx deployment fully functional, but I also gained a deeper understanding of Kubernetes deployments, services, ports, and the intricacies of networking.

This exercise taught me that Kubernetes isn’t just about YAML files and commands — it’s about understanding the underlying system, thinking like both a developer and a network administrator, and methodically solving problems. The journey from “it doesn’t work” to “it’s live and accessible” was challenging but immensely satisfying.

![](./images/1.png)
