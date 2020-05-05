# Home Automation

![Markdown Linter](../../workflows/Markdown%20Linter/badge.svg?branch=master)

ℹ️ In this exercise we will apply the course materials from software engineering. The project will start simple and will continue to grow with features and refactorings. It is important to keep up with the extensions as they build further on the exercises that where previously made.

## Updating exercise instructions

Exercise instructions will be added as weeks pass. To get the latest instructions you need to pull them into your own project. This can be done with the following command:

```bash
git pull https://github.com/vives-software-engineering-2020/home-automation.git master
```

This will add or update the `exercise.md` file and its dependencies in your own project.

## Tagging releases

The exercise will continue to grow with extra features and changes. It is important to track the different states (or versions) of this project. For this we can make use of git **tags**.

At the end of every exercise you will need to _tag_ your version or commit. You can do this with the git tag command:

```bash
git tag v0.1
```

If for some reason you forgot to tag your version, and want to add an tag to a previous commit, you can simply add the commit hash at the end of the git tag command to tag that commit.

You can lookup previous commit by using git log:

```text
git log --pretty=oneline
15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
a6b4c97498bd301d84096da251c98a07c7723e65 beginning write support
0d52aaab4479697da7686c15f77a3d64d9165190 one more thing
6d52a271eda8725415634dd79daabbc4d9b6008e Merge branch 'experiment'
0b7434d86859cc7b8c3d5e1dddfed66ff742fcbc added a commit function
4682c3261057305bdd616e23b64b0857d832627b added a todo file
166ae0c4d3f420721acbb115cc33848dfcc2121a started write support
9fceb02d0ae598e95dc970b74767f19372d61af8 updated rakefile
964f16d36dfccde844893cac5b347e7b3d44abbc commit the todo
8a5cbc430f1a9c3d00faaeffd07798508422908a updated readme
```

and then add the commit hash when creating the tag

```bash
git tag v0.2 9fceb02
```

All made tags can be listed using the git tag command.

```bash
git tag
```

Tags are created locally by default. When pushing commits to a remote you need to specifically tell git to push the tags. You can do this with the following command:

```bash
git push origin --tags
```

Always check on GitHub in your project that the tags are pushed correctly.

## Versions

### v0.1 Thermostat

The goal of this version is to create software solution that acts as a thermostat. Given that we have some temperatures, we would like to know what the result is. The result can be used to control a heating and cooling unit.

A thermostat can be set to a _wanted temperature_ value for a room. To prevent oscillations and improve efficiency we need to give a dead-zone or _range_ where the thermostat will neither put on the heating nor put on the cooling. Next we need to be able to give the thermostat the _current temperature_ so that it can calculate the result.

![Thermostat](./img/thermostat.png)

Create a small TypeScript application that demonstrates the functionality of an `Thermostat` class. The application could have the following class structure.

![Thermostat app](./img/simple-thermostat-app.png)

The application (for now) can define some hard-coded temperatures. For each of those temperatures, we will pass them to the `Thermostat` and display the results in the console.

The goal of the `App` is just to verify the `Thermostat` class, and that it is able to correctly control heating and cooling units.

#### Tips

##### Directory structure

It's always a good idea to structure your files. All your classes should live inside a `src` directory. The `index.ts` file can be placed inside the root directory. This structure will result in a clean project, it will allow you to easily find your files, and give feedback about their function.

```text
home-automation-yourname
│   index.ts
└───src
        file_a.ts
        file_b.ts
        file_c.ts
```

### v0.2 JSON Thermostat

The `Thermostat` application is cool, but in only works using the command line. The `Thermostat` should also be able to work using JSON. Many applications provide information in JSON format. Making sure our thermostat is able to receive and respond with JSON will make it usable in many more situations.

In this version, you will create a `JSONThermostat` Class that is able to receive the settings in a JSON String, and is able to respond with a JSON String containing the result.

The `JSONThermostat` class can accept the **settings** in JSON format. The expected format is:

```json
{
  "temperature": 20.0,
  "range": 1.0
}
```

Once the `JSONThermostat` is set, it can process new values using an `update` method. The `update` method will again accept JSON using the following format:

```json
{
  "temperature": 23.4
}
```

The return value of the `update` method will output a JSON formatted `String`. An example is show below.

