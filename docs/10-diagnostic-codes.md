# Diagnostic Codes

PhpThunder reports static analysis findings as **diagnostic names** — short PascalCase identifiers shown in the Problems panel, hover tooltips, and code actions. This page lists every diagnostic, grouped by category, so you can look up what a diagnostic means and why it fires.

---

## Fatal errors

These are code PHP refuses to load, or that raises an `Error` as soon as the construct is evaluated. Each one is reported as an error at every analysis level, because the failure does not depend on a branching condition the analyser would have to follow — the file fails to compile, or the declaration itself throws when it is reached.

| Name                  | Meaning                                                                |
| --------------------- | ---------------------------------------------------------------------- |
| `CannotInstantiate`   | `new` on an interface, trait or enum                                   |
| `NonStaticCallStatic` | Instance method called statically with no `$this` in scope             |
| `AbstractInConcrete`  | `abstract` method in a class that is not abstract, or in an enum       |
| `DuplicateParameter`  | Two parameters share a name (`Redefinition of parameter $x`)           |
| `AssignThis`          | Assigning to, destructuring into, iterating into, or unsetting `$this` |
| `MemberOutsideClass`  | Property declared in an interface or an enum                           |
| `ExtendFinal`         | Class extends a `final` class                                          |
| `OverrideFinal`       | Method or constant overrides a `final` member                          |
| `FinalPrivateMember`  | `final private const` (private constants cannot be final)              |
| `InvalidMemberType`   | `void`/`never`/`callable`/`mixed` used illegally in a type position    |
| `ParentWithoutParent` | `parent::` in a class scope whose class has no parent                  |
| `UnhandledMatch`      | `match` with no `default` arm that cannot cover every subject value    |
| `EnumMagicMethod`     | Magic method an enum may not declare (`Enum E cannot include ...`)     |

Two deliberate non-reports keep this group free of false positives:

- `static::method()` is never flagged. Under late static binding the runtime class may redeclare the method as `static`, which is legal, so the call is not decidable statically.
- A non-static call made where a `$this` exists is not flagged. PHP binds `$this` and only rejects the call when the qualifier is outside the current lineage — that requires full hierarchy resolution, so phpthunder stays silent rather than risk a false positive.

`UnhandledMatch` is narrower than it may look. Missing a `default` arm only makes `UnhandledMatchError` _possible_, so the diagnostic is limited to what the declared types settle outright:

- the `match` has no arms at all, or
- the subject's type is an enum and the arms list enum cases for fewer than all of the cases that type can hold.

A match on a literal-valued subject such as `match($s)` with `$s` a `string` is **not** reported — phpthunder has no literal types, so deciding which values the arms miss would require narrowing and flow analysis. A match inside a `try` block is also skipped, because one of that block's `catch` clauses may handle the error and only PHP knows at runtime.

## Undefined symbols

| Name                | Meaning                                               |
| ------------------- | ----------------------------------------------------- |
| `UndefinedClass`    | Class does not exist in the project or vendor         |
| `UndefinedFunction` | Function is not defined                               |
| `UndefinedMethod`   | Method does not exist on the receiver                 |
| `UndefinedProperty` | Property does not exist on the receiver               |
| `UndefinedConstant` | Constant is not defined                               |
| `NewOnBuiltinType`  | `new` used on a builtin type (`new int`, `new float`) |
| `ReservedClassName` | Class name is a reserved word (case-insensitive)      |

## Undefined / unused variables

| Name                   | Meaning                                   |
| ---------------------- | ----------------------------------------- |
| `UndefinedVariable`    | Variable has not been assigned before use |
| `UnusedVariable`       | Variable is assigned but never read       |
| `UndefinedVariableMay` | Variable may be undefined (cross-file)    |
| `BranchUndefined`      | Variable is defined on some branches only |

## Type errors

| Name                 | Meaning                                           |
| -------------------- | ------------------------------------------------- |
| `TypeMismatch`       | Value does not match the expected type            |
| `ReturnInNever`      | Function with `never` return type returns a value |
| `ReturnValueInVoid`  | Function with `void` return type returns a value  |
| `MissingReturn`      | Non-void function is missing a return statement   |
| `ImplicitConversion` | Implicit scalar conversion (e.g. float to int)    |

