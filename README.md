# Shared coding styles

Reusable language styling rules for repositories and coding agents.

## Available profiles

| Language | Tooling | Agent guidance |
| --- | --- | --- |
| C# | [.editorconfig](.editorconfig) | [C# style guide](csharp.md) |

The current `.editorconfig` scopes all settings to `*.cs` files. Other languages are unaffected.

## Reference from another repository

Add the following to the consuming repository's `AGENTS.md` or equivalent agent instructions:

```markdown
For C# changes, read and follow both:
- https://github.com/thomhurst/editorconfig/blob/main/.editorconfig
- https://github.com/thomhurst/editorconfig/blob/main/csharp.md

Always use braces for control-flow bodies, including single statements.
Separate logical groups with one blank line; keep related statements together.
Apply these rules to code you add or change. Avoid unrelated formatting changes.
Preserve this repository's supported language version and explicit local overrides.
If these references are unavailable, report that and use the repository's local guidance.
```

The links become available once these files are pushed. Links to `main` track future changes; replace `main` with a commit SHA when a repository needs a fixed version.

## Apply editor and build rules

EditorConfig does not import remote profiles. To enforce the rules locally, copy `.editorconfig` to the consuming repository's root, or merge its `[*.cs]` section into an existing file. Preserve existing settings for other languages and any deliberate local overrides. `root = true` stops discovery of parent EditorConfig files; retain or omit it according to the consuming repository's existing hierarchy.

For SDK-style projects, enable style analysis during builds in the project's `.csproj` or an existing `Directory.Build.props`:

```xml
<Project>
  <PropertyGroup>
    <EnforceCodeStyleInBuild>true</EnforceCodeStyleInBuild>
  </PropertyGroup>
</Project>
```

If adding the property to a `.csproj`, place it inside that file's existing `<Project>` element. See [Microsoft's code analysis overview](https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/overview#code-style-analysis) for build enforcement.

Run these commands from the consuming repository, supplying its project or solution path:

```shell
dotnet format path/to/YourSolution.sln
dotnet format path/to/YourSolution.sln --verify-no-changes
dotnet build path/to/YourSolution.sln
```

The first command applies supported fixes; the second checks for remaining formatting changes. Builds with style analysis enabled reject missing braces (`IDE0011`). Other configured rules produce warnings where the SDK supports build-time enforcement. Logical grouping still requires agent or human review; see the [guide's enforcement limits](csharp.md#enforcement-limits).

See [`dotnet format` documentation](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-format) for command options. This repository contains profiles and documentation, so run .NET build and format commands in a consuming C# repository.
