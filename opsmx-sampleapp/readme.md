# This repository has a sample app to showcase canary deployments using Argo

#### Deploy  the Rollout, Services, and Ingress

After applying the deployment yamls, you should  notice a second ingress created by the rollouts controller, rollouts-demo-rollouts-demo-stable-canary. This ingress is the "canary ingress", which is a clone of the user-managed Ingress referenced under nginx.stableIngress. It is used by nginx ingress controller to achieve canary traffic splitting. The name of the generated ingress is formulated using <ROLLOUT-NAME>-<INGRESS-NAME>-canary.

#### Perform An Update

Update the image from blue to yellow
kubectl argo rollouts set image rollouts-demo rollouts-demo=argoproj/rollouts-demo:yellow
kubectl argo rollouts get rollout rollouts-demo

At this point, both the canary and stable version of the Rollout are running, with 5% of the traffic directed to the canary. One thing to note, is that the rollout is able to achieve a 5% canary weight despite only running two pods. This is able to be achieved since the traffic split happens at the ingress controller (as opposed to weighted replica counts and kube-proxy in the basic guide).
