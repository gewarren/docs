---
title: "Breaking change: Environment variable prefixes"
description: Learn about the .NET 7 breaking change in .NET extensions where environment variables prefixed with `__` are handled differently.
ms.date: 10/11/2022
---
# Environment variable prefixes



## Version introduced

.NET 7 Preview 4

## Previous behavior



## New behavior



## Type of breaking change

This change can affect [source compatibility](../../categories.md#source-compatibility).

## Reason for change

This change fixes a breaking change that was inadvertently introduced in .NET 6. The new behavior matches the .NET 5 behavior.

## Recommended action



## Affected APIs

- [Microsoft.Extensions.Configuration.EnvironmentVariables package APIs](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables)
