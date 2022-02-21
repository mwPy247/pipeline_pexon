# Setup a pipeline in Azure DevOps
Again I will list the steps I made to build a pipeline:

1. Create an Azure account
2. Create a new project.
3. Create a new pipeline.
4. I decided to write the pipeline as specified in `pipeline.yml`
   => Comment 1: I run the pipeline on my local pool, because azure complained that ... For this, I had to set up a local pool and a host agent to be able to execute the pipeline.
   => Comment 2: As my application is completely containerized the tests are also inside the container which means I have to first build the image and then test it.
   => Comment 3: The series of steps is as follows: Build image -> Run unit tests -> Push image to azure registry -> Start Azure container instance
