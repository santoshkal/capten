This document explains the functionalities of the provided Go code snippets focusing on the helm package. It interacts with the config.CaptenConfig struct and utilizes Helm libraries for managing Helm charts on a k3s cluster.

1. Client & NewClient

Client: This struct holds the Helm environment settings (Settings), a default timeout (defaultTimeout), and a reference to the captenConfig struct.
NewClient: This function creates a new Client instance.
It creates a new Helm environment settings object (cli.New).
It sets the KubeConfig path in the settings based on the captenConfig.PrepareFilePath function.
It creates the temporary application values directory (captenConfig.AppValuesTempDirPath) with appropriate permissions (folderPrmission).
It returns a pointer to the new Client object.
2. Install

This function installs a Helm chart on the k3s cluster and handles upgrades if configured.

It creates a new Helm repository entry (repoEntry) based on the provided appConfig information (repository name and URL).
It creates a new Helm environment settings object (settings) with the same KubeConfig path and sets the namespace based on appConfig.
It attempts to create a new Helm chart repository using the repoEntry and a Helm getter.
On success, it updates the repository cache path and downloads the index file.
It writes the repository information to the Helm repository configuration file.
It creates a new Helm action configuration (actionConfig) with the settings, namespace, and a custom logging function (LogHelmDebug).
It checks if the application is already installed using IsAppInstalled.
If not installed, it calls installApp to perform the installation.
If installed and captenConfig.UpgradeAppIfInstalled is true, it calls upgradeApp to upgrade the application.
3. IsAppInstalled

This function checks if a specific Helm release (application) is already installed on the k3s cluster.

It creates a new Helm list action (releaseClient) using the provided actionConfig.
It retrieves a list of currently installed Helm releases (releases).
It iterates through the list and checks if any release name matches the provided releaseName (from appConfig).
If a matching release is found and its status is "deployed", it indicates a successful installation and returns true.
If no matching release is found or the status is not "deployed", it returns false.
4. LogHelmDebug

This is a custom logging function used by Helm actions within this code. It simply forwards the provided format string and arguments to the capten package logger at the debug level.

5. installApp

This function performs the installation of a Helm chart on the k3s cluster.

It creates a new Helm install action (client).
It sets various installation parameters on the action client based on appConfig:
Namespace
Release name
Version
Timeout
Create namespace (if needed)
Dry run mode (based on captenConfig.AppDeployDryRun)
Debug mode (based on captenConfig.AppDeployDebug)
It locates the Helm chart based on the provided chartName using the settings.
It loads the located chart.
If no template values are provided in appConfig, it directly runs the installation without any values.
If template values are provided, it:
Calls prepareAppValues to generate a temporary file containing the transformed app values.
Merges the chart's default values with the transformed app values.
It runs the Helm install action with the loaded chart and the merged values.
It logs the release information using the debug logger.
6. upgradeApp

This function performs the upgrade of an already installed Helm chart on the k3s cluster.

It creates a new Helm upgrade action (client).
It sets various upgrade parameters on the action client similar to installApp.
It locates the Helm chart based on the provided chartName using the settings.
It loads the located chart.
If no template values are provided in appConfig, it directly runs the upgrade without any values.
If template values are provided, it follows the same logic from installApp to generate and merge app values.
It runs the Helm upgrade action with the release name, loaded chart, and the merged values.
It logs the release information using the debug logger.