---
title: Streaming JSON
link: https://ampcode.com/news/streaming-json
source: ampcode-com
published: 2025-09-09T00:00:00Z
updated: 2025-09-09T00:00:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
summary: 'The Amp CLI has two new flags: --stream-json and --stream-json-input. They can be used together with the --execute flag to turn on structured JSON output and input, respectively. Here is an example: $ amp --execute "what is 2+2?" --stream-json This would print something like the following on stdout: {"type":"system","subtype":"init","cwd":"/Users/mrnugget/work/amp","session_id":"T-2775dc92-90ed-4f85-8b73-8f9766029e83","tools":["oracle","..."],"mcp_servers":[]} {"type":"user","message":{"role":"user","content":[{"type":"text","text":"what is 2+2?"}]},"parent_tool_use_id":null,"session_id":"T-2775dc92-90ed-4f85-8b73-8f9766029e83"} {"type":"assistant","message":{"type":"message","role":"assistant","content":[{"type":"text","text":"2 + 2 equals 4."}],"stop_reason":"end_turn","usage":{"input_tokens":12783,"cache_creation_input_tokens":367,"cache_read_input_tokens":12416,"output_tokens":8,"max_tokens":224000,"service_tier":"standard"}},"parent_tool_use_id":null,"session_id":"T-2775dc92-90ed-4f85-8b73-8f9766029e83"} {"type":"result","subtype":"success","duration_ms":2906,"is_error":false,"num_turns":1,"result":"2 + 2 equals 4.","session_id":"T-2775dc92-90ed-4f85-8b73-8f9766029e83"} Structured input can be provided on stdin with the --stream-json-input flag: $ echo ''{ "type": "user", "message": { "role": "user", "content": [ { "type": "text", "text": "what is 2+2?" } ] } }'' | amp -x --stream-json --stream-json-input If --stream-json-input is set, the Amp CLI will process user messages as soon as they are received on stdin and exit as soon as stdin is closed and the agent has ended its turns. Combined with amp threads continue [thread], multi-turn conversations can even happen across multiple invocations of the CLI: $ amp -x "what is 2+2?" --stream-json ... $ amp threads continue -x "and now add 7 to that" --stream-json ... {"type":"result","result":"11.", "subtype":"success","duration_ms":2906,"is_error":false,"num_turns":2,"session_id":"T-2775dc92-90ed-4f85-8b73-8f9766029e83"} See the Manual for more details and the Appendix for the full schema.'
content: extracted
html: 2025-09-09-streaming-json.html
---

The Amp CLI has two new flags: `--stream-json` and `--stream-json-input`.

They can be used together with the `--execute` flag to turn on structured JSON output and input, respectively.

Here is an example:

```shell-session
$ amp --execute "what is 2+2?" --stream-json
```

This would print something like the following on stdout:

```
{"type":"system","subtype":"init","cwd":"/Users/mrnugget/work/amp","session_id":"T-2775dc92-90ed-4f85-8b73-8f9766029e83","tools":["oracle","..."],"mcp_servers":[]}
{"type":"user","message":{"role":"user","content":[{"type":"text","text":"what is 2+2?"}]},"parent_tool_use_id":null,"session_id":"T-2775dc92-90ed-4f85-8b73-8f9766029e83"}
{"type":"assistant","message":{"type":"message","role":"assistant","content":[{"type":"text","text":"2 + 2 equals 4."}],"stop_reason":"end_turn","usage":{"input_tokens":12783,"cache_creation_input_tokens":367,"cache_read_input_tokens":12416,"output_tokens":8,"max_tokens":224000,"service_tier":"standard"}},"parent_tool_use_id":null,"session_id":"T-2775dc92-90ed-4f85-8b73-8f9766029e83"}
{"type":"result","subtype":"success","duration_ms":2906,"is_error":false,"num_turns":1,"result":"2 + 2 equals 4.","session_id":"T-2775dc92-90ed-4f85-8b73-8f9766029e83"}
```

Structured input can be provided on stdin with the `--stream-json-input` flag:

```shell-session
$ echo '{
  "type": "user",
  "message": {
    "role": "user",
    "content": [
      {
        "type": "text",
        "text": "what is 2+2?"
      }
    ]
  }
}' | amp -x --stream-json --stream-json-input
```

If `--stream-json-input` is set, the Amp CLI will process user messages as soon as they are received on stdin and exit as soon as stdin is closed and the agent has ended its turns.

Combined with `amp threads continue [thread]`, multi-turn conversations can even happen across multiple invocations of the CLI:

```shell-session
$ amp -x "what is 2+2?" --stream-json
...
$ amp threads continue -x "and now add 7 to that" --stream-json
...
{"type":"result","result":"11.", "subtype":"success","duration_ms":2906,"is_error":false,"num_turns":2,"session_id":"T-2775dc92-90ed-4f85-8b73-8f9766029e83"}
```

See the [Manual](https://ampcode.com/manual#cli-streaming-json) for more details and the [Appendix](https://ampcode.com/manual/appendix#stream-json-output) for the full schema.