```json
{
  "cooling": false,
  "heating": false
}
```

#### Tests

Keep in mind to follow the json structure as defined. Later on, Unit tests will be added to check the behavior of the `JSONThermostat` class. If you fail to follow
the specified structure, the tests will fail and you will need to refactor the application.

<!-- #### Tests

Instead of using an `app.rb` or other application to test the behavior of the `JSONThermostat` we can me use of *Software Tests*. In the `test` directory you can find some tests that will create an instance of your `JSONThermostat` class and will run some code against it. When everything works as expected, the tests will PASS. If your implementation is not correct, the tests will FAIL. These test will give feedback on the expected behavior of your code.

To run the tests, you can execute the following command:

```shell
rake test
```

If all goes well and everything works like expected, you will get the following result:

```shell
Run options: --seed 48182

# Running:

..........

Finished in 0.004125s, 2424.1837 runs/s, 3636.2755 assertions/s.

10 runs, 15 assertions, 0 failures, 0 errors, 0 skips
```

If a test fails, you will get feedback why the test failed. For example:

```shell
Run options: --seed 64935

# Running:

F.........

Finished in 0.120293s, 83.1306 runs/s, 124.6958 assertions/s.

  1) Failure:
JSONThermostat#test_0001_should turn everything off when temperature is ok [C:/Users/sille/OneDrive/VIVES/SoftwareEngineering/oefeningen/thermostat-sillevl/test/json_thermostat_test.rb:16]:
--- expected
+++ actual
@@ -1,2 +1,2 @@
 # encoding: UTF-8
-"{\"cooling\":false,\"heating\":false}"
+"{\"cooling\":true,\"heating\":false}"

10 runs, 15 assertions, 1 failures, 0 errors, 0 skips
``` -->

### v0.3 Adding support for units

The thermostat is getting really nice. It can already do a lot. But at the moment the thermostat only works with a single unit of temperature. It works exclusively in Celsius or Fahrenheit or Kelvin. Some sensors provide there temperature values in a different unit that is used by the user. For example, an European user will set its thermostat using Celsius, but an American sensor that is used in the system will publish its values in Fahrenheit. It would be great if the application would support an additional 'unit' value in which the values are given.

To solve this, both the `setting` and `measurement` JSON objects could have an optional property called `unit`. The unit property could contain the following values `celsius`, `fahrenheit` or `kelvin`.

If no unit is given, it will default to `celsius`.

Any combination should be supported.

For example, you could now provide the following thermostat setting:

```json
{
  "temperature": 68.0,
  "range": 1.8,
  "unit": "fahrenheit"
}
```

And update the temperature using the following format:

```json
{
  "temperature": 293.2,
  "unit": "kelvin"
}
```

The return value of the `update` method will output a JSON formatted `String`. An example is show below.

```json
{
  "cooling": false,
  "heating": false
}
```

Note that `range` is a relative temperature difference, and `temperature` is an absolute temperature value. They should not be treated the same when converting them between different units. Eg: *1 Kelvin* range is not equal to *-272.15* °C, but *1 °C*.

## v0.4.0 NPM Package

