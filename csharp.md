# C# style guide

Use this guide together with [the C# EditorConfig profile](.editorconfig). It records the readability decisions that formatters cannot fully enforce.

## Braces and layout

- Always use braces for `if`, `else`, `for`, `foreach`, `while`, `do`, and other statements with an optional block body, including a single statement. Conventional `else if` chains are allowed.
- Put opening and closing braces on their own lines (Allman style). Do not collapse methods or control-flow blocks onto one line.
- Indent with four spaces. Use UTF-8, LF line endings, a final newline, and no trailing whitespace.
- Put each statement and declaration on its own line. Normal `for` headers are an exception.
- Use one blank line between logical groups: validation, preparation, work, and the result. Keep closely related assignments or calls together.
- Put one blank line before a new control-flow block and after a completed block when another statement follows at the same scope. Keep `else`, `catch`, `finally`, and the trailing `while` of a `do` loop attached to their construct.
- Put one blank line between methods, constructors, properties, and nested type definitions. Related fields can stay together.
- Do not add blank lines immediately inside braces, between consecutive closing braces, or between every statement. Avoid repeated blank lines.
- Break long expressions and argument lists when they become hard to scan. Aim for 120 characters; this is a readability guideline, not an enforced limit. Do not split URLs or string content merely to meet it.

```csharp
using System;
using System.Collections.Generic;

namespace Example;

public sealed class NameCollector
{
    public static List<string> Collect(IEnumerable<string> names)
    {
        ArgumentNullException.ThrowIfNull(names);

        var results = new List<string>();

        foreach (string name in names)
        {
            if (string.IsNullOrWhiteSpace(name))
            {
                continue;
            }

            string trimmedName = name.Trim();
            results.Add(trimmedName);
        }

        return results;
    }
}
```

## Types and expressions

- Use `var` when the initializer makes the type obvious, such as `var names = new List<string>();`. Use explicit types for built-in types and unclear return types, such as `int count = 0;` and `Customer customer = LoadCustomer();`. Anonymous types require `var`.
- Prefer C# type keywords such as `string`, `int`, and `bool` over framework names such as `String`, `Int32`, and `Boolean`.
- Allow expression bodies for simple properties and lambdas that fit on one line. Use block bodies for methods, constructors, operators, indexers, accessors, and local functions. Auto-properties such as `public string Name { get; init; }` remain appropriate.
- Prefer clear guard clauses to deep nesting. Avoid nested ternaries and expressions that combine unrelated work.
- Use modern language features when they improve clarity and the consuming project supports them. Do not rewrite code solely to use a newer syntax.

## Organization and naming

- Use file-scoped namespaces in C# 10+ projects. Keep `using` directives outside the namespace, with `System` directives first and no blank lines between import groups.
- Use `PascalCase` for namespaces, types, methods, properties, events, local functions, and constants. Prefix interfaces with `I` and type parameters with `T` (for example, `IRepository` and `TItem`).
- Use `camelCase` for parameters and local variables, `_camelCase` for private instance fields, and `s_camelCase` for private static fields. Constants use `PascalCase` regardless of accessibility.
- Use descriptive names. Give asynchronous methods an `Async` suffix unless implementing an existing contract with a different name.
- Write accessibility modifiers explicitly for non-interface members. Make fields `readonly` when they are only assigned during initialization.
- Keep comments focused on intent and constraints. Use XML documentation when it helps explain a public contract; avoid comments that merely repeat the code.

## General practices

- Respect nullable reference types and handle possible null values explicitly. Enable nullable analysis when setting up a new project.
- Use `async`/`await` throughout asynchronous paths. Avoid `.Result`, `.Wait()`, and `async void` except event handlers. Accept and propagate cancellation where appropriate.
- Dispose owned resources with `using` or `await using`; declaration form is fine when lifetime should extend to the end of the scope.
- Catch exceptions only when you can handle them or add useful context. Preserve the stack with `throw;` when rethrowing.
- Favor straightforward code and focused methods. Keep changes scoped to the task, and follow existing project contracts and supported language versions.

## Enforcement limits

Use [Curb](https://github.com/nullean/curb) for routine C# formatting and checks. Follow the [setup instructions](README.md#format-with-curb). Keep Roslyn build analysis enabled for semantic rules.

The profile uses built-in Roslyn rules; it does not require StyleCop, ReSharper, or another analyzer package. Missing control-flow braces are errors. Other configured style violations are warnings.

Roslyn's experimental `IDE2003` rule checks blank lines after blocks. It cannot identify logical groups or require all the member spacing described above. Agents and reviewers must apply those parts of this guide. Experimental rules may vary by SDK or editor and are not a complete build-time spacing check.

File-scoped namespaces are a default for modern projects, not a request to raise a project's language version. Override that preference for projects using C# 9 or earlier.

See [Microsoft's C# conventions](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/inside-a-program/coding-conventions), [braces rule](https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/style-rules/ide0011), [formatting options](https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/style-rules/csharp-formatting-options), and [experimental blank-line rule](https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/style-rules/ide2003) for the underlying tooling.
