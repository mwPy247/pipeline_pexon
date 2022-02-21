# Setup a pipeline in Azure DevOps
Again I will list the steps I made to build the pipeline:

1. Create an Azure account
2. Create a new project.
3. Create a new pipeline.
4. I decided to write the pipeline as specified in `pipeline.yml`

   => Comment 1: I run the pipeline on my local pool, because azure complained that ... For this, I had to set up a local pool and a host agent to be able to execute the pipeline.
   
   => Comment 2: As my application is completely containerized the tests are also inside the container which means I have to first build the image and then test it.
   
   => Comment 3: The series of steps is as follows: Build image -> Run unit tests -> Push image to azure registry -> Start Azure container instance
   
Building and running the image is basically the same as on my localhost. Only two new steps are required: Pushing the image to the azure registry and starting an azure container instance. This is how I pushed my container to the registry:

## Push to azure container registry:
Create container registry and resource group in azure and save names
	=> I used: registrierung1.azurecr.io for the server and
		        registrierung1 for the registry name

Then install the azure command line interface as documented (https://docs.microsoft.com/de-de/cli/azure/install-azure-cli) and login:

`Install azure cli`

`az acr login --name registrierung1`

Last but not least run: 

`docker tag mcr.microsoft.com/hello-world <login-server>/hello-world:v1`

`Push to registry: docker push <login-server>/hello-world:v1`

`Remove image: docker rmi <login-server>/hello-world:v1`

This is identical to what gets executed within the third stage of the pipeline. Push done!

## Start Azure container instance

The last step is to start the azure container instance as documented (https://docs.microsoft.com/de-de/azure/container-instances/container-instances-quickstart). It turned out to be easier to use the predefined task "Azure app service deploy". The configuration can be seen in the .yml-file.
