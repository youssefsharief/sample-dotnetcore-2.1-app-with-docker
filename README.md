* Add `output` folder to gitignore
* To deploy
    * `dotnet restore`
    * `dotnet publish -c Release -o output`
    * `cd output`
    * `dotnet WebApplication2.dll`