## Unused code

| Name                   | Meaning                                      |
| ---------------------- | -------------------------------------------- |
| `UnusedImport`         | `use` import is never referenced             |
| `UnusedPrivateMember`  | Private property or method is never accessed |
| `UnusedTopLevelSymbol` | Top-level function or constant is never used |

## Control flow

| Name                       | Meaning                                                       |
| -------------------------- | ------------------------------------------------------------- |
| `UnreachableCode`          | Statement cannot be reached                                   |
| `SwitchFallthrough`        | Non-empty case falls through without `break`/`return`         |
| `BreakContinueIllegal`     | `break`/`continue` used outside a valid loop/switch           |
| `DuplicateSwitchCase`      | Duplicate `case` value in a `switch`                          |
| `DuplicateSwitchDefault`   | More than one `default` branch in a `switch`                  |
| `DuplicateMatchCond`       | Duplicate condition in a `match` expression                   |
| `DuplicateMatchDefault`    | More than one `default` branch in a `match` expression        |
| `SwitchCaseSeparator`      | Incorrect separator between case and its body                 |
| `GotoLabelUndefined`       | `goto` targets a label that does not exist                    |
| `GotoCrossesBoundary`      | `goto` crosses a function or class boundary                   |
| `ForeachVarReuseCF`        | Foreach value variable is reused (alias of `ForeachVarReuse`) |
| `TryWithoutCatchOrFinally` | `try` block has no `catch` or `finally` clause (PHP fatal)    |

## Array checks

| Name                  | Meaning                                            |
| --------------------- | -------------------------------------------------- |
| `ArrayShapeKey`       | Access to a key not declared in the array shape    |
| `DuplicateArrayKey`   | Duplicate key in an array literal                  |
| `IllegalArrayKeyType` | Array key type is not `int` or `string`            |
| `InvalidStringOffset` | Offset on a string is not an integer (PHP 8 fatal) |

## Generic / template checks

| Name              | Meaning                                          |
| ----------------- | ------------------------------------------------ |
| `GenericBounds`   | Generic type is out of its declared bounds       |
| `GenericVariance` | Generic type used in the wrong variance position |

## Hierarchy checks

| Name                           | Meaning                                              |
| ------------------------------ | ---------------------------------------------------- |
| `AbstractMethodNotImplemented` | Subclass does not implement an abstract method       |
| `MethodCompatibility`          | Overriding method is not compatible with the parent  |
| `InterfaceMethodVisibility`    | Interface method implemented with reduced visibility |
| `AbstractInstantiation`        | `new` used on an abstract class                      |
| `ReadonlyPropertyOutsideCtor`  | Readonly property assigned outside the constructor   |

## Deprecated / removed

| Name                    | Meaning                                           |
| ----------------------- | ------------------------------------------------- |
| `DeprecatedUsage`       | Symbol is marked `@deprecated`                    |
| `RemovedUsage`          | Symbol has been removed in the target PHP version |
| `VersionRequirement`    | Feature requires a different PHP version          |
| `DeprecatedDeclaration` | Declaration itself is deprecated                  |
| `DeprecatedSyntax`      | Language construct deprecated as of the target PHP version |

## Style

