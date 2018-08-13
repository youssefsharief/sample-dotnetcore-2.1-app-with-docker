* Add `output` folder to gitignore
* To deploy
    * `dotnet restore`
    * `dotnet publish -c Release -o output`
    * `cd WebApplication2/output`
    * `dotnet WebApplication2.dll`

* To deploy using docker
    * To understand the content of the Dockerfile
        * We are having 2 base images. "microsoft/dotnet:2.1-sdk" and "microsoft/dotnet:2.1-aspnetcore-runtime"
            * The former is used to build our app and is not optimized for production 
                * In this image, first, we only need to copy the *.sln file and the *.csproj file in order to get Nuget dependencies using `dotnet restore`
                * Then we copy the rest of our content in order to be able to build and publish our production build using `dotnet publish -o output`. The dlls are ssaved in the "output" directory as shown from the command
            * The later is used to run the app from the dlls and is optimized for production. That's why it has a smaller base image which is "microsoft/dotnet:2.1-aspnetcore-runtime"
                * We only need to copy the output direcotry from the previous build and then run the `dotnet WebApplication2.dll` command
    * `docker build -t web2 .` Wait for the base image to get downloaded for the first time
    * Now we sussessfully have an image called web2
        * You could check that by running `docker images`
            * Notice that the size of the image is only 258 Mb since it has only the runtime and is optimized for production
    * Now you could run a container from the last resulted image by `docker run -ti web2:latest`
    * Currenlty we could run build and run the docker image and the app is running in port 80 inside the container but we need to add to expose the container from the outside
  