# Flow
1. Implement your library
2. Set up for release
3. Login to the `npm`
4. Publish your library to the `npm`
## 1. Implement your library
In this step, you just need to implement your code for library.
## 2. Set up for release
In this step, you should prepare some setting for release.
### 1. Login to npm
```shell
npm login
```
### 2. Update entry file
You should place this code at the top of the file.
```js
#!/usr/bin/env node
```
Like below.
```js
// This file should be entry point of the library
// e.g. index.js or index.ts
#!/usr/bin/env node

// library logic
// ... //
```
### 3. Update package.json
You should set the `bin`.
And the key of the `bin` should be same with the `name` field.
```json
{
	// ... //
	"name": "name_of_my_library",
	"bin": {
		"name_of_my_library": "./index.js"
	},
}
```
### 4. Publish to npm
```shell
npm publish
```
## 3. Check that it works
Check it works properly on the dummy project :)
```shell
npx name_of_my_library

or 

npm i name_of_my_library
```
# Note
## Should include `dependencies` / `devDependencies` to `package.json`
If you don't include the dependencies used by your application,
an error will occur when the user installs your library.
## Change the version of the library for each publish
Before publishing, you must update the `version` in `package.json`.
```json
// Current status
{
	// ... //
	"version": "0.0.1",
}

// When you publish new version
{
	// ... //
	"version": "0.0.2",
}
```
## ESmodule vs CommonJs 
If you want to support `ESmodule` and `CommonJS`,
you should dfd