# About Zimbra

Zimbra is a great Open Source email server. It is widely used in private cloud or on-premise. Installing Zimbra can be challenging, so we make this into container for easy deployment. The steps can be summaried as below:

1. Create a VM (or physical server). Install Basic OS.
2. Install Docker following this guide: https://docs.docker.com/engine/install/
3. See [USAGE](USAGE.md) file to run your Zimbra.

# Zimbra Docker container

Docker is a great tool for system integrator to automate the deployment work so that it is predictable and easy to maintain afterward.

The Zimbra software is designed to run as VM. However we package it to run as Docker container. The result is still a traditional Zimbra on VM but run as a container.

# Example use of this solution

Here is one good way to deploy our Zimbra container:

1. Devel: `mkdir devel` and create `compose.yml`. This can be used for testing and further development. Test your changes before try it on production.

2. Staging: `mkdir staging` and create `compose.yml`. Apply what you did in the testing environment here. A staging environment could have other systems relying on your Zimbra. This is a good place to verify everything still work as expected.

3. Production: `mkdir production` and create `compose.yml`. The actual production environment.

You can use 3 VM for each of the environment above. Or you can squeeze all into one VM.

By following the process above, you avoid modifying the production system directly. You should test it at Devel, verify it at Staging, and then finally apply it to the Production. This give you a predictable outcome.

Refer to the [USAGE](USAGE.md) for deployment guide.

# Build the image

Refer to the [BUILD](BUILD.md) for more info.
