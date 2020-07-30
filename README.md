# gauge-dotnet-docker-linux-nuget
A docker image with Artifacts Credential Provider installed, to enable access to private nuget feed url.

`VSS_NUGET_EXTERNAL_FEED_ENDPOINTS` needs to be set as environment variable

# Example of Usage
## Run docker image directly with environment
### Set the environment variable during docker run
```
$Env:VSS_NUGET_EXTERNAL_FEED_ENDPOINTS = "{\"endpointCredentials\": [{\"endpoint\":\"${FEED_URL}\", \"username\":\"docker\", \"password\":\"${PAT}\"}]}"

docker run -e VSS_NUGET_EXTERNAL_FEED_ENDPOINTS --rm -v ${PWD}:/workspace -w /worksapce jenniferz79/gaugedotnetlinux-nuget gauge run specs
```
### Make a powershell script to make life easier.

## Build docker image with credential set
### DockerFile
```
FROM jenniferz79/gaugedotnetlinux-nuget
ARG FEED_URL
ARG PAT

ENV VSS_NUGET_EXTERNAL_FEED_ENDPOINTS "{\"endpointCredentials\": [{\"endpoint\":\"${FEED_URL}\", \"username\":\"docker\", \"password\":\"${PAT}\"}]}"
```

### Build the DockerFile with command
```
docker build --build-arg FEED_URL=<your feed url> --build-arg PAT=<personal access token> -t gauge-dotnet-linux-myimage -f .\Dockerfile .
```

### Run the newly created image
```
docker run -e VSS_NUGET_EXTERNAL_FEED_ENDPOINTS --rm -v ${PWD}:/workspace -w /worksapce jenniferz79/gauge-dotnet-linux-myimage gauge run specs
```