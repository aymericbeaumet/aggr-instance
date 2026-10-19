---
title: Bring Your Own Tools
link: https://ampcode.com/news/toolboxes
source: ampcode-com
published: 2025-08-29T00:00:00Z
updated: 2025-08-29T00:00:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
summary: 'You can now provide tools to Amp in the form of executable files. If the environment variable AMP_TOOLBOX is set and contains the path to a directory, Amp will look in that directory for executables to be used as tools. On start, Amp will invoke each executable it found, with the environment variable TOOLBOX_ACTION set to describe. The tool is then expected to write its description (which is what will end up in the system prompt that''s sent to the agent) to stdout. Here is an example: #!/usr/bin/env bun import fs from ''node:fs'' const action = process.env.TOOLBOX_ACTION if (action === ''describe'') showDescription() else if (action === ''execute'') runTests() function showDescription() { process.stdout.write( JSON.stringify({ name: ''run-tests'', description: ''use this tool instead of Bash to run tests in a workspace'', args: { dir: [''string'', ''the workspace directory''] }, }), ) } When the agent then decides to invoke your tool, Amp runs the executable again, this time setting TOOLBOX_ACTION to execute. Amp passes the tool call arguments on stdin and the executable can then execute the tool call: function runTests() { let dir = JSON.parse(fs.readFileSync(0, ''utf-8''))[''dir''] dir = dir && dir.length > 0 ? dir : ''.'' Bun.spawn([''pnpm'', ''-C'', dir, ''run'', ''test'', ''--no-color'', ''--run''], { stdio: [''inherit'', ''inherit'', ''inherit''], }) } You can read more about toolboxes and possible input and output formats in the manual.'
content: extracted
html: 2025-08-29-bring-your-own-tools.html
---

You can now provide tools to Amp in the form of executable files.

If the environment variable `AMP_TOOLBOX` is set and contains the path to a directory, Amp will look in that directory for executables to be used as tools.

On start, Amp will invoke each executable it found, with the environment variable `TOOLBOX_ACTION` set to `describe`. The tool is then expected to write its description (which is what will end up in the system prompt that's sent to the agent) to `stdout`.

Here is an example:

```javascript
#!/usr/bin/env bun
import fs from 'node:fs'
const action = process.env.TOOLBOX_ACTION

if (action === 'describe') showDescription()
else if (action === 'execute') runTests()

function showDescription() {
	process.stdout.write(
		JSON.stringify({
			name: 'run-tests',
			description: 'use this tool instead of Bash to run tests in a workspace',
			args: { dir: ['string', 'the workspace directory'] },
		}),
	)
}
```

When the agent then decides to invoke your tool, Amp runs the executable again, this time setting `TOOLBOX_ACTION` to `execute`. Amp passes the tool call arguments on `stdin` and the executable can then execute the tool call:

```javascript
function runTests() {
	let dir = JSON.parse(fs.readFileSync(0, 'utf-8'))['dir']
	dir = dir && dir.length > 0 ? dir : '.'
	Bun.spawn(['pnpm', '-C', dir, 'run', 'test', '--no-color', '--run'], {
		stdio: ['inherit', 'inherit', 'inherit'],
	})
}
```

You can read more about toolboxes and possible input and output formats [in the manual](https://ampcode.com/manual#toolboxes).
