---
description: Deploy Crossplane + Backstage + ArgoCD POC for AWS infrastructure
---

# Crossplane + Backstage + ArgoCD POC Deployment Workflow

## 🔄 Workflow Architecture

```
1. User điền form trong Backstage UI
         │
         ▼
2. Backstage Scaffolder xử lý template
         │
         ▼
3. fetch:template - Render template files
         │
         ▼
4. publish:github:pull-request - Tạo pull request claim resources

         │
         ▼
8. ArgoCD sync → Crossplane provision → AWS resources
```

## Prerequisites

- [ ] Kubernetes cluster running (`kubectl cluster-info`)
- [ ] Helm 3.x installed (`helm version`)
- [ ] AWS Access Key and Secret Key
- [ ] GitHub Personal Access Token (scopes: `repo`, `workflow`)
- [ ] NPM version >22.x
---

## Step 1: Install Crossplane

// turbo
```bash
helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update
kubectl create namespace crossplane-system --dry-run=client -o yaml | kubectl apply -f -
helm upgrade --install crossplane crossplane-stable/crossplane --namespace crossplane-system --wait
```

Verify:
```bash
kubectl get pods -n crossplane-system
```

## Step 2: Install AWS Providers

// turbo
```bash
 kubectl apply -f crossplane-backstage/1-crossplane/provider/aws-provider.yaml 
```

Wait for providers (takes 2-3 minutes):
```bash
kubectl get providers -w
```

## Step 3: Configure AWS Credentials

⚠️ **Replace with your actual AWS credentials:**

```bash
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: aws-creds
  namespace: crossplane-system
type: Opaque
stringData:
  credentials: |
    [default]
    aws_access_key_id = YOUR_ACCESS_KEY_HERE
    aws_secret_access_key = YOUR_SECRET_KEY_HERE
EOF
```

Apply ProviderConfig:
// turbo
```bash
kubectl apply -f d:/jenkins-self-lab/crossplane-backstage/1-crossplane/provider/providerconfig.yaml
```

## Step 4: Apply XRDs and Compositions

// turbo
```bash
kubectl apply -f d:/jenkins-self-lab/crossplane-backstage/1-crossplane/xrds/
kubectl apply -f d:/jenkins-self-lab/crossplane-backstage/1-crossplane/compositions/
```

Verify:
```bash
kubectl get xrd
kubectl get compositions
```

## Step 5: Install ArgoCD

// turbo
```bash
kubectl create namespace argocd --dry-run=client -o yaml | kubectl apply -f -
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
helm upgrade --install argocd argo/argo-cd --namespace argocd -f d:/jenkins-self-lab/crossplane-backstage/2-argocd/argocd-values.yaml --wait
```

Get admin password:
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```


Access: https://<IP-PUB>:31903(username: admin)

## Step 6: Generate ArgoCD Token

**⚠️ Important: Port-forward must be running before executing these commands!**

```bash


# Get admin password



# Login to ArgoCD
argocd login 52.221.211.56:31903 \
  --username admin \
  --password $(kubectl get secret -n argocd argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode) \
  --plaintext
# Generate token for Backstage
argocd account generate-token --account admin --id backstage
```

Save this token - you'll need it for GitHub secrets.

## Step 7: Configure GitHub Organization/Account Secrets

Go to GitHub → Settings → Secrets → Actions and add:

| Secret Name | Value |
|-------------|-------|
| `ARGOCD_SERVER` | Your ArgoCD server URL (e.g., `argocd.example.com` or `localhost:8080`) |
| `ARGOCD_AUTH_TOKEN` | Token from Step 6 |

## Step 8: Install Backstage



```bash
kubectl create namespace backstage --dry-run=client -o yaml | kubectl apply -f -
kubectl apply -f D:\backstage-self-lab\backstage-app\4.1-backstagek8s
```

Port-forward:
```bash
kubectl port-forward svc/backstage -n backstage 7007:7007
kubectl port-forward -n backstage svc/backstage 7007:80
```
Access: http://localhost:7007

## Step 9: Register Templates in Backstage

1. Open Backstage: http://localhost:7007
2. Go to **Create** menu
3. Click **Register Existing Component**
4. Enter template URL from your repo or local path

## Demo Flow

1. **Open Backstage** → Create → Select "AWS S3 Bucket"
2. **Fill form** → bucketName, region, environment, owner
3. **Submit** → Watch progress
4. **Check GitHub** → New pull request created with manifests
5. **Check ArgoCD** → New Application created and syncing
6. **Check Crossplane** → Claim created: `kubectl get s3buckets`
7. **Check AWS** → S3 bucket provisioned

## Verification Commands

```bash
# Crossplane
kubectl get providers
kubectl get xrd
kubectl get compositions
kubectl get s3buckets
kubectl get databases
kubectl get networks

# ArgoCD
kubectl get applications -n argocd

# Backstage
# Check catalog at http://localhost:7007/catalog
```

## Cleanup

```bash
bash d:/jenkins-self-lab/crossplane-backstage/scripts/cleanup.sh
```