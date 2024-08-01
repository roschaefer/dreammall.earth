# Helmfile

You need the following tools installed on your machine:
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [helmfile](https://helmfile.readthedocs.io/en/latest/) 
- [sops](https://github.com/getsops/sops)
- [age](https://github.com/FiloSottile/age)

Not required, but highly recommended:
- [k9s](https://k9scli.io/)

## Setup

Append your public `age` public key to [sops.yaml](../../.sops.yaml).
Ask another developer who can already read sops [secret files](./secrets) to re-encrypt these files your public key.

## Deployment

Relevant files that need to be adapted are:
- [helmfile](./helmfile.yaml)
- [secret files](./secrets)
- [default values](./dreammall/values.yaml)

Check the deployment with:
```sh
helmfile diff
```

If you are happy with what you see, you can do:

```sh
helmfile apply
```

Observe the pods starting up:
```sh
kubectl get pods -w
```

Or you can use:
```sh
k9s
```

After a while your release will be up and running.

### TLS Certificate

Make sure your DNS records are in place. Then check your certificates:
```sh
kubectl describe -n dreammall certificate letsencrypt-tls
```

If your certificate "is up to date and has not expired" you can change `letsencrypt-staging` to `letsencrypt-prod` in:
```sh
kubectl edit -n dreammall ingress dreammall-master
kubectl edit -n dreammall ingress authentik-server
```

You can do all of these above much more comfortably with `k9s`.
