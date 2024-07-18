## These functions manage the creation and updates of Kubernetes Secrets for TLS certificates and CA certificates used by the Capten system.

1. createOrUpdateSecret:

This reusable function takes a Kubernetes client and a Secret object as arguments.
It attempts to get the Secret with the provided name and namespace using the Kubernetes API.
If the Secret doesn't exist (k8serrors.IsNotFound(err)), it creates a new Secret using the provided object and the Create method of the client.
If the Secret exists, it updates the existing Secret with the provided object using the Update method.
Any errors encountered during these operations are wrapped with additional context using errors.WithMessage and returned.
2. CreateOrUpdateCertSecrets:

This function is responsible for creating or updating multiple certificate-related secrets.
It retrieves the kubeconfig path from the captenConfig and uses GetK8SClient to create a Kubernetes client for interacting with the API server.
It then calls separate functions to create or update three specific secrets:
createOrUpdateAgentCertSecret: Handles the agent certificate and CA bundle.
createOrUpdateAgentCACert: Handles the CA certificate bundle for the agent.
createOrUpdateClusterCAIssuerSecret: Handles the intermediate CA certificate and key for the Cluster Issuer used by Cert Manager.
3. createOrUpdateAgentCertSecret, createOrUpdateAgentCACert & createOrUpdateClusterCAIssuerSecret:

These functions follow a similar pattern:
They read certificate and key data from files specified in the captenConfig paths using os.ReadFile.
They create a corev1.Secret object with the following details:
ObjectMeta: Defines the secret name and namespace based on the provided configuration.
Data: A map containing the certificate and key data as byte arrays. The specific keys used for storing the data depend on the secret type.
Type: Sets the type of the secret (e.g., corev1.SecretTypeTLS for TLS secrets, corev1.SecretTypeOpaque for opaque data).

## Finally, they call createOrUpdateSecret to handle creating or updating the secret in the Kubernetes API server.
Overall, these functions provide a structured approach to managing certificate secrets in Kubernetes for the Capten system. They leverage a reusable createOrUpdateSecret function and separate functions for handling specific types of certificate secrets.