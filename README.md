# yarn_workspaces

Reference: https://classic.yarnpkg.com/lang/en/docs/workspaces/

In this test, we will set up a test project with the following file hierarchy:

![image](https://github.com/yuri-duque/yarn_workspaces/assets/26638073/1be682ea-ebcd-405d-8dac-df2091129254)


## Creating projects

root

`
yard init -y
`

app

`
mkdir app && cd app && yarn init -y && cd ..
`

ds

`
mkdir ds && cd ds && yarn init -y && cd ..
`


## Configuring project

The projects in this test will have the following dependencies.

- app project will depend on the ds project.
- ds project will not depend on any other project.
- root will depend on both app and ds.

Therefore, we need to configure the package.json files of the projects to represent this dependency.

app/package.json

```
{
  ...
  "private": true,
  "workspaces": [
    "ds"
  ]
}
```

package.json

```
{
  ...
  "private": true,
  "workspaces": [
    "app",
    "ds"
  ]
}
```

## Installing dependencies

With the projects configured, we can install the dependencies, which will create a single “node_modules” folder with each of the workspaces inside it.

![image](https://github.com/yuri-duque/yarn_workspaces/assets/26638073/2dcb503f-3443-4d9e-b4a9-843aff808188)

## Creating Scripts
To run the projects, we need scripts in each of them, but it would be impractical to run each workspace separately to run the whole project. To solve this, Yarn Workspaces has a command to help us.

yarn version: 4.2.2

`yarn workspaces foreach --all run <commandName>`

For this command to work, we need to create the command it will call in each of the workspaces.

app/package.json
```
{
  ...
  "private": true,
  "scripts": {
    "start": "node index.js"
  },
  "workspaces": [
    "ds"
  ]
}
```

app/index.js
```
console.log("APP index");
```

ds/package.json
```
{
  ...
  "private": true,
  "scripts": {
    "start": "node index.js"
  }
}
```

ds/index.js
```
console.log("DS index");
```

package.json
```
{
  ...
  "private": true,
  "scripts": {
    "start": "yarn workspaces foreach --all run start"
  },
  "workspaces": [
    "app",
    "ds"
  ]
}
```

With these files created, when we run the command yarn start, we will get the following output:

![image](https://github.com/yuri-duque/yarn_workspaces/assets/26638073/26777682-5000-43c1-8942-2171b5d0b3ed)

Note that the console output appeared in the terminal, indicating that both workspaces were executed correctly.