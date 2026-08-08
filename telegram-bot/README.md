# telegram-bot

Telegram Bot API client for Pipe — build bots with long polling, no external tools needed.

Requires: Pipe v0.8+ (`http_request`, `parse_json`, `to_json` builtins).

## Install

```bash
pipe -get telegram-bot
```

Then in your script:

```pipe
import "telegram"
```

## Setup

Create a bot with [@BotFather](https://t.me/BotFather) and set the token:

```bash
export TELEGRAM_BOT_TOKEN="123456:ABC-DEF..."
```

`tg_bot nil` reads the token from the `TELEGRAM_BOT_TOKEN` env var.

## Functions

### Bot & Polling

| Function | Description |
|----------|-------------|
| `tg_bot(token?)` | Create a bot handle. Call `tg_bot nil` to read the token from the `TELEGRAM_BOT_TOKEN` env var |
| `tg_me(bot)` | Bot identity `{id, username, first_name, ...}` |
| `tg_get_updates(bot, timeout, types?)` | Long-poll for updates. `timeout` in seconds (0–50). `types` is an optional list of update types; defaults to `["message", "edited_message", "callback_query"]`. Offset is advanced automatically |

### Sending

| Function | Description |
|----------|-------------|
| `tg_send_text(bot, chat_id, text)` | Plain text message |
| `tg_send_md(bot, chat_id, text)` | Message with Markdown formatting |
| `tg_send_html(bot, chat_id, text)` | Message with HTML formatting |
| `tg_send_mdv2(bot, chat_id, text)` | Message with MarkdownV2 formatting |
| `tg_reply_text(bot, chat_id, text, reply_to)` | Text message replying to another message id |
| `tg_send_photo_url(bot, chat_id, url, caption?)` | Photo by URL (optional caption) |
| `tg_send_photo_file_id(bot, chat_id, file_id, caption?)` | Photo by file id (optional caption) |
| `tg_send_buttons(bot, chat_id, text, buttons)` | Text with an inline keyboard. `buttons` is a nested list of `{text, callback_data}` / `{text, url}` rows |
| `tg_send_chat_action(bot, chat_id, action)` | Chat action like `"typing"`, `"upload_photo"`, ... |

### Editing & Management

| Function | Description |
|----------|-------------|
| `tg_edit_text(bot, chat_id, message_id, text)` | Edit a message's text |
| `tg_edit_md(bot, chat_id, message_id, text)` | Edit with Markdown formatting |
| `tg_edit_html(bot, chat_id, message_id, text)` | Edit with HTML formatting |
| `tg_delete_message(bot, chat_id, message_id)` | Delete a message |
| `tg_forward_message(bot, chat_id, from_chat, message_id)` | Forward a message from another chat |
| `tg_set_reaction(bot, chat_id, message_id, emoji)` | React to a message with an emoji (e.g. `"👍"`) |

### Callbacks & Chat

| Function | Description |
|----------|-------------|
| `tg_answer_callback_query(bot, callback_query_id, text?)` | Acknowledge a button press (optional toast text) |
| `tg_get_chat(bot, chat_id)` | Chat info `{id, type, title, username, ...}` |
| `tg_get_chat_member(bot, chat_id, user_id)` | Member info for a user in a chat |

## Usage

```pipe
import "telegram-bot"

bot: tg_bot nil
print ("Bot: @" ++ (get (tg_me bot) "username"))

while true
    updates: tg_get_updates bot 25 nil
    for u in updates
        msg: get u "message"
        if msg != nil
            chat_id: get (get msg "chat") "id"
            text: get msg "text"
            if text != nil
                tg_send_text bot chat_id ("Echo: " ++ text)
        else
            q: get u "callback_query"
            if q != nil
                tg_answer_callback_query bot (get q "id") "Button pressed!"
```

### Inline keyboard

```pipe
buttons: [
    [{text: "Yes", callback_data: "yes"}, {text: "No", callback_data: "no"}],
    [{text: "Open Pipe", url: "https://pipe.sh"}]
]
tg_send_buttons bot chat_id "Vote:" buttons
```

Then handle presses in the poll loop via `callback_query` (see above).

See the full [Echo + AI bot example](https://github.com/MachuraHarry/pipe/blob/master/examples/telegram_bot.pipe) with `/start` buttons, `/summarize`, `/translate` and reactions.
