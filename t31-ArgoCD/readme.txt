kubectl patch service/argocd-server  -n argocd -p '{"spec": {"type": "NodePort"}}' 
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath='{ .data.password }' | base64 -d
