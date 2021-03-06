# Poke-App
## How app looks like:
#### Main Page
![main page](https://user-images.githubusercontent.com/26496834/87839433-e6501100-c8ee-11ea-84a9-5c9bd98e4175.PNG)

#### Drop down menu for search bar
![dropdown menu](https://user-images.githubusercontent.com/26496834/87839466-113a6500-c8ef-11ea-86e7-010f3daf2635.PNG)

#### Drop down menu when a user types some of the pokemon's name
![dropdown menu with suggestions](https://user-images.githubusercontent.com/26496834/87839474-1992a000-c8ef-11ea-959f-2408ed114d42.PNG)

## Description of build and release pipelines:
The build pipeline gets executed on a new commit to `develop` and `master` branches. The release pipeline automatically deploys the code to App Service once the build is complete for `master` branch. 

#### Build Pipeline ####
- To restrict the build pipeline to `develop` and `master` branches, they are specified in the `trigger` options such as 
```yml
trigger:
- master
- develop
```
- Pipeline variables are used to reduce amount of hard-coded strings in the pipeline.
- `pool` specifies which OS to use for the job of the pipeline. For this app `ubantu-latest` was used.
- The steps show the sequence which is going to be followed to make up a job. Each step runs on their own with access to the pipeline workspace.
  - Task is used to reference to a building block of a pipeline including the version of it. In this app, there are 3 tasks:
    - `NodeTool@0` that will install the `Node.js` version of `10.x`
    -  Script is a just a shortcut for the command line task. For this app it installs npm dependencies and generates optimised web-app files.
    - `ArchiveFiles@2` task creates a zip file from the directory specified by `buildDir`.
    - `PublishBuildArtifacts@1` task publishes a build artifact so it can be accessed by release pipeline later.
    
#### Release Pipeline ####
It deploys the artifacts that are produced by the Azure Pipeline build for this app (CI build). The CD release pipeline contains 1 stage with the Deploy Azure App Service task. This task deploys build artifacts to my App Service.

## How to use this app: 
- Enter a name of a pokemon (Not including the most recent pokemons that appear in Generation VII:Alola (some of them) and VIII:Galar)
- The app returns all important information about that pokemon such as being to see the difference between normal and shiny form of the selected pokemon, abilities, types and see from what generation with index it comes from.

## What this app involves:
- TypeScript React
- REST API for pokemons - [PokeAPI](https://pokeapi.co/)
- Pokemon images were taken from -[Pokemon Database](https://pokemondb.net/sprites/). Since this provides a better quality for them than in the API address.
- Deployment using Azure pipelines

## How to install this app:
- clone the repo
- navigate to the root directory
- enter `npm install` in a new terminal window

Once the *node_modules* folder has been created and all the dependencies have been successfully installed. 

Run `npm run start` which will compile the project and allow you to view the app locally.

Note: The project has already been set up with a live reloader. This will compile and reload the web page on your localhost port automatically when you save any file in the project. Therefore, you will NOT have to run `npm run start` every time.
