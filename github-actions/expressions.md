# Expressions

You can use expressions to programmatically set environment variables in workflow files and access contexts.

## Literals

As part of an expression, you can use `boolean`, `null`, `number`, or `string` data types.

| Data type | Literal value                                                                                         |
| --------- | ----------------------------------------------------------------------------------------------------- |
| `boolean` | `true` or `false`                                                                                     |
| `null`    | `null`                                                                                                |
| `number`  | Any number format supported by JSON.                                                                  |
| `string`  | You don't need to enclose strings in ${{ and }}. However, if you do, you must use single quotes (')   |
|           | around the string. To use a literal single quote, escape the literal single quote using an additional |
|           | single quote (''). Wrapping with double quotes (") will throw an error.                               |

Example of literals

```yml
env:
  myNull: ${{ null }}
  myBoolean: ${{ false }}
  myIntegerNumber: ${{ 711 }}
  myFloatNumber: ${{ -9.2 }}
  myHexNumber: ${{ 0xff }}
  myExponentialNumber: ${{ -2.99e-2 }}
  myString: Mona the Octocat
  myStringInBraces: ${{ 'It''s open source!' }}
```

## Operators

| Operator | Description           |
| -------- | --------------------- | --- | --- |
| ( )      | Logical grouping      |
| [ ]      | Index                 |
| .        | Property de-reference |
| !        | Not                   |
| <        | Less than             |
| <=       | Less than or equal    |
| >        | Greater than          |
| >=       | Greater than or equal |
| ==       | Equal                 |
| !=       | Not equal             |
| &&       | And                   |
|          |                       |     | Or  |

> Notes:
>
> GitHub ignores case when comparing strings.
>
> `steps.<step_id>.outputs.<output_name>` evaluates as a `string`. You need to use specific syntax to tell GitHub to evaluate an expression rather than treat it as a `string`.
>
> When you use expressions in an `if` conditional, you may omit the `${{ }}` expression syntax because GitHub Actions automatically evaluates the `if` conditional as an expression. Using the `${{ }}` expression syntax turns the contents into a `string`, and strings are truthy. For example, `if: true && ${{ false }}` will evaluate to `true`.
>
> For numerical comparison, the `fromJSON()` function can be used to convert a string to a number.

GitHub offers ternary operator like behaviour that you can use in expressions.

```yml
env:
  MY_ENV_VAR: ${{ github.ref == 'refs/heads/main' && 'value_for_main_branch' || 'value_for_other_branches' }}
```

## Functions

Some functions cast values to a string to perform comparisons:

| Type    | Result                                        |
| ------- | --------------------------------------------- |
| Null    | ''                                            |
| Boolean | 'true' or 'false'                             |
| Number  | Decimal format, exponential for large numbers |
| Array   | Arrays are not converted to a string          |
| Object  | Objects are not converted to a string         |

### `contains`

`contains(search, item)`

Returns `true` if search contains item. If search is an array, this function returns `true` if the item is an element in the array. If search is a `string`, this function returns `true` if the item is a substring of search. **This function is not case sensitive.** Casts values to a string.

### `startsWith`

`startsWith(searchString, searchValue)`

Returns true when `searchString` starts with `searchValue`. This function is not case sensitive. Casts values to a string.

### `endsWith`

`endsWith(searchString, searchValue)`

Returns true if `searchString` ends with `searchValue`. This function is not case sensitive. Casts values to a string.

### `format`

`format(string, replaceValue0, replaceValue1, ..., replaceValueN)`

Replaces values in the string, with the variable `replaceValueN`. Variables in the string are specified using the `{N}` syntax, where N is an integer. You must specify at least one replaceValue and string.

`format('Hello {0} {1} {2}', 'Mona', 'the', 'Octocat')`

Returns `'Hello Mona the Octocat'`.

### `join`

`join(array, optionalSeparator)`

The value for array can be an array or a string. All values in array are concatenated into a string. If you provide optionalSeparator, it is inserted between the concatenated values. Otherwise, the default separator , is used. Casts values to a string.

### `toJSON`

`toJSON(value)`

Returns a pretty-print JSON representation of value. You can use this function to debug the information provided in contexts.

### `fromJSON`

`fromJSON(value)`

Returns a JSON object or JSON data type for value. You can use this function to provide a JSON object as an evaluated expression or to convert environment variables from a string.

### `hashFiles`

`hashFiles(path)`

Returns a single hash for the set of files that matches the path pattern. You can provide a single path pattern or multiple path patterns separated by commas. The path is relative to the `GITHUB_WORKSPACE` directory and can only include files inside of the `GITHUB_WORKSPACE`. This function calculates an individual `SHA-256` hash for each matched file, and then uses those hashes to calculate a final `SHA-256` hash for the set of files. If the path pattern does not match any files, this returns an empty string.

`hashFiles('**/package-lock.json')`

`hashFiles('**/package-lock.json', '**/Gemfile.lock')`

## Status check functions

### `success`

Returns `true` when none of the previous steps have failed or been canceled.

```yml
steps:
  ...
  - name: The job has succeeded
    if: ${{ success() }}
```

### `always`

Causes the step to always execute, and returns `true`, even when canceled. The always expression is best used at the step level or on tasks that you expect to run even when a job is canceled. For example, you can use always to send logs even when a job is canceled.

### `cancelled`

Returns `true` if the workflow was canceled.

### `failure`

Returns true when any previous step of a job fails. If you have a chain of dependent jobs, failure() returns true if any ancestor job fails.

```yml
steps:
  ...
  - name: The job has failed
    if: ${{ failure() }}
```

#### failure with conditions

You can include extra conditions for a step to run after a failure, but you must still include failure() to override the default status check of success() that is automatically applied to if conditions that don't contain a status check function.

```yml
steps:
  ...
  - name: Failing step
    id: demo
    run: exit 1
  - name: The demo step has failed
    if: ${{ failure() && steps.demo.conclusion == 'failure' }}
```
