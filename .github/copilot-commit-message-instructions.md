- Use Conventional Commits specification.

# Structure

```
<type>[optional scope]: <description>
{blank line}
[body]
```

# Elements:

1. **fix:** a commit of the *type* `fix` patches a bug in your codebase.
2. **feat:** a commit of the *type* `feat` introduces a new feature to the codebase.
3. **BREAKING CHANGE:** appends a `!` after the type, introduces a breaking change.
4. *types* other than `fix` and `feat` are allowed, recommends `build`, `chore`, `docs`, `style`, `refactor`, `test`, and others.
5. Use zh-TW to write `<description>` and `[body]`.
6. Keep technical term and symbols in en-US.
7. Use list format for `[body]`, each line starting with `- `.