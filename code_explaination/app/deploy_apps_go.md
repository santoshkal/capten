# DeployApps

This function deploys a set of apps using Helm. The function takes a config.CaptenConfig struct, a map of global values, and a group file as arguments.

It calls the prepareAppGroupConfigs function to prepare the app group configurations.
It creates a new Helm client using the helm.NewClient function.
It installs the app group configurations using the installAppGroup function.
If the installation is successful, it returns no error. Otherwise, it returns an error.

## installAppGroup

This function installs a set of app configurations using Helm. The function takes a config.CaptenConfig struct, a Helm client, and a slice of app configurations as arguments.

It iterates over the app configurations and installs each one using the Helm client.
If an app configuration is already installed, it logs a message indicating that the app is already installed.
If an app configuration installation fails, it logs an error message and continues to the next app configuration.
If all app configurations are installed successfully, it returns true. Otherwise, it returns false.

## prepareAppGroupConfigs

This function prepares a set of app group configurations. The function takes a config.CaptenConfig struct, a map of global values, and an app group name file as arguments.

It reads the app group name file using the GetApps function.
It iterates over the app names and reads the corresponding app configuration files using the GetAppConfig function.
It prepares each app configuration by setting the template values, override values, UI endpoint, and API endpoint.
It returns a slice of prepared app configurations.

## replaceOverrideTemplateValues

This function replaces template values in an override values map with actual values. The function takes a template values map and a values map as arguments.

It marshals the template values map into a YAML string.
It parses the YAML string into a template.
It executes the template with the values map as input.
It unmarshals the resulting YAML string into a new map.
It returns the new map.
replaceTemplateStringValues

This function replaces template values in a string with actual values. The function takes a template string and a values map as arguments.

It parses the template string into a template.
It executes the template with the values map as input.
It returns the resulting string.

### Note: The config.CaptenConfig struct is used to construct file paths and other configuration-related functionality throughout the functions.