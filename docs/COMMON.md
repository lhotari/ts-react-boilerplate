# Common

We will begin with the simplest piece, common (or shared) code. This will also serve as a good introduction to [TypeScript](https://www.typescriptlang.org/).

### <a name="initialize">Initialize</a>

We will begin by creating the structure and installing the necessary dependencies (make sure you have [Yarn](https://yarnpkg.com/lang/en/) installed). Open up your console in the directory you want to build your application in and run the following commands:

1. Initialize the Yarn-project and answer the prompts according to your application:
```
yarn init
```
2. Add the necessary dependencies, [TypeScript](https://www.typescriptlang.org/) and [TSLint](https://palantir.github.io/tslint/) (for code quality checks, a.k.a. linting). When you add dependencies in [Yarn](https://yarnpkg.com/lang/en/) they will be saved to your `package.json` and [Yarn](https://yarnpkg.com/lang/en/) will create a `yarn.lock` file to manage the versions of those dependencies. The flag `-D` saves them as `devDependencies` which will not be utilized when running the system in production.
```
yarn add -D typescript tslint
```
3. Open your project in an editor like [Visual Studio Code](https://code.visualstudio.com/) or [Atom](https://atom.io/), though I recommend [Visual Studio Code](https://code.visualstudio.com/) as it has [IntelliSense](https://en.wikipedia.org/wiki/Intelligent_code_completion).
4. Create the file `Todo.ts` inside a folder called `common` which will in turn be inside `src` that is located at the root of your application.

### <a name="configuring">Configuring TypeScript</a>

Next we will configure [TypeScript](https://www.typescriptlang.org/) by creating a file in the root folder called [tsconfig.json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html). [Tsconfig.json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html) indicates to [TypeScript](https://www.typescriptlang.org/) that the folder is a [TypeScript](https://www.typescriptlang.org/) project. Start by writing the following content into your [tsconfig.json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html):
```json
{
    "compilerOptions": {
        "noImplicitAny": true,
        "target": "es5",
        "moduleResolution": "node"
    },
    "files": [
        "src/**/*.{ts,tsx}"
    ]
}
```
On the first row we start by defining the [compilerOptions](https://www.typescriptlang.org/docs/handbook/compiler-options.html). The first rule we set is `noImplicitAny` which will enforce the use of type declarations (more about those later) when the type would otherwise be inferred as `any`. The second rule defines the target ECMAScript version (in this case [ES5](https://kangax.github.io/compat-table/es5/)) for the compiled JavaScript files. In the third line we set the [moduleResolution](https://www.typescriptlang.org/docs/handbook/module-resolution.html) mode for the [TypeScript](https://www.typescriptlang.org/) compiler as `node`, which allows the importing of dependencies from the folder `node_modules` (where [Yarn](https://yarnpkg.com/lang/en/) saves them by default) by using non-relative imports. 

### <a name="startcoding">Start coding</a>

We will begin by creating a class that defines our Todo-items. A Todo should have an id (to differentiate between todos), a title (tha actual todo) and information on whether the todo has been completed or not. Here we see the benefit of using [TypeScript](https://www.typescriptlang.org/), as it allows us to define the actual information contained in a Todo (in plain JavaScript and all other dynamically typed languages you as a developer have to keep track of such things). I'll first show you the class in its simplest form and then explain what each keyword means.

```typescript
export default class Todo {
    readonly id: number;
    readonly title: string;
    readonly done: boolean;
}
```

---

In the first line
```typescript
export default class Todo {
```
we first define that this `class` is the default export (`export default`) of our module, which means that in other files we can write
```typescript
import Todo from '../path/to/Todo.ts'
```
instead of (just writing `export`)
```typescript
import { Todo } from '../path/to/Todo.ts'
```
to get a hold of our new `Todo`-class.
> Remember that a module can only have one default export.

After the export-clause we define the `class Todo`. In [TypeScript](https://www.typescriptlang.org/) a [class](https://www.typescriptlang.org/docs/handbook/classes.html) is something you can instantiate (create an instance of), that can inherit other classes (have their properties as well).

---

On the second line
```typescript
    readonly id: number;
```
we define our first property for the `class Todo`. The first word [readonly](https://basarat.gitbooks.io/typescript/docs/types/readonly.html) defines the property as something that is [immutable](https://en.wikipedia.org/wiki/Immutable_object) and from now on [TypeScript](https://www.typescriptlang.org/) a [class](https://www.typescriptlang.org/docs/handbook/classes.html) will give you an error if you try to change the value of a `Todo`'s `number`-field. The second word `id` is the name of the property (so later on we can access it by calling `myInstanceOfTodo.id`). The last word here is the [type](https://www.typescriptlang.org/docs/handbook/basic-types.html) of the property, meaning what kind of information the property can hold, in this case a `number` (this particular type doesn't care if it is a whole number or a floating one).

---

The two following lines
```typescript
    readonly title: string;
    readonly done: boolean;
```
are otherwise the same as the first one, except the property `title` has a [type](https://www.typescriptlang.org/docs/handbook/basic-types.html) of `string`, meaning it's an arbitrary sequence of letters and characters and the property `done` has a [type](https://www.typescriptlang.org/docs/handbook/basic-types.html) of `boolean`, meaning that its value is either `true` or `false`.

---

Congratulations, you have now created your very first [TypeScript](https://www.typescriptlang.org/) `class`!

### <a name="linting">Linting</a>

It's time to start linting you code by using [TSLint](https://palantir.github.io/tslint/). Let's begin by creating a [Yarn script](https://yarnpkg.com/lang/en/docs/cli/run/) to run [TSLint](https://palantir.github.io/tslint/):
```json
// In package.json
"scripts": {
    "lint:ts": "tslint 'src/**/*.{ts,tsx}'"
}
```
The lint command can now be run with `yarn run lint:ts`. This will now run [TSLint](https://palantir.github.io/tslint/) with its default settings. However, that might not always be enough for you and if you want to define the rules for your own codebase more accurately, you can create a `tslint.json` in the root folder and populate it with rules according to [TSLint rules](https://palantir.github.io/tslint/rules/). For example in the boilerplate the `tslint.json` looks like that:
```json
{
    "extends": ["tslint-react"], // We'll go over this later
    "rules": {
        "jsx-no-lambda": false, // This is a part of tslint-react and disallows functions inside a React component's redner()-method
        "no-any": true, // Disallows use of any as a type declaration
        "no-magic-numbers": true, // Disallow magic numbers
        "only-arrow-functions": [true], // Enforces the use of arrow functions instead of the traditional syntax
        "curly": true, // Enforces curly braces in all ifs/fors/dos/whiles
        "no-var-keyword": true, // Enforces the use of the new keywords let and const instead of the old var
        "triple-equals": true, // Enforces the use of triple equals instead of double equals in conditionals
        "indent": ["spaces"], // Enforces indentation using spaces instead of tabs
        "prefer-const": true, // Enforces the use of const unless let is needed
        "semicolon": [true, "always"], // Enforces that all lines should end in a semicolon
        "eofline": true, // Enforces an empty line at the end of file
        "trailing-comma": [true, { "multiline": "always", "singleline": "never" }], // Enforces a comma at the end of all parameters that end in a new line
        "arrow-return-shorthand": [true], // Suggests one to use shorthand arrow functions when possible
        "class-name": true, // Enforces PascalCased class names
        "interface-name": [true, "always-prefix"], // Enforces all interfaces to follow PascalCasing and be prefixed with I
        "quotemark": [true, "single", "jsx-double"] // Enforces the use of single quotation marks except in React
    }
}
```

### <a name="alternatives">Alternatives</a>

Below you can find alternatives to TypeScript, if you don't fancy it as much as I:
- [Flow](http://simplefocus.com/flowtype/)
- [PureScript](http://www.purescript.org/)