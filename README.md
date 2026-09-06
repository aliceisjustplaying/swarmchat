# swarmchat

Let Claude Code and Codex/Astra talk as equal peers in [Herdr](https://herdr.dev), while you watch their conversation in a third pane.

Requires macOS or Linux, Python 3, and Herdr with `herdr agent prompt`. Open both agents in the same project.

## Setup

Clone this helper once:

```sh
git clone https://github.com/aliceisjustplaying/swarmchat.git ~/src/a/swarmchat
```

Click either agent pane. Press **Ctrl+B**, release, then press **-** to open a shell underneath. In that shell, change to your project's root directory and run:

```sh
mkdir -p .peer-chat
cp ~/src/a/swarmchat/{tell-peer,claude.txt,astra.txt} .peer-chat/
chmod +x .peer-chat/tell-peer
touch .peer-chat/chat.log
tail -f .peer-chat/chat.log
```

Leave this running: the empty pane is waiting for messages. **Ctrl+C** stops watching. Add `.peer-chat/` to your project's `.gitignore`.

Paste into **Claude Code**:

```text
Read .peer-chat/claude.txt and follow its setup instructions.
```

Paste into **Codex/Astra**:

```text
Read .peer-chat/astra.txt and follow its setup instructions.
```

Once both say they're ready, give either one a task:

```text
Work with your peer to review this project. Use tell-peer to discuss improvements and recommend one to me.
```

Watch the chat below; you can still talk directly to either agent. Repeat their setup after restarting them.

Delivery errors appear in the chat log. Resolve any approval prompt and check the recipient before retrying; messages are not retried automatically.
