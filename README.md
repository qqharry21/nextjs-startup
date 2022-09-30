# Project startup

`npx create-next-app@latest --ts ./`

## Install Eslint

`npm install eslint --save-dev`

Initialize the config file

`npx eslint --init`

If you are using YARN, then run this command

`yarn add -D eslint-plugin-react@latest @typescript-eslint/eslint-plugin@latest @typescript-eslint/parser@latest`

Install TypeScript plugins related to ESLint

`npm install -D eslint-plugin-import @typescript-eslint/parser eslint-import-resolver-typescript`

## Install Prettier

`npm install -D prettier eslint-config-prettier eslint-plugin-prettier eslint-plugin-react-hooks`

Create the prettierrc file
`touch .prettierrc`

If you are going to use jest in this project, then add this line to eslintrc file

```JSON
{
    "env": {
        "browser": true,
        "es2021": true,
	"jest": true // Add this line!
    },
	...
}
```

To use prettier alongside with eslint you need to change the extends object:

```JSON
{
	...
	"extends": [
	  "eslint:recommended",
	  "plugin:react/recommended",
	  "plugin:@typescript-eslint/recommended",
	  "prettier" // Add this line!
	],
	...
}
```

**Basic rules**

```JSON
{
	...
	"rules": {
        "react/react-in-jsx-scope": "off",
        "camelcase": "error",
        "spaced-comment": "error",
        "quotes": ["error", "single"],
        "no-duplicate-imports": "error"
  },
	...
}
```

Update the eslintrc file

```JSON
"plugins": ["react", "react-hooks", "@typescript-eslint", "prettier"],
```

The last thing to set up in ESLint is the eslint-import-resolver-typescript. Just add the settings key in the eslint configuration file.

```JSON
{
	...
	"settings": {
    "import/resolver": {
      "typescript": {}
    }
  }
}
```

Update prettierrc file

```JSON
{
  "semi": false,
  "tabWidth": 2,
  "printWidth": 100,
  "singleQuote": true,
  "trailingComma": "all",
  "jsxSingleQuote": true,
  "bracketSpacing": true
}
```

## Install Commitizen

`npm i commitizen -D`

`npx commitizen init cz-conventional-changelog`

Add this line to your package.json file

```JSON
{
  ...,
   "config": {
    "commitizen": {
      "path": "cz-conventional-changelog"
    }
  }
}
```

And `prepare-commit-msg` file after install Husky

```
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

exec < /dev/tty && node_modules/.bin/cz --hook || true
```

## Install Husky

`npx husky-init && npm install`

Update `pre-commit` file

```
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

echo '🛠️🛠️🛠️  Styling, testing and building your project before committing  🛠️🛠️🛠️'

# Check tsconfig
npm run check-types ||
(
  echo '🚨🚨🚨  TypeScript check failed  🚨🚨🚨'
  false
)

# Check Prettier standards
npm run format && npm run check-format ||
(
  echo '🚨🚨🚨  Prettier check failed  🚨🚨🚨'
  false
)

# Check ESLint standards
npm run check-lint ||
(
  echo '🚨🚨🚨  ESLint check failed  🚨🚨🚨'
  false
)

echo '✅✅✅  All checks passed  ✅✅✅'

npm run build ||
(
  echo '❌🚨❌  Build failed  ❌🚨❌'
  false
)

echo '✅✅✅  Build Succeed  ✅✅✅'
```

## Setup editor config

Create .editorconfig

```
root = true

[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
```

Create vscode config for this project

```JSON
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.tslint": true
  },
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[javascript]": {
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "typescript.tsdk": "node_modules/typescript/lib",
  "[typescriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

Update package.json

```JSON
{
  "prepare": "husky install",
  "check-types": "tsc --pretty --noEmit",
  "check-format": "prettier -w . && prettier -c .",
  "check-lint": "eslint src/**/*.{js,jsx,ts,tsx,json}",
  "fix-lint": "eslint --fix 'src/**/*.{js,jsx,ts,tsx,json}'",
  "format": "prettier --write 'src/**/*.{js,jsx,ts,tsx,css,md,json}' --config ./.prettierrc",
  "test-all": "npm run format && npm run check-lint && npm run check-types && npm run build"
}
```
