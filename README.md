<p align="center"><a href="https://valkyrja.io" target="_blank">
    <img src="https://raw.githubusercontent.com/valkyrjaio/art/refs/heads/master/full-logo/orange/php.png" width="400">
</a></p>

# Valkyrja PHPArkitect

PHPArkitect architectural rules and custom expressions for the Valkyrja project.

<p>
    <a href="https://packagist.org/packages/valkyrja/phparkitect"><img src="https://poser.pugx.org/valkyrja/phparkitect/require/php" alt="PHP Version Require"></a>
    <a href="https://packagist.org/packages/valkyrja/phparkitect"><img src="https://poser.pugx.org/valkyrja/phparkitect/v" alt="Latest Stable Version"></a>
    <a href="https://packagist.org/packages/valkyrja/phparkitect"><img src="https://poser.pugx.org/valkyrja/phparkitect/license" alt="License"></a>
    <!-- <a href="https://packagist.org/packages/valkyrja/phparkitect"><img src="https://poser.pugx.org/valkyrja/phparkitect/downloads" alt="Total Downloads"></a>-->
    <a href="https://scrutinizer-ci.com/g/valkyrjaio/phparkitect/?branch=master"><img src="https://scrutinizer-ci.com/g/valkyrjaio/phparkitect/badges/quality-score.png?b=master" alt="Scrutinizer"></a>
    <a href="https://coveralls.io/github/valkyrjaio/phparkitect?branch=master"><img src="https://coveralls.io/repos/github/valkyrjaio/phparkitect/badge.svg?branch=master" alt="Coverage Status" /></a>
    <a href="https://shepherd.dev/github/valkyrjaio/phparkitect"><img src="https://shepherd.dev/github/valkyrjaio/phparkitect/coverage.svg" alt="Psalm Shepherd" /></a>
    <a href="https://sonarcloud.io/summary/new_code?id=valkyrjaio_phparkitect"><img src="https://sonarcloud.io/api/project_badges/measure?project=valkyrjaio_phparkitect&metric=sqale_rating" alt="Maintainability Rating" /></a>
</p>

Build Status
------------

<table>
    <tbody>
        <tr>
            <td>Linting</td>
            <td>
                <a href="https://github.com/valkyrjaio/phparkitect/actions/workflows/phpcodesniffer.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phparkitect/actions/workflows/phpcodesniffer.yml/badge.svg?branch=master" alt="PHP Code Sniffer Build Status"></a>
            </td>
            <td>
                <a href="https://github.com/valkyrjaio/phparkitect/actions/workflows/phpcsfixer.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phparkitect/actions/workflows/phpcsfixer.yml/badge.svg?branch=master" alt="PHP CS Fixer Build Status"></a>
            </td>
        </tr>
        <tr>
            <td>Coding Rules</td>
            <td>
                <a href="https://github.com/valkyrjaio/phparkitect/actions/workflows/phparkitect.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phparkitect/actions/workflows/phparkitect.yml/badge.svg?branch=master" alt="PHPArkitect Build Status"></a>
            </td>
            <td>
                <a href="https://github.com/valkyrjaio/phparkitect/actions/workflows/rector.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phparkitect/actions/workflows/rector.yml/badge.svg?branch=master" alt="Rector Build Status"></a>
            </td>
        </tr>
        <tr>
            <td>Static Analysis</td>
            <td>
                <a href="https://github.com/valkyrjaio/phparkitect/actions/workflows/phpstan.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phparkitect/actions/workflows/phpstan.yml/badge.svg?branch=master" alt="PHPStan Build Status"></a>
            </td>
            <td>
                <a href="https://github.com/valkyrjaio/phparkitect/actions/workflows/psalm.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phparkitect/actions/workflows/psalm.yml/badge.svg?branch=master" alt="Psalm Build Status"></a>
            </td>
        </tr>
        <tr>
            <td>Testing</td>
            <td>
                <a href="https://github.com/valkyrjaio/phparkitect/actions/workflows/phpunit.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phparkitect/actions/workflows/phpunit.yml/badge.svg?branch=master" alt="PHPUnit Build Status"></a>
            </td>
            <td></td>
        </tr>
    </tbody>
