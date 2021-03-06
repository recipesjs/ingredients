This is a fork of [babylon](https://github.com/babel/babylon)

<p align="center">
  <img alt="ingredients" src="https://raw.githubusercontent.com/babel/logo/master/ingredients.png" width="700">
</p>

<p align="center">
  ingredients is a JavaScript parser used in <a href="https://github.com/recipesjs/recipes">recipes</a>.
</p>

<p align="center">
  <a href="https://travis-ci.org/recipesjs/ingredients"><img alt="Travis Status" src="https://img.shields.io/travis/recipesjs/ingredients/master.svg?style=flat&label=travis"></a>
  <a href="https://codecov.io/gh/recipesjs/ingredients"><img alt="Codecov Status" src="https://img.shields.io/codecov/c/github/recipesjs/ingredients/master.svg?style=flat"></a>
</p>

 - The latest ECMAScript version enabled by default (ES2017).
 - Comment attachment.
 - Support for JSX and Flow.
 - Support for experimental language proposals.

## Credits

Heavily based on [acorn](https://github.com/marijnh/acorn) and [acorn-jsx](https://github.com/RReverser/acorn-jsx),
thanks to the awesome work of [@RReverser](https://github.com/RReverser) and [@marijnh](https://github.com/marijnh).

Significant diversions are expected to occur in the future such as streaming, EBNF definitions, sweet.js integration, interspacial parsing and more.

## API

### `ingredients.parse(code, [options])`

### Options

- **allowImportExportEverywhere**: By default, `import` and `export`
  declarations can only appear at a program's top level. Setting this
  option to `true` allows them anywhere where a statement is allowed.

- **allowReturnOutsideFunction**: By default, a return statement at
  the top level raises an error. Set this to `true` to accept such
  code.

- **allowSuperOutsideMethod** TODO

- **sourceType**: Indicate the mode the code should be parsed in. Can be
  either `"script"` or `"module"`.

- **sourceFilename**: Correlate output AST nodes with their source filename.  Useful when generating code and source maps from the ASTs of multiple input files.

- **plugins**: Array containing the plugins that you want to enable.

### Output

ingredients generates AST according to [Babel AST format][].
It is based on [ESTree spec][] with the following deviations:

- [Literal][] token is replaced with [StringLiteral][], [NumericLiteral][], [BooleanLiteral][], [NullLiteral][], [RegExpLiteral][]
- [Property][] token is replaced with [ObjectProperty][] and [ObjectMethod][]
- [MethodDefinition][] is replaced with [ClassMethod][]
- [Program][] and [BlockStatement][] contain additional `directives` field with [Directive][] and [DirectiveLiteral][]
- [ClassMethod][], [ObjectProperty][], and [ObjectMethod][] value property's properties in [FunctionExpression][] is coerced/brought into the main method node.

AST for JSX code is based on [Facebook JSX AST][] with the addition of one node type:

- `JSXText`

[Babel AST format]: https://github.com/recipesjs/ingredients/blob/master/ast/spec.md
[ESTree spec]: https://github.com/estree/estree

[Literal]: https://github.com/estree/estree/blob/master/spec.md#literal
[Property]: https://github.com/estree/estree/blob/master/spec.md#property
[MethodDefinition]: https://github.com/estree/estree/blob/master/es6.md#methoddefinition

[StringLiteral]: https://github.com/recipesjs/ingredients/blob/master/ast/spec.md#stringliteral
[NumericLiteral]: https://github.com/recipesjs/ingredients/blob/master/ast/spec.md#numericliteral
[BooleanLiteral]: https://github.com/recipesjs/ingredients/blob/master/ast/spec.md#booleanliteral
[NullLiteral]: https://github.com/recipesjs/ingredients/blob/master/ast/spec.md#nullliteral
[RegExpLiteral]: https://github.com/recipesjs/ingredients/blob/master/ast/spec.md#regexpliteral
[ObjectProperty]: https://github.com/recipesjs/ingredients/blob/master/ast/spec.md#objectproperty
[ObjectMethod]: https://github.com/recipesjs/ingredients/blob/master/ast/spec.md#objectmethod
[ClassMethod]: https://github.com/recipesjs/ingredients/blob/master/ast/spec.md#classmethod
[Program]: https://github.com/recipesjs/ingredients/blob/master/ast/spec.md#programs
[BlockStatement]: https://github.com/recipesjs/ingredients/blob/master/ast/spec.md#blockstatement
[Directive]: https://github.com/recipesjs/ingredients/blob/master/ast/spec.md#directive
[DirectiveLiteral]: https://github.com/recipesjs/ingredients/blob/master/ast/spec.md#directiveliteral
[FunctionExpression]: https://github.com/recipesjs/ingredients/blob/master/ast/spec.md#functionexpression

[Facebook JSX AST]: https://github.com/facebook/jsx/blob/master/AST.md

### Example

```javascript
require("ingredients").parse("code", {
  // parse in strict mode and allow module declarations
  sourceType: "module",

  plugins: [
    // enable jsx and flow syntax
    "jsx",
    "flow"
  ]
});
```

### Plugins

 - `jsx`
 - `flow`
 - `doExpressions`
 - `objectRestSpread`
 - `decorators`
 - `classProperties`
 - `exportExtensions`
 - `asyncGenerators`
 - `functionBind`
 - `functionSent`