Now that there exists some basic functionality it is time to converter the project into an library. The library will enable the use re-use the code in new and other projects without the need of writing the code all over again, or copy pasting. To leverage the power of a library it is necessary to convert the project into an **package**. A package is just some code that can be managed by some external tools. The most popular **package manager** for JavaScript and TypeScript is called **[NPM](https://npmjs.org)**.

Creating an NPM package is really easy. The only thing needed is to add some extra information about the package. This is done using an `package.json` file. The file can be created manually or by using the `npm init` command.

The `package.json` file will contain information about the project. A name, author, description, dependencies (the libraries needed to use this library)...

The goal of this iteration (version) is to create a package and publish it on npmjs.org. Every package must have a globally unique name. To manage all the exercises of every student we must agree on a naming scheme. You MUST use a scoped package name. More information can be found in the NPM documentation [about scopes](https://docs.npmjs.com/about-scopes).

```text
@[YOUR-NPM-USERNAME]/home-automation
```

* Create an account on [npmjs.org](https://npmjs.org)
* Replace \[YOUR-NPM-USERNAME\] (including the `[` and `]`) with the username of your account
* Don't forget to add the `@` sign in front.
* Use this name as the `name` of your project in your `package.json`

To get started building an NPM package, you can take a look at the following tutorials and coures:

Official NPM Documentation:

* [About NPM](https://docs.npmjs.com/about-npm/)
* [Creating a new NPM user account](https://docs.npmjs.com/creating-a-new-npm-user-account)
* [Creating a package.json file](https://docs.npmjs.com/creating-a-package-json-file)
* [Creating and publishing scoped public packages](https://docs.npmjs.com/creating-and-publishing-scoped-public-packages) (ignore the `npmrc` parts!)

Tutorials:

* [How to make a beatiful tiny npm package and publish it](https://www.freecodecamp.org/news/how-to-make-a-beautiful-tiny-npm-package-and-publish-it-2881d4307f78/)
* [Creating your first npm package](https://dev.to/therealdanvega/creating-your-first-npm-package-2ehf)

YouTube videos:

* [NPM Crash Course](https://www.youtube.com/watch?v=jHDhaSSKmB0) (Only the usage of NPM, not the creation of custom packages)
* [How to create and publish NPM Packages?](https://www.youtube.com/watch?v=rTsz09zRuTU)
* [Writing & Publishing your First NPM Package!](https://www.youtube.com/watch?v=4zzbNac6f6Q)
* [Create and Publish NPM package in less than 10 minutes](https://www.youtube.com/watch?v=I5ILzRzB46A)

## v0.4.1 `REAMDE.md` Documentation

[How to write a great README for your GitHub project](https://dbader.org/blog/write-a-great-readme-for-your-github-project)

The most important features that need to be included into your README are:

* Title
* Description
* Installation instructions
* Multiple usage instructions and examples
* Licence
* Author information

Be free to add other useful information if you like so.

### Open Source License

Take a look at [choosealicense.com/licenses](https://choosealicense.com/licenses/). What is the difference between the most popular open source licenses: **MIT**, **Apache License 2.0** and **GNU GPLv3**?

Choose a license for your package that:

* Does not state that changes to the code must be documented
* Derived work must not published under the same license (any licence can be used)

### NPM Badge

Add a badge below your title with the version of your NPM package like this:

![npm badge](https://img.shields.io/npm/v/npm.svg)

Take a look here [badges.github.io/gh-badges](https://badges.github.io/gh-badges/) to get some information on how to create a badge for your OWN package.

## v0.4.2 Cleanup

Make sure that there are NO Javascript (`*.js`) in your repository. Please clean up your project !!!

Use only `ts-node` to run and test your code, but not the `tsc` command as this will create the `.js` files.

## v0.4.3 Linter

> **lint**, or a **linter**, is a tool that analyzes source code to flag _programming errors_, _bugs_, _stylistic errors_, and _suspicious constructs_.

In order to prevent errors or bugs the project must be checked using a Linter.

### TypeScript dependency

Before continueing, it must be specified that the project uses TypeScript as well. At the moment TypeScript is installed globally, but other environments that will run your code (such as other developers, NPM, GitHub,...) must know that TypeScript is a dependency. This can now easely be solved by executing the following command:

```bash
npm install typescript --save
```

This was previously not possible becease the project was lacking a `package.json` file to document and track the projects dependencies.

### Tutorial

The following tutorial gives a clean introduction in how to setup a linter for TypeScript projects: [ESLint for TypeScript](https://khalilstemmler.com/blogs/typescript/eslint-for-typescript)

Follow the steps described in this tutorial and apply them to your project. Keep in mind the following details described below.

### `.eslintrc` configuration

Use the following configuration for your project. Do not change or alter the configuration.

```json
{
  "root": true,
  "parser": "@typescript-eslint/parser",
  "plugins": [
    "@typescript-eslint"
  ],
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/eslint-recommended",
    "plugin:@typescript-eslint/recommended"
  ],
  "rules": {
    "no-console": 1
  }
}
```

Important: **DO NOT COPY/PASTE THIS CODE**, use git to pull this code into your project using the following command:

```bash
git pull git@github.com:vives-software-engineering-2020/home-automation.git eslintrc-configuration
```

### `npm run lint` command

In order to execute the linter, NPM can be used to create a script. The script will run a custom command.

Update your `package.js` file and add the following scripts:

```json
...
  "scripts": {
    ...
    "lint": "eslint ./src --ext .ts",
    "prepublish": "npm run lint",
  }
...
```

This will create a new npm script that can be called simply by using

```bash
npm run lint
```

Use this command every time to check if your code is conform the linter rules.

#### `prepublish` script

The **prepublish** script will make sure to run the linter _before_ you package and publish your project to npm. This will prevent publishing any mistakes automatically.

#### VS Code integration

Note that you can also show ESLint errors and warnings directly into VS Code. This is very handy. You just need to install the following extension in your VS Code:

[https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

#### GitHub Action

In order to automatically get feedback from any linter mistakes on GitHub, an GitHub Action can be used. The setup of an action is beyond the scope of this exercise. Therefore a prepared configuration is made available.

To get the configuration into your project, simply run the following pull command:

```json
git pull git@github.com:vives-software-engineering-2020/home-automation.git gh-action-eslint
```

#### Visual feedback with Badges

Add the following badges to your projects README.md (just below the title) to get visual feedback on the GitHub Actions with a badge.

```markdown
![ESLint](../../workflows/ESLint/badge.svg?branch=master)
![Markdown Linter](../../workflows/Markdown%20Linter/badge.svg?branch=master)
```

## v0.5 HTTP Temperature Sensor

Instead of using a temperature sensor with hardcoded values, it should be better
to get the values from a real sensor. Many communication technologies exist to fetch
sensor data, but HTTP is the easiest and most allround method.

Extend the home automation library with a class that can fetch temperatures from
a HTTP API. These temperatures can then be used with the thermostat.

This version should add at least 2 classes.

* `HttpTemperatureSensor`: Class that only enables you to fetch a temperature value from a given URL.
* `HttpThermostat`: A Thermostat that automatically fetches its current temperature value from a given URL.

### `HttpTemperatureSensor` class

The class should be pretty easy to implement. The class should contain a constructor that accepts a string containing
an URL, and a method `getTemperature()` that returns a number representing the temperature.

You can use an library like [Axios](https://www.npmjs.com/package/axios) to create HTTP requests.
Don't forget to manage this dependency using npm and the `package.json` file.

Note: Take into account that HTTP requests and responses are _asynchronous_. This will have
an influence on your code and implementations. Try to keep it asynchronous, it's not a good practice
to make it behave synchronous. This should not be hard, but you cannot get around it.

### `HttpThermostat` class

This class is an implementation of a Thermostat that accepts a wanted value and a range (just like before),
and another extra argument in the form of a string that contains an URL. The class should not have an update
method (the temperature is not updated by calling a function but by an HTTP request), the value should
get fetched automatically when asking the object for an result (eg: isCooling?, isHeating?, getResult?...)

Note!: **Be wise** and reuse as much code as possible (without copy pasting !). Keep it DRY.

### Testing it out

In order to test your classes you can use the following HTTP API that returns a fake temperature.

[http://dummy-sensors.azurewebsites.net/api/sensor/abba5](http://dummy-sensors.azurewebsites.net/api/sensor/abba5)

### Update project

Don't forget to update your README, npm package, publish to npm, examples,...

## v0.6 Documentation

When publishing a library or project, it is important to document it. Documentation
makes it possible to let the 'user' of your code know how to use it, and tells what
your code can do.

Generating documentation for code is pretty easy. You just need to add some comments in
your code following some specific rules and you are done. Software can then interpret these
comments or docs and generate a website or any other form of documentation.

More on how to generate documentation can be found in the course [chapter 7 Source code documentation generation](https://software-engineering.devbit.courses/07-documentation/generation.html#annotations)

### Adding Documentation Comments

Add block comments containing documentation to 'all' your classes and methods.

### Update your `README.md`

Add a link in your README file to the documentation website.

Don't forget to update your npm package, publish to npm, examples,...

## v0.6.1 GitHub Pages Bug

You might have noticed that the documentation generated with TypeDoc has a bug.
When visiting your GitHub Pages in the browser, all might seem fine, but some
links result in a 404 error. This is the result of the Jekyll framework that GitHub
uses to host the GitHub Pages.

In Jekyll, files starting with an `_` (underscore) are considered special files and
will it won't copy those file to the final site. Luckily GitHub provides a feature
to disable the Jekyll processing of the files.

All you need to do is add a file named `.nojeckyll` in your `/docs` directory.

You can pull in this file using the following pull command.

```bash
git pull git@github.com:vives-software-engineering-2020/home-automation.git bugfix/docs-jekyll
```

More information about this bug/feature can be found on the [GitHub Blog](https://github.blog/2009-12-29-bypassing-jekyll-on-github-pages/)

## v0.7.0 Managing Single Responsibility and Dependencies

Next step is to evaluate the 'design' of our project. You might have a working solution
but maybe your code could be organized better to handle any kind of change (Agile?!).

Some techniques and principles can help you to evaluate and help you to change
your implementation in order to achieve a better design.

Read the following chapters:

* [Object Oriented Design](https://software-engineering.devbit.courses/08-solid/object-oriented_design.html)
* [Single Responsibility](https://software-engineering.devbit.courses/08-solid/designing_classes_with_a_single_response.html)
* [Managing Dependencies](https://software-engineering.devbit.courses/08-solid/managing_dependencies.html)

> Chapters are converter from Ruby implementation, if something is not clear, please
> let me know

Apply the tips and techniques discussed in those chapters.

Note: Changing your code to enforce Single Responsibility (SRP) and reducing your
dependencies will not change your current feature set (and thus behavior) that is
available in your project. This exercise is thus a refactoring in its purest form.
It will only result in reorganizing and restructure your existing behavior.

Changing the version number with a minor increment, might seem wrong, and your are
right. But applying SRP end managing dependencies will most likely change your
the way you use your code, and thus change its object interfaces. This means
that you will break existing applications, and thus a minor version increment can
be used to make this clear. (As we are still below v1.0.0, breaking changes are
allowed with a minor version increment)

Don't forget to update your README, documentation and npm package at the end.

### v0.7.1 Automating GitHub Pages fix

When updating your documentation website using the `npm run docs` command, the
file for the fix of the bug described in `v0.6.1` gets removed. If you are not
careful, you might commit the removal of this file. If you do commit this, the
bug will keep on existing in future versions.

The problem exists because the TypeDoc command used in `npm run docs` deletes all
files in the `/docs` directory by default. Thus also removing the `.nojekyll` file
created to fix the GitHub Pages bug.

Manually creating this file each time you generate new docs is very error prone.
The best way to solve this new problem is to automate it.

Step 1) Add a new devdependency called `touch` in your `package.json` file using the
following command:

```bash
npm install touch -D
```

This will install a package that enables us to use the touch command from within
JavaScript (and not depend on the operating system)

Step 2) Update your `npm run docs` command in the `package.json`:

Add the following script:

```json
"nojekyll": "nodetouch docs/.nojekyll",
```

Update the `docs` script to:

```json
"docs": "typedoc && npm run nojekyll",
```

This will make sure the `.nojekyll` file is created after creating the docs.

## v0.8.0 Unit Tests

When adding code to your projects might not result in the expected behavior. These
errors are called bugs and are normal to the development process. Solving bugs means
less time to write code and add useful behavior to your projects. Eliminating bugs
is thus beneficial to the development cycle. If you don't write bugs, you don't have
to solve them.

How could you make sure you are not introducing bugs into your code? There is no
magical solution, but some thing help. Unit Testing is a practice used by software
developers to detect and reduces bugs in your code. It helps refactoring your code
without worrying about introducing new bugs.

In chapter [chapter 8 Unit Testing](https://software-engineering.devbit.courses/09-unit-testing/)
is explained. Read this chapter as it will learn you what unit tests are and how
they can help your development.

The goal of this version is to add Unit tests to the project. The implementation
is already there, TDD cannot be used, but we can test our existing behavior using
unit tests.

Jest is a framework that enables unit testing in JavaScript and TypeScript. Follow
the [Jest introduction and tutorial](https://software-engineering.devbit.courses/09-unit-testing/jest.html)
and add some tests to your application.

Testing the code using a testing framework will render your `index.ts` useless.
No more need to manually run your application and inspect the results. If something
is wrong, your tests should warn you.

Tip: You could try to migrate some parts of your `index.ts` file to unit tests.

* Add at least 10 different unit tests to your application (you are free to add
more tests if you want to).
* Cover at least 3 different classes with your test.

Don't forget to publish your package on npm, update your git tags, upload tags to
github, update docs, update README... (if needed).
