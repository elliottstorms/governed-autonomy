# Your first hook

A hook is a shell command Claude Code runs automatically around its own actions. This one checks every markdown file the AI writes and, if a word you have banned appears, makes Claude fix it on the spot. It is deliberately trivial: the point is to learn the mechanism, because every serious guardrail (secret scanning, style linting, publish gates) is this same shape with a bigger pattern list.

## 1. The script

Save this as `~/.claude/hooks/banned-words.sh` and make it executable (`chmod +x ~/.claude/hooks/banned-words.sh`):

```bash
#!/bin/bash
# Flags markdown files containing banned words right after they are
# written. Exit 2 sends the reason to Claude, who fixes the file;
# exit 0 stays silent.
# A wiring problem (no jq, no file) exits 0 on purpose: a broken hook
# should never block normal work. Guards that gate PUBLISHING should
# make the opposite choice and fail closed.

BANNED='synergy|circle back|paradigm shift'   # your list here

f=$(jq -r '.tool_input.file_path // empty' 2>/dev/null)
[ -n "$f" ] || exit 0
case "$f" in *.md) ;; *) exit 0 ;; esac
[ -f "$f" ] || exit 0

hits=$(grep -n -i -E "$BANNED" "$f")
if [ -n "$hits" ]; then
  echo "banned-words: fix these lines before this file is done:" >&2
  echo "$hits" >&2
  exit 2
fi
exit 0
```

## 2. The wiring

Add this to `~/.claude/settings.json` (create the file if it does not exist; if it already has a `hooks` section, merge rather than replace):

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          { "type": "command", "command": "~/.claude/hooks/banned-words.sh" }
        ]
      }
    ]
  }
}
```

## 3. Prove it works

Never trust a guard you have not watched fail, so make it fail on purpose:

1. Start a fresh Claude Code session.
2. Ask it to write a markdown file that includes one of your banned words.
3. Watch the hook report the offending line, and watch Claude rewrite the file in response.
4. Remove the banned word from the request and confirm normal writes pass quietly.

If step 3 shows no reaction, the hook is not wired: check that the script is executable, that `jq` is installed (`brew install jq` on a Mac), and that the settings file is valid JSON.

## Where to go from here

Swap the banned-word list for whatever you actually care about: your company's forbidden phrases, em dashes if they annoy you, TODO markers in files that ship. Then write your second hook as a pre-publish gate that fails closed. The mechanism never changes; only the stakes do.