</table>

## Overview

This repository contains two things:

1. **`Valkyrja\Arkitect\Rules`** — a reusable rule set that encodes the naming
   and namespace conventions used across the Valkyrja project. Drop it into any
   PHPArkitect config to enforce those conventions on your own code.

2. **Custom expressions** — PHPArkitect `Expression` implementations that fill
   gaps in the built-in expression library.

## Installation

```bash
composer require valkyrja/phparkitect
```

## Usage

Reference `Rules::getRules()` from your `phparkitect.php` configuration file:

```php
// phparkitect.php
use Valkyrja\Arkitect\Rules;

return Rules::getRules();
```

`getRules()` returns a closure that receives a PHPArkitect `Config` object and
registers all rules against two class sets:

- `src/` — source rules (naming, namespaces, architectural constraints)
- `tests/` — test rules (finality, naming conventions for test helpers)

The paths are resolved relative to the directory **three levels above**
`src/Arkitect/Rules.php` (i.e. the project root), matching the standard
Valkyrja monorepo layout.

Run PHPArkitect as normal:

```bash
vendor/bin/phparkitect check
```

## Rules Reference

### Source Rules

#### No `@author` docblocks

All classes must not contain an `@author` docblock annotation.

#### Attributes

| Rule                                                                | Constraint                                   |
|---------------------------------------------------------------------|----------------------------------------------|
| Classes with `#[Attribute]` must reside in `*Attribute\` namespaces | Attributes belong in dedicated namespaces    |
| Classes in `*Attribute\` namespaces must carry `#[Attribute]`       | Only attributes live in attribute namespaces |
| Classes outside `*Attribute\*` must not carry `#[Attribute]`        | Non-attributes must not be attributed        |

#### Constants

