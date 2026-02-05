---
name: pnpm-link-local
description: Link a local package using pnpm for development testing. Use when the user wants to test local changes to a dependency, link a local package, use a local version of a library, link charts, or mentions pnpm link.
---

# Link Local Package with pnpm

## Package Aliases

When the user refers to a package by shorthand, use the full package name:

| Shorthand | Package              |
| --------- | -------------------- |
| charts    | `@automattic/charts` |

## Workflow

### Step 1: Link into the consuming project

Assume that the user has already built and linked the local package to be linked, into the pnpm global location.

In the **consuming project directory** (where you want to use the local version):

```bash
pnpm link <package-name>
```

Example for `@automattic/charts`:

```bash
pnpm link @automattic/charts
```

### Step 2: Verify the link

Check that the symlink is in place:

```bash
ls -la node_modules/@automattic/charts
```

Should show a symlink pointing to your local package directory.

## Unlinking

When you're done testing and want to restore the published version:

```bash
pnpm unlink <package-name>
pnpm install
```

Example:

```bash
pnpm unlink @automattic/charts
pnpm install
```

## Monorepo Considerations - VERY IMPORTANT!

In a pnpm workspace monorepo, you may need to link at the workspace root or in a specific package directory depending on where the dependency is declared.

If the user specifies a package name, eg. `widgets-toolkit`, add the link there.

```bash
cd packages/widgets-toolkit
pnpm link @automattic/charts
```

## Troubleshooting

**Changes not reflected?**

- Rebuild the local package after making changes
- Some bundlers cache aggressively—restart the dev server

**TypeScript not finding types?**

- Ensure the local package has been built (including type declarations)
- Check that `types` or `typings` field in package.json points to built `.d.ts` files

**Peer dependency warnings?**

- Local linking may surface peer dependency mismatches—usually safe to ignore during development
