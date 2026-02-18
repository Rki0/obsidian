# Create CLAUDE.md
You can create your project summary like `README.md` with the command below.
```shell
/init
```
# Separate public and local CLAUDE file
If you don't want to push your `~.md` file, you can use `~.local.md`.
1. Create `~.local.md`
2. Add that file into the `.gitignore`
# If you want to skip permission question
Claude code will ask you about the permission to update your code every time(every session).
If you want to skip this, you can get it with the command below.
```shell
claude --dangerously-skip-permissions
```
