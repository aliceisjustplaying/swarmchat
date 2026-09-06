# swarmchat

Let Claude Code and Codex/Astra talk as equal peers in [Herdr](https://herdr.dev), with a shared conversation you can watch in a third terminal pane.

`tell-peer` appends each message to `chat.log`, then delivers it through `herdr agent prompt`. Both agents keep their existing CLI sessions and work in the same project. Python standard library only; no server or API keys to configure.

## Setup

Requires macOS or Linux, Python 3, and Herdr with `herdr agent prompt`. Start Claude Code and Codex/Astra in two Herdr panes, both in the project you want them to work on.

Clone this helper once (skip this if it is already there):

```sh
git clone https://github.com/aliceisjustplaying/swarmchat.git ~/src/a/swarmchat
```

### 1. Open the chat pane

Click either agent pane, press **Ctrl+B**, release, then press **-** to split downward with Herdr's default keys. In the new shell, change to your project's root directory, then run:

```sh
mkdir -p .peer-chat
cp ~/src/a/swarmchat/tell-peer ~/src/a/swarmchat/claude.txt ~/src/a/swarmchat/astra.txt .peer-chat/
chmod +x .peer-chat/tell-peer
touch .peer-chat/chat.log
tail -f .peer-chat/chat.log
```

**An empty pane is normal.** `tail -f` stays running and displays messages as they arrive. Ctrl+C stops watching the log; it does not stop either agent. Keep `.peer-chat/` out of your project's commits by adding it to that project's `.gitignore`.

### 2. Connect Claude

Paste into Claude Code and wait for it to report ready:

```text
Read .peer-chat/claude.txt and follow its setup instructions.
```

### 3. Connect Astra

Paste into Codex/Astra and wait for it to report ready:

```text
Read .peer-chat/astra.txt and follow its setup instructions.
```

These instructions name the agents `claude` and `astra` in Herdr and teach both the same messaging command. Those names must be available in your Herdr session. Repeat the connection steps after restarting either agent.

### 4. Give either agent an objective

For example:

```text
Work with your peer to review this project and agree on the most useful next improvement. Use tell-peer to discuss it, then report your joint recommendation to me. You are equal collaborators.
```

Watch their conversation in the chat pane. You can still type directly into either agent.

## Sending a message

From a Herdr shell in the project root:

```sh
./.peer-chat/tell-peer claude 'Can you review my changes?'
./.peer-chat/tell-peer astra 'Yes. Which files should I look at?'
```

The name is the **sender**; the helper chooses the other agent as recipient. It submits immediately, including when the recipient is working. A successful send means Herdr accepted the input, not that the peer has replied.

Delivery errors appear in the log and return a nonzero exit code. If an agent is blocked on an approval prompt, resolve it and check whether the message arrived before retrying. There is no automatic retry or message queue. The setup prompts tell peers to coordinate file edits and avoid endless acknowledgment loops.
