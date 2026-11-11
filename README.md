# claude-proofread

A tiny HTTP proofreading service backed by headless [Claude Code](https://claude.com/claude-code) (`claude -p`), so it runs on a Claude subscription instead of an API key.

It fixes typos, misspellings and punctuation only. It never rephrases, and it never changes letter case. The final period is optional, but a question still gets its `?`.

## API

```
POST /proofread   {"text": "..."}   -> 200 {"text": "<corrected>"}
GET  /health                         -> 200 {"ok": true}
```

Texts can be up to 4000 characters. At most 3 `claude` processes run at once.

## Run

Requirements: Python 3 (stdlib only), and `claude` installed and logged in.

```sh
./proofread-server.py
```

| env | default |
|---|---|
| `PROOFREAD_HOST` | `0.0.0.0` |
| `PROOFREAD_PORT` | `8787` |
| `PROOFREAD_MODEL` | `sonnet` |
| `CLAUDE_BIN` | `~/.local/bin/claude` |
| `CLAUDE_CONFIG_DIR` | `~/.claude` |

There is **no authentication**, so bind it to a private network only.

Keep it alive with cron:

```cron
* * * * * flock -n /tmp/proofread-server.lock /path/to/proofread-server.py
```

## License

MIT
