---
title: Amp TypeScript SDK
link: https://ampcode.com/news/typescript-sdk
source: ampcode-com
published: 2025-10-03T00:00:00Z
updated: 2025-10-03T00:00:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
summary: 'Today we''ve launched the Amp TypeScript SDK. It allows you to programmatically use the Amp agent in your TypeScript programs. Here is a program, for example, that instructs the Amp agent to find and list specific files in a folder using a custom toolbox tool: import { AmpOptions, execute } from ''@ampcode/sdk'' import path from ''path'' const prompt = ` What files use authentication in this directory? Go through all the files and folders. Use the format_file_tree tool to format results. Only output the file tree. ` const options: AmpOptions = { cwd: path.join(process.cwd(), ''src''), // Run in `./src` folder dangerouslyAllowAll: true, // Allow all tools, trust in Amp toolbox: path.join(process.cwd(), ''toolbox''), // Location of custom toolbox } // execute starts the agent and streams messages const messages = execute({ prompt, options }) for await (const message of messages) { // A system message contains information about the current session if (message.type === ''system'') { console.log(`Started thread: ${message.session_id}`) // For example, the custom tools that were found in the toolbox console.log( ''Available tools in Toolbox: '', message.tools.filter((tool) => tool.startsWith(''tb__'')).join('', ''), ) } else if (message.type === ''assistant'') { // An assistant message contains the assistants text replies // or tool uses console.log(''Assistant:'', message) } else if (message.type === ''result'') { // A result message contains the last message of the assistant console.log(''Files using authentication:'', message.result) } } You only need to install one package to get started: $ npm install @ampcode/sdk Since you can invoke Amp in any TypeScript program, there are very few limits to what you can build. Here are some ideas and examples of things we''ve built internally: Code Review Agent: Automated pull request analysis and feedback Documentation Generator: Create and maintain project documentation Test Automation: Generate and execute test suites Migration Assistant: Help upgrade codebases and refactor legacy code CI/CD Integration: Smart build and deployment pipelines Issue Triage: Automatically categorize and prioritize bug reports To get more ideas and familiar with the SDK, take a look at the examples in the manual.'
content: extracted
html: 2025-10-03-amp-typescript-sdk.html
---

Today we've launched the Amp TypeScript SDK. It allows you to programmatically use the Amp agent in your TypeScript programs.

Here is a program, for example, that instructs the Amp agent to find and list specific files in a folder using a [custom toolbox tool](https://ampcode.com/news/toolboxes):

```typescript
import { AmpOptions, execute } from '@ampcode/sdk'
import path from 'path'

const prompt = `
	What files use authentication in this directory?
	Go through all the files and folders.
	Use the format_file_tree tool to format results.
	Only output the file tree.
`

const options: AmpOptions = {
	cwd: path.join(process.cwd(), 'src'), // Run in `./src` folder
	dangerouslyAllowAll: true, // Allow all tools, trust in Amp
	toolbox: path.join(process.cwd(), 'toolbox'), // Location of custom toolbox
}

// execute starts the agent and streams messages
const messages = execute({ prompt, options })

for await (const message of messages) {
	// A system message contains information about the current session
	if (message.type === 'system') {
		console.log(`Started thread: ${message.session_id}`)

		// For example, the custom tools that were found in the toolbox
		console.log(
			'Available tools in Toolbox: ',
			message.tools.filter((tool) => tool.startsWith('tb__')).join(', '),
		)
	} else if (message.type === 'assistant') {
		// An assistant message contains the assistants text replies
		// or tool uses
		console.log('Assistant:', message)
	} else if (message.type === 'result') {
		// A result message contains the last message of the assistant
		console.log('Files using authentication:', message.result)
	}
}
```

You only need to install one package to get started:

```shell-session
$ npm install @ampcode/sdk
```

Since you can invoke Amp in any TypeScript program, there are very few limits to what you can build. Here are some ideas and examples of things we've built internally:

- **Code Review Agent**: Automated pull request analysis and feedback
- **Documentation Generator**: Create and maintain project documentation
- **Test Automation**: Generate and execute test suites
- **Migration Assistant**: Help upgrade codebases and refactor legacy code
- **CI/CD Integration**: Smart build and deployment pipelines
- **Issue Triage**: Automatically categorize and prioritize bug reports

To get more ideas and familiar with the SDK, take a look at the examples in the [manual](https://ampcode.com/manual/sdk#advanced-usage).
