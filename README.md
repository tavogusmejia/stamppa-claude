# Stamppa

The Stamppa plugin for Claude. It connects Claude to [Stamppa](https://stamppa.com), the PDF viewer for drawing sets. The plugin gives Claude a local connection to the drawings you have open in Stamppa and three ready-made skills for plan work.

## What it is

- **A local connection (MCP)** to the Stamppa app on your computer. Claude can list your sheet sets, search and read sheet text, read markups and measured quantities, and, if you allow it, add markups.
- **Three skills**:
  - `review-drawing-set`: review a set, find issues and leave markups signed as made by the AI assistant, unsaved, for you to review.
  - `quantities-to-table`: turn the measurements in a drawing into a Markdown or CSV table.
  - `review-report`: build a review report from the markups, the sheet text and the sheet package.

No product code lives in this repository: only the manifest, the connection settings and the skills.

## Requirements

- Stamppa **27.3.2 or later**, installed on the same computer as Claude, and a Stamppa license.
- In Stamppa, **Settings > AI assistant** turned on. To let Claude add or change markups, also tick "Let it add and change markups". Markups Claude adds are signed as made by the AI assistant, are one undo step and stay unsaved until you save.

## Installation

### Claude Code

```
/plugin marketplace add tavogusmejia/stamppa-claude
/plugin install stamppa@stamppa-claude
```

Start a new session. The skills appear as `/stamppa:review-drawing-set`, `/stamppa:quantities-to-table` and `/stamppa:review-report`.

The connection starts `Stamppa --mcp`. On macOS it uses `/Applications/Stamppa.app/Contents/MacOS/Stamppa`. The plugin format has no per-system command, so on **Windows** set the environment variable `STAMPPA_EXE` to the full path of `Stamppa.exe` (the folder you chose when you installed Stamppa) before starting Claude Code, or connect with the one-click button described below, which writes the right path for your system.

### Cowork

Add the same marketplace from the plugins screen in Cowork and install `stamppa`. Stamppa runs on your computer, so Cowork must be running on the same computer. Windows users: set `STAMPPA_EXE` as above.

### Claude Desktop

In the Desktop chat, the connection comes from Stamppa itself, in one click: open **Stamppa > Settings > AI assistant**, choose Claude Desktop and press Connect, then quit and reopen Claude Desktop. The skills come with the plugin in Cowork or Claude Code.

## Privacy

> **Draft, pending legal review (US-334).** Do not rely on this section until that review is done.

- Your PDF files do not leave your computer through this plugin. Stamppa runs locally and the connection runs on your machine.
- What the tools **return** does go to the model: sheet text, markup comments, measured quantities, sheet names and, if you ask for them, sheet images. Claude processes that content under the terms of the Claude plan you use.
- Do not connect drawings that you are not allowed to share with an AI service.
- Plano Base Arquitectos S.A.S. does not receive your drawings or your conversations through this plugin.

## License

To be decided (pending). Until then no license is granted beyond reading and installing the plugin as published.

## About

Made by Plano Base Arquitectos S.A.S. - https://stamppa.com

## Trademarks

Stamppa is a product of Plano Base Arquitectos S.A.S. It is not affiliated with, endorsed by or sponsored by Anthropic. Claude is a trademark of Anthropic, PBC.
