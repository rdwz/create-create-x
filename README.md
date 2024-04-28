<div align="center">
<h1 align="center">✨ Create Create X</h1>
<h6>Create your own 🛠️create-something app. A fork of the awesome `create-create-app`</h6>
<br/>
<img src="https://raw.githubusercontent.com/painfulexistence/create-create-x/main/.github/assets/ss1.png" alt="screenshot"/>
<br/><br/>
<a href="https://npmjs.org/package/create-create-x"><img src="https://img.shields.io/npm/v/create-create-x.svg"></a>
<a href="https://npmjs.org/package/create-create-x"><img src="https://badgen.net/npm/dt/create-create-x"></a>
<a href="https://github.com/painfulexistence/create-create-x/actions?workflow=test"><img src="https://github.com/painfulexistence/create-create-x/workflows/test/badge.svg"/></a>
</div>

## Why?

- 🎩 **Template engine** You can use template strings in text files, file names, and folder names.
- 💄 **Highly customizable** Can change caveat text, add extra command-line options.

## Table of contents

- [✨ Create Create X](#-create-create-x)
  - [Why?](#why)
  - [Table of contents](#table-of-contents)
  - [Quick Start](#quick-start)
  - [Making Your Templates](#template)
  - [Configuring Your Create X App](#config)
  - [Contributing](#contribution)
    - [Contributors ✨](#contributors-)

## Quick Start

Let's create `create-greet` package in five steps.

### 1. Bootstrap your project

```shell
# Any of the following commands will work
npx create-create-x@latest greet # recommended
npm init create-x greet
yarn create create-x greet
pnpm create create-x greet
bun create create-x greet
```

You will then be asked about your project.

![screenshot](https://raw.githubusercontent.com/painfulexistence/create-create-x/main/.github/assets/ss1.png)

### 2. Add and edit template files

```shell
cd create-greet
```

Then you can see the `templates/default` folder where the actual template files go.

Note that `.gitignore` files should be named `gitignore` to avoid being ignored on publishing.

### 3. Build the app (TypeScript only)

Run `npm run build` or `yarn build` to transpile TypeScript code into JavaScript. If you chose the default template, this step is not necessary.

### 4. Publish package to npm

Run `npm publish` or `yarn publish` to publish your `create-greet` app to npm.

### 5. Use the published package

```shell
# Any of the following commands will work
npx create-greet ohayo
npm init greet ohayo
yarn create greet ohayo
pnpm create greet ohayo
bun create greet ohayo
```

![screenshot](https://raw.githubusercontent.com/painfulexistence/create-create-x/main/.github/assets/ss2.png)

## Making Your Templates

### Single Template

Edit files inside `templates/default`. Every file name, directory name, and a text file will be processed through Handlebars template engine to replace all template strings with the respective value.

Built-in variables are:

- `{{name}}` package name (e.g. `ohayo`)
- `{{description}}` package description
- `{{author}}` author name (e.g. `John Doe`)
- `{{email}}` author email (e.g. `john@example.com`)
- `{{contact}}` author name formatted with `{{name}} <{{email}}>`. If email is missing, simply `{{name}}`
- `{{license}}` package license (e.g. `MIT`)
- `{{year}}` current year (e.g. `2021`)
- `{{template}} selected template name (e.g. `typescript`)
- `{{packageManager}} package manager (e.g. `yarn`)

### Multiple templates

Creates a new directory in the location defined by `templateRoot`. It can be accessed via `--template` flag (e.g. `create-something <name> --template <template>`).
You might want to set `promptForTemplate` to `true` to explicitly ask the user to choose a template during the initialization phase. If `promptForTemplate` is `false`, which is the default behavior, `default` template will be used unless the user explicitly selects a template via `--template` cli flag.

### Helper functions

In the following example, we assume that the variable `name` is `create-react-app`.

#### `upper`

Convert text to UPPERCASE.

`{{upper name}}` becomes `CREATE-REACT-APP`.

#### `lower`

Convert text to lowercase.

`{{lower name}}` becomes `create-react-app`.

#### `capital`

Convert text to CapitalCase.

- `{{capital name}}` becomes `CreateReactApp`
- `{{capital name space=true}}` becomes `Create React App`.

#### `camel`

Convert text to camelCase.

`{{camel name}}` becomes `createReactApp`.

#### `snake`

Convert text to snake_case.

`{{snake name}}` becomes `create_react_app`.

#### `kebab`

Convert text to kebab-case.

`{{kebab name}}` becomes `create-react-app`.

#### `space`

Replace all word separators with single space.

`{{space name}}` becomes `create react app`

#### `uuid`

Generates unique UUID string.

```
{{uuid}} // => a5df7100-da46-47a6-907e-afe861f48b39
{{upper (uuid)}} // => A5DF7100-DA46-47A6-907E-AFE861F48B39
```

## Configuring Your Create X App

The app configuration can be found in `src/cli.js` (or `src/cli.ts` if you choose the `typescript` template).

```ts
import { resolve } from "path";
import { create } from "create-create-x";

create("create-greet", {
  templateRoot: resolve(__dirname, "..", "templates"),
  extra: {
    language: {
      type: "input",
      describe: "greeting language",
      default: "en",
      prompt: "if-no-arg",
    },
  },
  modifyName: (name) => `package-prefix-${name}`,
  after: async ({ installNpmPackage }) => {
    console.log("Installing additional packages");
    await installNpmPackage("chalk");
  },
  caveat: `Your app has been created successfully!`,
});
```

### templateRoot (required)

`templateRoot` is set to `path.resolve(__dirname, '../templates')`. You can change this to any location you like.

### modifyName (default: `undefined`)

`(name: string) => string | Promise<string>`

Modify `name` property.

```js
{
  modifyName: (name) => (name.startsWith("create-") ? name : `create-${name}`);
}
```

### extra (default: `undefined`)

`object | undefined`

Additional questions can be defined. These options will be available as CLI flags, interactive questions, and template strings. In the example above, `--language` flag and the `{{language}}` template string will be enabled in the app.

All possible options can be found in the [yargs-interactive documentation](https://github.com/nanovazquez/yargs-interactive#options).

### defaultDescription (default: `description`)

Default value for a package description.

### defaultAuthor (default: `user.name` in `~/.gitconfig` otherwise `Your name`)

Default value for a package author.

### defaultEmail (default: `user.email` in `~/.gitconfig` otherwise `Your email`)

Default value for a package author email.

### defaultTemplate (default: `default`)

Default value for a template.

### defaultLicense (default: `MIT`)

Default value for license.

### defaultPackageManager (default: `undefined`)

Default value for package manager. `npm`, `yarn` and `pnpm` are available. `undefined` to auto detect package manager.

### promptForDescription (default: `true`)

Interactively asks users for a package description.

### promptForAuthor (default: `true`)

Interactively asks users for a package author.

### promptForEmail (default: `true`)

Interactively asks users for a package author email.

### promptForTemplate (default: `false`)

Interactively asks users to select a template. If there are no multiple templates in the `templates` directory, it won't display a prompt anyways.

Even if `promptForTemplate` is set to `false`, users can still specify a template via a command line flag `--template <template>`.

```
create-something <name> --template <template>
```

### promptForLicense (default: `true`)

Interactively asks users for a package license.

### promptForPackageManager (default: `false`)

Interactively asks users for a package manager to use for installing packages from npm.

### skipGitInit (default: `false`)

Skip initializing a git repository at a creation time.

### skipNpmInstall (default: `false`)

Skip installing package dependencies at a creation time.

### after (default: `undefined`)

`(options: AfterHookOptions) => void`

Define after-hook script to be executed right after the initialization process.

### caveat (default: `undefined`)

`string | ((options: AfterHookOptions) => string | void) | undefined`

The caveat message will be shown after the entire process is completed.

```js
create("create-greet", {
  caveat: "Happy coding!",
});
```

```js
create("create-greet", {
  caveat: ({ answers }) => `Run -> cd ${answers.name} && make`,
});
```

```js
create("create-greet", {
  extra: {
    plugin: {
      type: "input",
      describe: "plugin to be used in your project",
      default: "some-plugin",
      prompt: "if-no-arg",
    },
  },
  after: async ({ installNpmPackage, answers }) => {
    const plugin = answers.plugin;
    console.log(`Installing additional package: ${plugin}`);
    await installNpmPackage(plugin);
  },
  caveat: ({ packageDir }) => {
    console.log("Next step:");
    console.log(`cd ${packageDir} && npm start`);
  },
});
```

### `AfterHookOptions`

```typescript
{
  // variables
  name: string; // e.g. "create-greet"
  template: string; // e.g. "default"
  packageDir: string; // e.g. "/path/to/ohayo"
  templateDir: string; // e.g. "/path/to/create-greet/templates/default"
  year: number; // e.g. 2020
  answers: {
    name: string; // package name passed through `modifyName`
    description: string; // description
    author: string; // e.g. "John Doe"
    email: string; // e.g. "john@example.com"
    contact: string; // e.g. "John Doe <john@example.com>"
    license: string; // e.g. "MIT"
    [key: string]: string | number | boolean | any[]; // any values defined in the `extra` field.
  };

  // helper functions
  run: (command: string, options?: CommonOptions<string>) => ExecaChildProcess<string>; // execute shell commands in the package dir
  installNpmPackage: (packageName: string | [string], isDev?: boolean) => Promise<void>; // install npm package. uses package manager specified by --node-pm CLI param (default: auto-detect)
}
```

## Contributing

PRs are always welcomed.

### Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tbody>
    <tr>
      <td align="center" valign="top" width="14.28%"><a href="https://uechi.io/"><img src="https://avatars0.githubusercontent.com/u/431808?v=4?s=100" width="100px;" alt="uetchy"/><br /><sub><b>uetchy</b></sub></a><br /><a href="https://github.com/painfulexistence/create-create-x/commits?author=uetchy" title="Code">💻</a> <a href="https://github.com/painfulexistence/create-create-x/commits?author=uetchy" title="Documentation">📖</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://vivliostyle.org/"><img src="https://avatars1.githubusercontent.com/u/3324737?v=4?s=100" width="100px;" alt="Shinyu Murakami"/><br /><sub><b>Shinyu Murakami</b></sub></a><br /><a href="https://github.com/painfulexistence/create-create-x/commits?author=MurakamiShinyu" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="http://twitter.com/takahashim"><img src="https://avatars2.githubusercontent.com/u/10401?v=4?s=100" width="100px;" alt="Masayoshi Takahashi"/><br /><sub><b>Masayoshi Takahashi</b></sub></a><br /><a href="https://github.com/painfulexistence/create-create-x/commits?author=takahashim" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="http://alexanderliu.com"><img src="https://avatars.githubusercontent.com/u/41758627?v=4?s=100" width="100px;" alt="Alexander Liu"/><br /><sub><b>Alexander Liu</b></sub></a><br /><a href="https://github.com/painfulexistence/create-create-x/commits?author=alexanderl19" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://vilja.me"><img src="https://avatars.githubusercontent.com/u/24564003?v=4?s=100" width="100px;" alt="Vilja"/><br /><sub><b>Vilja</b></sub></a><br /><a href="https://github.com/painfulexistence/create-create-x/commits?author=iVilja" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://lucaslabs.tech/"><img src="https://avatars.githubusercontent.com/u/12949236?v=4?s=100" width="100px;" alt="Lucas Colombo"/><br /><sub><b>Lucas Colombo</b></sub></a><br /><a href="https://github.com/painfulexistence/create-create-x/commits?author=lucas-labs" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="http://twitter.com/DevLucidum/"><img src="https://avatars.githubusercontent.com/u/30090734?v=4?s=100" width="100px;" alt="Loïc Chen"/><br /><sub><b>Loïc Chen</b></sub></a><br /><a href="https://github.com/painfulexistence/create-create-x/commits?author=painfulexistence" title="Code">💻</a></td>
    </tr>
  </tbody>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!
