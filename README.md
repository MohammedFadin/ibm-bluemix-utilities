# IBM Bluemix Utilities

## `dockercfg` Generator

Codeship supports using custom images to generate dockercfg files during the build process. To use this image to integrate with IBM Bluemix Registry, simply define an entry in your services file for this image, and reference it from any steps or services which need to interact with the Bluemix Registry repositories with the `dockercfg_service` field. You'll also need to provide the following environment variables using an [encrypted env file](https://codeship.com/documentation/docker/encryption/):

* BLUEMIX_API_KEY - Your Bluemix API key

Optionally, you can also set the following variables to assume a role across accounts before generating the dockercfg:

* BLUEMIX_API_ENDPOINT - The API endpoint, defaults to api.ng.bluemix.net

Here is an example of using and ECR Dockercfg generator to authenticate pushing an image.

```yaml
# codeship-services.yml
app:
  build:
    image: registry.ng.bluemix.net/your_namespace/image
    dockerfile_path: ./Dockerfile
bluemix_dockercfg:
  image: codeship/ibm-bluemix-dockercfg-generator
  add_docker: true
  encrypted_env_file: bluemix.env.encrypted
```

```yaml
# codeship-steps.yml
- service: app
  type: push
  tag: master
  image_name: registry.ng.bluemix.net/your_namespace/image
  registry: registry.ng.bluemix.net/
  dockercfg_service: bluemix_dockercfg:
```

You can also use this authentication to pull images, or use with caching, by defining the `dockercfg_service` field on groups of steps, or each individual step that pulls or pushes an image, or by adding the field to specific services.