| Name                        | Meaning                                                 |
| --------------------------- | ------------------------------------------------------- |
| `NestedTernary`             | Nested ternary expressions are hard to read             |
| `AbstractStatic`            | `abstract static` is not valid                          |
| `AbstractPrivate`           | `abstract private` is not valid                         |
| `SelfStaticOutsideClass`    | `self::` / `static::` used outside a class              |
| `StaticMethodViaInstance`   | Static method called on an instance                     |
| `ClassNestedInClosure`      | Class declared inside a closure                         |
| `InterfaceNestedInFunction` | Interface declared inside a function                    |
| `TraitNestedInFunction`     | Trait declared inside a function                        |
| `EnumNestedInFunction`      | Enum declared inside a function                         |
| `MagicMethodStatic`         | Magic method declared `static`                          |
| `MagicMethodNonPublic`      | Magic method has reduced visibility                     |
| `DefineLeadingSlash`        | `define()` name starts with a leading slash             |
| `OldStyleConstructor`       | Constructor uses the old `ClassName()` syntax           |
| `OptionalBeforeRequired`    | Optional parameter before a required one                |
| `SelfAssignment`            | Variable assigned to itself                             |
| `ReadonlyPropertyWrite`     | Readonly property assigned after construction           |
| `ReservedFunctionName`      | Function name is a reserved word                        |
| `ConstAliasConflict`        | Constant alias conflicts with another definition        |
| `UseAliasConflict`          | `use` alias conflicts with another definition           |
| `UnknownNamedParam`         | Named argument does not match any parameter             |
| `NamedParamOverwrite`       | Named argument overwrites an already-assigned parameter |
| `PositionalAfterNamed`      | Positional argument after a named argument              |
| `MissingRequiredParams`     | Required parameter is not passed                        |
| `TooManyArgs`               | Too many arguments passed to a function/method          |
| `StringConcatWithPlus`      | String concatenation using `+` instead of `.`           |
| `ForeachByRefUnset`         | `foreach` by-reference variable not unset after loop    |
| `StrictTypesDeclare`        | `declare(strict_types=)` has incorrect form             |
| `EmptyBody`                 | Control structure has an empty body                     |
| `NamespacePlacement`        | Namespace declaration is not at the top of the file     |
| `ReadonlyPropertyDecl`      | Readonly property declaration has invalid form          |
| `ReadonlyPromotedNoType`    | Readonly promoted property has no type declaration      |
| `InstanceofRHS`             | `instanceof` right-hand side is not a valid class       |
| `InconsistentExits`         | Function has inconsistent return/exit paths             |
| `MagicMethodSignature`      | Magic method signature does not match the expected form |
| `ForeachVarReuse`           | Foreach value variable is reused in a nested loop       |
| `Psr4ClassName`             | Class name does not match PSR-4 autoloading rules       |
| `DynamicInclude`            | Dynamic include path cannot be resolved                 |

## Duplicate definitions

| Name                  | Meaning                          |
| --------------------- | -------------------------------- |
| `DuplicateDefinition` | Symbol is defined more than once |

## PHPDoc

| Name                       | Meaning                                           |
| -------------------------- | ------------------------------------------------- |
| `PHPDocOrphanParam`        | `@param` tag has no matching parameter            |
| `PHPDocMissingParam`       | Real parameter is missing a `@param` tag          |
| `PHPDocParamTypeMismatch`  | `@param` type does not match the PHP type hint    |
| `PHPDocReturnTypeMismatch` | `@return` type does not match the PHP return type |

## Purity

| Name                      | Meaning                                    |
| ------------------------- | ------------------------------------------ |
| `ImpureCallInPureContext` | Impure function called from a pure context |

## Taint analysis

| Name                 | Meaning                                             |
| -------------------- | --------------------------------------------------- |
| `TaintedDataInSink`  | Tainted data flows into a security-sensitive sink   |
| `TaintedDataEscaped` | Tainted data was escaped before use (informational) |
| `TaintSourceToSink`  | Direct source-to-sink taint flow                    |

## Composer

| Name                       | Meaning                                       |
| -------------------------- | --------------------------------------------- |
| `ComposerInvalidJSON`      | Invalid JSON syntax in `composer.json`        |
| `ComposerInvalidVersion`   | Invalid version constraint in `composer.json` |
| `ComposerAbandonedPackage` | Installed package is marked abandoned         |

## Feature version

| Name             | Meaning                                           |
| ---------------- | ------------------------------------------------- |
| `FeatureVersion` | Language feature requires a different PHP version |

---

## Suppressing diagnostics

You can suppress a diagnostic on the line where it fires or the line before:

```php
// noinspection UndefinedClass
$x = new NonExistentClass();
```

PhpThunder respects `@phpstan-ignore-next-line`, `@psalm-suppress`, and `// noinspection` comments and PhpStan, Psalm and PhpStorm diagnostic codes. See the [project configuration](07-project-configuration.md) doc for workspace-level severity overrides.
