# TypeScript Configuration

## Learning Goals

- Automate TypeScript compilation with watch mode
- Describe the common configuration options for TypeScript

## Introduction

Before we continue with the TypeScript basics, let's make developing with
TypeScript a bit easier. Currently, the way we learned to compile a `.ts` 
file into `.js` is by running the `tsc <filename>` command. This was fine 
for the small example file we practiced with, but now imagine we're actively
developing a project. It would quickly become tedious having to save the file
and run the `tsc` command, before finally being able to run the app with 
Node.js. 

Additionally, we could forget to run the `tsc` command to begin with, resulting
in frustration before figuring out why the changes we made are not applying.

Thankfully, there's a way to avoid that with TypeScript's 'watch mode'.

## Watch Mode 

Watch mode keeps TypeScript's compiler running as we develop, watching for 
changes made in a specified `.ts` file or any `.ts` file within a directory. 
In watch mode, any time a change is made and saved, TypeScript will automatically
recompile the changed file for us.

To start watch mode, we use the `--watch` flag (or its shorter alias `-w`).
For example, to watch a specific file named `app.ts` we would run: 

```bash
tsc app.ts --watch 
```

Terminal would then spit out something like the following to inform us that 
watch mode is now running: 

```bash
[2:31:15 PM] Starting compilation in watch mode...

[2:31:16 PM] Found 0 errors. Watching for file changes.
```

We still have to run a command, but with watch mode we only have to run it once 
when we begin developing rather than after every change we make. 

> **Note**: To exit watch mode when you're done developing for the time being,
> use `Ctrl + C`

## Watch Multiple Files 

While all of the above is helpful, it still doesn't solve the problem when we 
have multiple files we want to watch. One solution is to simply provide all 
files to watch, like so:

`tsc app.ts home.ts contact.ts --watch` 

This works perfectly fine for small projects, but the larger a project gets, 
the more tedious that would become. Thankfully, as mentioned earlier, there is 
a way to watch an entire directory at once. 

Before we can do that, we first need to tell TypeScript that an entire directory 
is a TypeScript project. We do so by running the following command once at the 
root of the directory: 

`tsc --init` 

This initializes the directory as a TypeScript project. It will do so by 
automatically generating a `json` configuration file named `tsconfig.json`. 
We will go into more depth about this file later in this lesson. 

After this initialization, we can now use watch mode without specifying a 
file to watch, as TypeScript knows the entire directory is a TypeScript project. 
To start it, we simply omit any file name and just attach the flag: 

`tsc --watch` 

Now, it will watch for changes made to any `.ts` file within that project 
directory. 

## Configure TypeScript

Before we continue learning about TypeScript's features, let's look closer 
at the `tsconfig.json` file that was automatically generated during 
initialization. This file allows us to customize our TypeScript projects. 
Upon generation, it is filled with defaults such as the following:

```json
{
  "compilerOptions": {
    /* Language and Environment */
    "target": "es2016" /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */,

    /* Modules */
    "module": "commonjs" /* Specify what module code is generated. */,

    /* Interop Constraints */
    "esModuleInterop": true /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables 'allowSyntheticDefaultImports' for type compatibility. */,
    "forceConsistentCasingInFileNames": true /* Ensure that casing is correct in imports. */,

    /* Type Checking */
    "strict": true /* Enable all strict type-checking options. */,
    "skipLibCheck": true /* Skip type checking all .d.ts files. */
  }
}
```

Note that your file may have slightly different values for the options shown
above and will definitely have many other entries in it that are commented out. 
Those entries are provided as examples and commented out because they are not 
required in base configuration.

Let's discuss the few entries shown above that are not commented out by default:

1. `target`: since TypeScript needs to be compiled to be turned into JavaScript,
   we can actually take advantage of that step to not only convert our
   TypeScript code into JavaScript, but also to target a specific version of
   JavaScript. This can be advantageous for applications that need to support
   older versions of browsers, for example.
2. `module`: JavaScript supports the concept of "modules", which control the
   scope within which your variables, functions, classes, and etc. are available.
   `commonjs` is the `Node` module loader and is fine for us to use at the
   moment.
