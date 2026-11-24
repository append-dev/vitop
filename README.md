# vitop

Every Vitest run on your Mac, where it is and what failed.

<p align="center">
  <img src="assets/about.png" alt="vitop's About window: its amber icon, a flask, and a link to this page" width="452">
</p>

Your agents run tests in many folders at once, and each run's output scrolls past in a terminal you are not watching.
vitop finds every Vitest run on your Mac, started by an agent, a terminal or a script: the folder it runs in, what was
typed and by whom, its workers and the memory they hold. Give a run vitop's reporter, and it also tells which file and
test run now, and where each failure is, with its error.

## Requirements

- macOS, on Apple silicon or Intel.
- Node.js 18 or later.
- Vitest. Runs were found on Vitest 4.1 and 5.1; the reporter was tried on 5.1.

## Install

> vitop is not on npm yet: the commands below work once 0.1.0 is published.

### For your agent

Give vitop to Claude Code, for every project:

```sh
claude mcp add -s user vitop -- npx -y vitop@latest mcp
```

Start a new session, and your agent has two tools: `status`, the runs going on, and `last`, how the last run of a
folder went. Ask it whether the tests passed, and it reads where the run is and what failed instead of reading the
whole output again.

Any other agent that speaks MCP runs the same server: `npx -y vitop@latest mcp`.

### For your terminal

```sh
npm install -g vitop
```

Then see where your tests are:

```sh
vitop                 # The runs going on: their files, the tests running now, each failure
vitop watch           # The same, read again every second
vitop last            # How the last run of this folder went, or goes
vitop stop 71006      # End a run, by its process or its folder
```

`vitop --help` lists every command.

### The tests of a run

With nothing more, vitop sees each run's processes. To see its files and tests, give Vitest vitop's reporter:

```sh
vitop reporter
```

It writes the reporter to `~/.vitop/reporter.mjs` and says how to give it to a run:

```sh
vitest run --reporter=default --reporter=$HOME/.vitop/reporter.mjs
```

Or to every run of a project on your Mac, in its `vitest.config`: `reporters: ['default', '<that path>']`. On a CI
machine it writes nothing, and it never fails a run.

### The menu bar

```sh
vitop menubar
```

A flask in your menu bar, filled while a run goes on, with its files beside it (`41 of 88 files`). Its menu lists each
run with the tests running now and each failure, a click copying a test's place or a failure's error, and Stop; then
the runs ended in the last hour. To have it open at every login, turn on **Open at login** in its menu.

## Kept private

Nothing leaves your Mac. vitop reads your processes and what its reporter writes in `~/.vitop`; a run's file is removed
a day after the run ends.

## Update

```sh
npm install -g vitop@latest
```

Your agent's `npx -y vitop@latest` checks for the latest version each time a session starts. Quit vitop from its menu
and open it again to run the new one.

## Uninstall

1. Turn off **Open at login** in vitop's menu, then choose **Quit vitop**.
2. Remove it from your agent and your terminal:

   ```sh
   claude mcp remove -s user vitop
   npm uninstall -g vitop
   ```

3. Remove the reporter from any `vitest.config` you gave it to, and remove its folder: `rm -rf ~/.vitop`.

## License

Free to use, at home and at work. vitop is [Append](https://append.dev)'s; its source is not published.

© 2026 Append
