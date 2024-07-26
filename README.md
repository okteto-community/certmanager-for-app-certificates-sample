# go-getting-started-certificate
This is an example of how to add Ingress Objects with TLS and CertManager creating certificate for the host


## Test sample

We added some folder samples to add for your specific scenario that require to be changed specific if you want to use it. 
Right now the Chart of the Application is created without that options in order to work from start when deploying it. 


### Okteto Configuration

To test it you need to add change your Okteto Chart Helm configuration as it is in `okteto-chart-config`

```yaml
namespace:
  ingress:
    annotations:
      cert-manager.io/cluster-issuer: "myissuer"
```

This will ensure that all the ingresses created by Okteto will have that annotation that Cert Manager needs to know to create the certificates. 


### Cert Manager
And create the Issuer for Cert Manager that will create for each ingress in the Namespace the host. 
An example is shown in the folder `certmanager`


### Application Configuration

And finally you should parametrize the name of the host with the name of the Namespace where it is being deployed. 

There is an example in the folder `app-config`.

To do so, add to your Helm values configuration the option of the Namespace and use the Environment variable that Okteto provides: OKTETO_NAMESPACE

In this case you have to remove the annotation to auto generate host from Okteto

```yaml
annotations:
    dev.okteto.com/generate-host: hello-world
```

To be able to add the TLS section to the ingress. You need to specify here to Cert Manager the name it will create for that domain. 


```yaml
- secretName: mydomain
    hosts:
      - mydomain.dev.okteto.net
```