3. `esModuleInterop`: the need for this module is pretty technical, but the high
   level summary is that without this value set to `true`, the handling of
   modules between TypeScript and JavaScript can result in errors.
4. `forceConsistentCasingInFileNames`: different Operating Systems (Windows,
   MacOS, Linux, ...) have different rules for whether file names are case
   sensitive. This can cause issues with imports if one person working in a
   case-insensitive file system is allowed to commit an import that doesn't
   match the actual case of the target file. Then someone who is in a
   case-sensitive file system would not be able to run the same code. With this
   setting set to `true`, TypeScript enforces that the imports must match the
   case of the files in the file system.
5. `strict`: this controls a family of settings related to how strict we are
   asking TypeScript to be with types. For example, whether or not all variables 
   _must_ have a type, or if type `any` is allowed. Setting this value to `true` 
   allows you control those subsets of strict typing with additional
   configuration items, such as `strictNullChecks`, `noImplicitAny`, and others.
6. `skipLibCheck`: this tells the TypeScript compiler that it should not check
   the types in all your declaration files. Instead, `tsc` will only check the
   types that you explicitly use in your code.

There are too many other available settings to cover them all here, but here are
a few additional ones that you may eventually need to use:

1. `lib`: allows you to specify additional libraries that TypeScript should be
   aware of
2. `declaration`: this is necessary if you want to export your custom types to
   make them available as a library
3. `outDir`: a property to specify where your `.js` files will be
   generated. By default, they are generated in the same directory as your `.ts`
   files, but this can cause issues with namespaces because your IDE may not
   know how to make the distinction between variables and functions in your
   TypeScript vs. JavaScript files.
4. `include`: this property specifies the list of files `tsc` should look for if 
   it's called without an explicit list of files on the command line. By default,
   this property is not defined at all and `tsc` knows to look for any `.ts` files 
   in the entire directory. This property is useful when you only want specific 
   files or directories to be watched.  
5. `rootDir`: tells the TypeScript compiler what directory should be used as the
   base directory in the file structure that is generated by the compiler
6. `experimentalDecorators`: this one _must_ be set to `true` in order for us to
   use a very important part of Angular, which is the ability to decorate our
   code with annotations. We will cover that in more details in later sections.

## Set Up for the Following Lessons 

Now that we know how to initialize a TypeScript project and utilize its very 
helpful watch mode, we can start learning about the language's actual features. 
Let's set up a directory where we can practice everything we will learn in this 
section. 

Create a directory called `ts-practice` and initialize it as a TypeScript project.
Within the directory, create two sub-folders, one named `src` and the other `dist`.
Replace your `tsconfig.json` with this simplified, base version that we can use to 
compile the few examples we will cover in the rest of this section:

```json
{
  "compilerOptions": {
    "target": "es2016" /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */,
    "experimentalDecorators": true /* Enable experimental support for TC39 stage 2 draft decorators. */,
    "module": "commonjs" /* Specify what module code is generated. */,
    "rootDir": "./src" /* Specify the root folder within your source files. */,
    "outDir": "./dist",
    "esModuleInterop": true /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables 'allowSyntheticDefaultImports' for type compatibility. */,
    "forceConsistentCasingInFileNames": true /* Ensure that casing is correct in imports. */,
    "strict": true /* Enable all strict type-checking options. */,
    "skipLibCheck": true /* Skip type checking all .d.ts files. */
  },
  "include": ["src/**/*", "test/**/*"]
}
```

Take note that we've specified the `rootDir` as `src` and the `outDir` as `dist`. 
This means we want all our `.ts` files to be within `src` and all the compiled 
`.js` files will be generated inside `dist`. 

## Conclusion 

Watch mode and project initialization streamlines the development process so that we 
can focus on writing code. With all that under our belt, we're now equipped with all 
the tools we need to start writing some TypeScript code. Let's utilize them to create 
a TypeScript project directory where we can practice. 

## Resources 

- [TSConfig Reference - Docs on every TSConfig option](https://www.typescriptlang.org/tsconfig)