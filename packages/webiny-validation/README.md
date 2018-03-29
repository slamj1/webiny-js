# Validation

A simple and pluginable data validation library, packed with frequently used validators like `required`, `email`, `url`, `creditCard` etc.

## Documentation

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

#### Table of Contents

-   [Installation](#installation)
-   [Get Started](#get-started)
-   [Classes](#classes)
    -   [Validation](#validation)
        -   [setValidator](#setvalidator)
        -   [getValidator](#getvalidator)
        -   [validate](#validate)
        -   [validateSync](#validatesync)
-   [Flow Types](#flow-types)
-   [ValidationError](#validationerror)
-   [creditCard](#creditcard)
-   [email](#email)
-   [eq](#eq)
-   [gt](#gt)
-   [gte](#gte)
-   [in](#in)
-   [number](#number)
-   [Validator](#validator)
-   [ValidateOptions](#validateoptions)

### Installation

`yarn add webiny-validation`


### Get Started

#### Simple example

Validation of data is performed asynchronously by default, like in the following example:

    import { validation } from 'webiny-validation';

    validation.validate(123, 'required,number,gt:20').then(() => {
        // Value is valid
    }).catch(e => {
        // Value is invalid
    });

Validators are specified by its name and in this case, there are three - `required`, `number` and `gt`.
Since value is not empty, it is a number and is greater than 20, code execution will proceed regularly. 
On the other hand, if one of the validator was not satisfied, an instance of `ValidationError` would be thrown.

    import { validation } from 'webiny-validation';

    validation.validate(10, 'required,number,gt:20').catch(e => {
        // Throws an Error with message "Value needs to be greater than 20.".
    });

It is possible to provide as many validators as needed.

#### Passing arguments

As seen in previous section, validators can accept additional arguments, which are divided by colon.
The following validator simply states that value must be greater than 20:

    gt:20

Some validators may even accept more than one arguments:

    in:dog:cat:fish:bird

There is no limit on the number of passed arguments.

#### Built-in validators

The following is a complete list of built-in validators, ready to be used immediately:

-   `creditCard`
-   `email`
-   `eq`
-   `gt`
-   `gte`
-   `in`
-   `integer`
-   `lt`
-   `lte`
-   `maxLength`
-   `minLength`
-   `number`
-   `password`
-   `phone`
-   `required`
-   `url`

More information about each can be found in the following sections.

#### Adding custom validators

Apart from built-in validators, there are cases where additional validators might be needed. The following example 
shows how to add a custom validator:

    import { validation, ValidationError } from 'webiny-validation';

    validation.setValidator('gender', value => {
      if (['male', 'female'].includes(value)) {
          return;
      }
      throw new ValidationError('Value needs to be "male" or "female".);
    });

    // Throws an instance of ValidationError error.
    await validation.validate('none', 'gender');

#### Synchronous validation

As mentioned, validation by default is performed asynchronously. 
But if more suitable, it can also be performed synchronously:

    import { validation } from 'webiny-validation';

    // Throws an instance of ValidationError error.
    validation.validateSync('parrot', 'in:cat:dog:fish:parrot');

    // Success.
    validation.validateSync('fish', 'in:cat:dog:fish:parrot');

#### Returning instead of throwing

The following example shows how to force ValidationError to be returned, instead of thrown:

    import { validation } from 'webiny-validation';
    const error = await validation.validate("", "required", { throw: false });

Once executed, an instance of ValidationError will be assigned to the `error` constant.


### Classes




#### Validation

[packages-utils/webiny-validation/src/validation.js:23-148](https://github.com/webiny/webiny-js/blob/d48d320d653854e2ae18bc85dde4cb083d93d593/packages-utils/webiny-validation/src/validation.js#L23-L148 "Source code on GitHub")

Main class of Validation library.
Exported as a singleton instance, it offers methods for sync/async data validation and overwriting or adding new validators.

**Examples**

```javascript
import { validation } from 'webiny-validation';

// `validation` is a preconfigured instance of Validation class.
// From here you can either add new validators or use it as-is.
```

##### setValidator

[packages-utils/webiny-validation/src/validation.js:40-43](https://github.com/webiny/webiny-js/blob/d48d320d653854e2ae18bc85dde4cb083d93d593/packages-utils/webiny-validation/src/validation.js#L40-L43 "Source code on GitHub")

Add new validator.

**Parameters**

-   `name` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Validator name.
-   `callable` **[Validator](#validator)** Validator function which throws a ValidationError if validation fails.

Returns **[Validation](#validation)** 

##### getValidator

[packages-utils/webiny-validation/src/validation.js:50-55](https://github.com/webiny/webiny-js/blob/d48d320d653854e2ae18bc85dde4cb083d93d593/packages-utils/webiny-validation/src/validation.js#L50-L55 "Source code on GitHub")

Get validator function by name.

**Parameters**

-   `name` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Validator name.

Returns **[Validator](#validator)** A validator function.

##### validate

[packages-utils/webiny-validation/src/validation.js:64-92](https://github.com/webiny/webiny-js/blob/d48d320d653854e2ae18bc85dde4cb083d93d593/packages-utils/webiny-validation/src/validation.js#L64-L92 "Source code on GitHub")

Asynchronously validates value.

**Parameters**

-   `value` **any** Value to validate.
-   `validators` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** A list of comma-separated validators (eg. required,number,gt:20).
-   `options` **[ValidateOptions](#validateoptions)** Validation options.

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;([boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean) \| [ValidationError](#validationerror))>** 

##### validateSync

[packages-utils/webiny-validation/src/validation.js:101-129](https://github.com/webiny/webiny-js/blob/d48d320d653854e2ae18bc85dde4cb083d93d593/packages-utils/webiny-validation/src/validation.js#L101-L129 "Source code on GitHub")

Synchronously validates value.

**Parameters**

-   `value` **any** Value to validate.
-   `validators` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** A list of comma-separated validators (eg. required,number,gt:20).
-   `options` **[ValidateOptions](#validateoptions)** Validation options.

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;([boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean) \| [ValidationError](#validationerror))>** 

### Flow Types




### ValidationError

[packages-utils/webiny-validation/src/validationError.js:10-21](https://github.com/webiny/webiny-js/blob/d48d320d653854e2ae18bc85dde4cb083d93d593/packages-utils/webiny-validation/src/validationError.js#L10-L21 "Source code on GitHub")

This class is used by validators to throw an error when value validation fails.

**Parameters**

-   `message` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Error message
-   `validator` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Validator that triggered this error
-   `value` **any** Value that triggered this error

### creditCard

[packages-utils/webiny-validation/src/validators/creditCard.js:17-54](https://github.com/webiny/webiny-js/blob/d48d320d653854e2ae18bc85dde4cb083d93d593/packages-utils/webiny-validation/src/validators/creditCard.js#L17-L54 "Source code on GitHub")

Credit card validator. This validator will check if the given value is a credit card number.

**Parameters**

-   `value` **any** This is the value that will be validated.

**Examples**

```javascript
import { validation } from 'webiny-validation';
validation.validate('notACreditCard', 'creditCard').then(() => {
 // Valid
}).catch(e => {
 // Invalid
});
```

-   Throws **[ValidationError](#validationerror)** 

### email

[packages-utils/webiny-validation/src/validators/email.js:17-27](https://github.com/webiny/webiny-js/blob/d48d320d653854e2ae18bc85dde4cb083d93d593/packages-utils/webiny-validation/src/validators/email.js#L17-L27 "Source code on GitHub")

Email validator. This validator checks if the given value is a valid email address.

**Parameters**

-   `value` **any** This is the value that will be validated.

**Examples**

```javascript
import { validation } from 'webiny-validation';
validation.validate('email@gmail.com', 'email').then(() => {
 // Valid
}).catch(e => {
 // Invalid
});
```

-   Throws **[ValidationError](#validationerror)** 

### eq

[packages-utils/webiny-validation/src/validators/eq.js:18-27](https://github.com/webiny/webiny-js/blob/d48d320d653854e2ae18bc85dde4cb083d93d593/packages-utils/webiny-validation/src/validators/eq.js#L18-L27 "Source code on GitHub")

Equality validator. This validator checks if the given values are equal.

**Parameters**

-   `value` **any** This is the value that will be validated.
-   `params` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>** This is the value to validate against. It is passed as a validator parameter: `eq:valueToCompareWith`

**Examples**

```javascript
import { validation } from 'webiny-validation';
validation.validate('email@gmail.com', 'eq:another@gmail.com').then(() => {
 // Valid
}).catch(e => {
 // Invalid
});
```

-   Throws **[ValidationError](#validationerror)** 

### gt

[packages-utils/webiny-validation/src/validators/gt.js:18-25](https://github.com/webiny/webiny-js/blob/d48d320d653854e2ae18bc85dde4cb083d93d593/packages-utils/webiny-validation/src/validators/gt.js#L18-L25 "Source code on GitHub")

"Greater than" validator. This validator checks if the given values is greater than the `min` value.

**Parameters**

-   `value` **any** This is the value that will be validated.
-   `params` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>** This is the value to validate against. It is passed as a validator parameter: `gt:valueToCompareAgainst`

**Examples**

```javascript
import { validation } from 'webiny-validation';
validation.validate(10, 'gt:100').then(() => {
 // Valid
}).catch(e => {
 // Invalid
});
```

-   Throws **[ValidationError](#validationerror)** 

### gte

[packages-utils/webiny-validation/src/validators/gte.js:18-25](https://github.com/webiny/webiny-js/blob/d48d320d653854e2ae18bc85dde4cb083d93d593/packages-utils/webiny-validation/src/validators/gte.js#L18-L25 "Source code on GitHub")

"Greater than or equals" validator. This validator checks if the given values is greater than or equal to the `min` value.

**Parameters**

-   `value` **any** This is the value that will be validated.
-   `min` **any** This is the value to validate against. It is passed as a validator parameter: `gte:valueToCompareAgainst`

**Examples**

```javascript
import { validation } from 'webiny-validation';
validation.validate(10, 'gte:100').then(() => {
 // Valid
}).catch(e => {
 // Invalid
});
```

-   Throws **[ValidationError](#validationerror)** 

### in

[packages-utils/webiny-validation/src/validators/in.js:18-27](https://github.com/webiny/webiny-js/blob/d48d320d653854e2ae18bc85dde4cb083d93d593/packages-utils/webiny-validation/src/validators/in.js#L18-L27 "Source code on GitHub")

"In array" validator. This validator checks if the given values is greater than or equal to the `min` value.

**Parameters**

-   `value` **any** This is the value that will be validated.
-   `params` **any** This is the array of values to search in. It is passed as a validator parameter: `in:1:2:3`. Array values are separated by `:`.

**Examples**

```javascript
import { validation } from 'webiny-validation';
validation.validate(10, 'in:10:20:30').then(() => {
 // Valid
}).catch(e => {
 // Invalid
});
```

-   Throws **[ValidationError](#validationerror)** 

### number

[packages-utils/webiny-validation/src/validators/number.js:11-19](https://github.com/webiny/webiny-js/blob/d48d320d653854e2ae18bc85dde4cb083d93d593/packages-utils/webiny-validation/src/validators/number.js#L11-L19 "Source code on GitHub")

This validator checks of the given value is a number

**Parameters**

-   `value` **any** 

Returns **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** 

### Validator

[packages-utils/webiny-validation/types.js:9-9](https://github.com/webiny/webiny-js/blob/d48d320d653854e2ae18bc85dde4cb083d93d593/packages-utils/webiny-validation/types.js#L9-L9 "Source code on GitHub")

This type defines the validator function.

**Parameters**

-   `value` **any** This is the value being validated.
-   `parameters` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>** (Optional) This represents an array validator parameters.


-   Throws **[ValidationError](#validationerror)** 

### ValidateOptions

[packages-utils/webiny-validation/types.js:17-19](https://github.com/webiny/webiny-js/blob/d48d320d653854e2ae18bc85dde4cb083d93d593/packages-utils/webiny-validation/types.js#L17-L19 "Source code on GitHub")

This is an object containing validation options.

**Properties**

-   `throw` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Should validation throw on failure? Default: true.