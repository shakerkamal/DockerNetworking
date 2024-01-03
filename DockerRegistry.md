### Cloud for storing docker Images
To run a docker container the following command is used:
`docker run nginx`.

The nginx name is the name of the image stored in docker hub. Typically the ***User/Account* and *Image/Repository*** is provided while pulling the image from registry.

The actual format is: **Regisrtry/User/Image** which is `docker.io/nginx/nginx`.


### Docker Private Registry
Create a custom private registry with docker registry: 
`docker run -d -p 5000:5000 --name registry registry:2`

To push a image in a private registry the following command can be used:
>`docker image tag my-image localhost:5000/my-image`
>`docker push localhost:5000/my-image`