| Rule                                                                              | Constraint           |
|-----------------------------------------------------------------------------------|----------------------|
| Final classes (excluding `*Security` and `*Provider`) must reside in `*Constant\` | Constants namespace  |
| Classes in `*Constant\` must be final                                             | Constants are sealed |

#### Providers

| Type                        | Naming               | Namespace    |
|-----------------------------|----------------------|--------------|
| `ServiceProviderContract`   | `*ServiceProvider`   | `*Provider\` |
| `ComponentProviderContract` | `*ComponentProvider` | `*Provider\` |
| `CliRouteProviderContract`  | `*RouteProvider`     | `*Provider\` |
| `ListenerProviderContract`  | `*ListenerProvider`  | `*Provider\` |
| `HttpRouteProviderContract` | `*RouteProvider`     | `*Provider\` |
| All classes in `*Provider\` | `*Provider` suffix   | —            |

#### Factories

| Rule                                                | Constraint                                |
|-----------------------------------------------------|-------------------------------------------|
| Classes named `*Factory` must reside in `*Factory\` | Factories belong in factory namespaces    |
| Classes in `*Factory\` must be named `*Factory`     | Only factories live in factory namespaces |

#### CLI Commands

Classes named `*Command` (outside `Valkyrja\Cli\*`, and not `*Handler` or
`*Middleware`) must reside in `*Cli\Command\` namespaces.

#### Security

Classes named `*Security` must reside in `*Security\` namespaces and must be
final.

#### Exceptions and Throwables

| Rule                                                                                  | Constraint                                       |
|---------------------------------------------------------------------------------------|--------------------------------------------------|
| Classes named `*Exception` must reside in `*Exception\`                               | Exceptions namespace                             |
| Classes extending `Throwable` must reside in `*Throwable\`                            | Throwable objects belong in throwable namespaces |
| Classes implementing `Throwable` must be named `*Exception`                           | Throwables are exceptions                        |
| Classes named `*RuntimeException` must reside in `*Abstract\` and be abstract         | Base runtime exceptions are abstract             |
| Classes named `*InvalidArgumentException` must reside in `*Abstract\` and be abstract | Base invalid argument exceptions are abstract    |

#### Types, Models, and Entities

| Rule                                                                                        | Constraint         |
|---------------------------------------------------------------------------------------------|--------------------|
| Classes extending `Type` (outside Config, Entity, Model namespaces) must reside in `*Type\` | Types namespace    |
| Classes extending `Model` (outside Config, Entity namespaces) must reside in `*Model\`      | Models namespace   |
| Classes extending `Entity` must reside in `*Entity\`                                        | Entities namespace |

#### Interfaces (Contracts)

| Rule                                                       | Constraint                    |
|------------------------------------------------------------|-------------------------------|
| Interfaces must reside in `*Contract\` namespaces          | Contracts namespace           |
| Classes in `*Contract\` must be interfaces                 | Only interfaces are contracts |
| Interfaces (except `*Throwable`) must be named `*Contract` | Contract naming convention    |
| Classes named `*Contract` must be interfaces               | Naming implies interface      |

#### Traits

| Rule                                       | Constraint                           |
|--------------------------------------------|--------------------------------------|
| Traits must reside in `*Trait\` namespaces | Traits namespace                     |
| Classes in `*Trait\` must be traits        | Only traits live in trait namespaces |
| Traits must not be named `*Trait*`         | No "Trait" in the name               |
| All classes must not be named `*Trait*`    | No "Trait" in any name               |

#### Abstract Classes

| Rule                                                                                                         | Constraint                       |
|--------------------------------------------------------------------------------------------------------------|----------------------------------|
| Abstract classes (outside `*Factory` and specific routing controller namespaces) must reside in `*Abstract\` | Abstract namespace               |
| Classes in `*Abstract\` must be abstract                                                                     | Only abstract classes live there |
| Abstract classes must not be named `*Abstract*`                                                              | No "Abstract" in the name        |
| Non-abstract classes must not be named `*Abstract*`                                                          | No "Abstract" in any name        |

#### Enums

| Rule                                                                                                 | Constraint                         |
|------------------------------------------------------------------------------------------------------|------------------------------------|
| Enums must reside in `*Enum\` namespaces                                                             | Enums namespace                    |
| Classes in `*Enum\` (outside `Valkyrja\Type\Enum`) must be enums                                     | Only enums live in enum namespaces |
| Enums must not be named `*Enum*`                                                                     | No "Enum" in enum names            |
| Non-enums (outside `Valkyrja\Type\Enum`, excluding `EnhancedEnumSupport`) must not be named `*Enum*` | No "Enum" in non-enum names        |

### Test Rules

| Rule                                                                                                                                  | Constraint                         |
|---------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| All classes must not contain `@author` docblocks                                                                                      | Same as src                        |
| Test classes (in `*Tests\`, not `*TestCase`, `*AttributeClass`, `*EnvClass`, non-abstract, non-trait, outside `Vendor`) must be final | Tests are sealed                   |
| Classes in `*Classes\` (non-enum, non-trait) must be named `*Class`                                                                   | Test helper class naming           |
| Classes in `*Classes\` must not be named `*Test`                                                                                      | Test helpers are not tests         |
| Classes not named `*Test` must not reside in `*Unit\` or `*Functional\`                                                               | Only tests live in test namespaces |
| Traits (not `TestCase`) in tests must reside in `*Trait\` and be named `*Trait`                                                       | Test trait conventions             |

## Custom Expressions

### `NotHaveAttribute`

`Valkyrja\Arkitect\Expression\ForClasses\NotHaveAttribute`

The inverse of PHPArkitect's built-in `HaveAttribute`. Evaluates to a violation
when the target class **does** carry the specified PHP attribute.

```php
use Valkyrja\Arkitect\Expression\ForClasses\NotHaveAttribute;

Rule::allClasses()
    ->that(new NotResideInTheseNamespaces('*Attribute\\*'))
    ->should(new NotHaveAttribute(Attribute::class))
    ->because('Non-attribute classes must not carry #[Attribute]');
```

**Constructor:**

```php
new NotHaveAttribute(string $attribute)
```

| Parameter    | Description                                              |
|--------------|----------------------------------------------------------|
| `$attribute` | Fully qualified class name of the PHP attribute to check |

**Violation message format:**

```
<FQCN> should not have the attribute <attribute> because <reason>
```
