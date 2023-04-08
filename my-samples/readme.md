# My Samples

Create account :

```sh
kubectl apply -f my-samples/account.yaml
```

## View Accounts

All Account Users are able to view their Account through their generated ClusterRole. Let's try this by impersonating john:

### View your own accounts as regular account user

```sh
kubectl get accounts --as=masoud
```

## Allow Users To Create Spaces

By default, Account Users cannot create Spaces themselves. They can only use the Spaces/Namespaces that belong to their Accounts. That means a cluster admin would need to create the Spaces for an Account and then the Account Users could work with these Spaces/Namespaces.

To allow all Account Users to create Spaces for their own Accounts, create the following RBAC ClusterRoleBinding:

### Run this as cluster admin

```sh
kubectl apply -f my-samples/rbac-creator.yaml
```

## Create Spaces

After granting Account Users the right to create Spaces for their Accounts (see ClusterRoleBinding in 3.1.), all Account Users are able to create Spaces. Let's try this by impersonating john:

```sh
kubectl apply -f my-samples/space.yaml --as=masoud
```

## View Spaces

Let's take a look at the Spaces of the Accounts that User john owns by impersonating this user:

### List all Spaces as masoud

```sh
kubectl get spaces --as=john
```

### Get the defails of one of masoud's Spaces

```sh
kubectl get space masoud-space -o yaml --as=masoud
```


## Use Spaces

Every Space is the virtual representation of a regular Kubernetes Namespace. That means we can use the associated Namespace of our Spaces just like any other Namespace.

Let's impersonate john again and create an nginx deployment inside johns-space:

```sh
kubectl apply -n masoud-space --as=masoud -f my-samples/application/deployment.yaml
```

That's great, right? A user that did not have any access to the Kubernetes cluster, is now able to create Namespaces on-demand and gets restricted access to these Namespaces automatically.
