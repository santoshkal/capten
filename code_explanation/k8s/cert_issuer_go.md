## Detailed Code Explanation for CreateOrUpdateClusterIssuer function
This function manages the creation or update of a Cluster Issuer resource in a k8s cluster using the Cert Manager API. It relies on the config.CaptenConfig struct for configuration details.

Steps:

Load Kubeconfig:

It retrieves the Kubeconfig file path from captenConfig.
It uses the clientcmd.BuildConfigFromFlags function (with empty flags) and the Kubeconfig path to create a k8s client configuration object.
If there's an error building the configuration, it returns an error with a message.
Create Cert Manager Client:

It uses the created k8s client configuration to create a new Cert Manager client (cmClient).
If there's an error creating the client, it returns the error.
Create Cluster Issuer Object:

It defines a certmanagerv1.ClusterIssuer object with the following details:
Object metadata:
Name: Set to the value of captenConfig.ClusterCAIssuer.
Spec:
IssuerConfig:
CA: This defines a Certificate Authority (CA) issuer.
SecretName: Set to the value of captenConfig.ClusterCACertSecretName which references the secret containing the CA certificate and key.
Get Existing Cluster Issuer (if any):

It attempts to retrieve a Cluster Issuer resource with the same name (issuer.Name) using the Cert Manager client.
If the resource is not found (k8serrors.IsNotFound(err)) it indicates a new issuer needs to be created.
Create Cluster Issuer (if not found):

If the issuer doesn't exist, it creates a new Cluster Issuer resource using the cmClient.CertmanagerV1().ClusterIssuers().Create method.
If there's an error creating the issuer, it returns an error with a message.
If successful, it logs a debug message indicating the creation of the Cluster Issuer.
Update Existing Cluster Issuer (if found):

If a Cluster Issuer resource with the same name is found, it updates the existing resource with the desired configuration.
It modifies the retrieved serverIssuer object's Spec.IssuerConfig.CA.SecretName to match the value from captenConfig.ClusterCACertSecretName.
It uses the issuerClient.Update method to update the Cluster Issuer resource with the modified configuration.
If there's an error updating the issuer, it returns an error with a message.
If successful, it logs a debug message indicating the update of the Cluster Issuer.
Overall, this function ensures a Cluster Issuer resource exists with the specified configuration, creating it if necessary or updating the existing one.