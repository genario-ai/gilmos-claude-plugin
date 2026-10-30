![GilmOS](assets/icon.png)

# GilmOS for Claude

Start a studio workspace or develop films in your existing [GilmOS](https://app.gilm.ai) projects from Claude. Write structured screenplays, break scenes into shots, prepare character and location references, generate images and videos, and save montage edit decisions. Read project and workspace chats and send authorized messages or replies as your connected account, plan resource delivery dates, project deadlines, tasks and meetings in the native GilmOS agenda.

## What's included

| Skill | Use it to |
| --- | --- |
| `gilmos-onboarding` | Understand workspaces and projects, inspect account setup, create a requested first FREE workspace and project |
| `gilmos-projects` | Connect, find a project, read its production context, manage documents, tasks, statuses and assignments |
| `gilmos-screenwriting` | Write or revise a screenplay with typed cinematic paragraphs, or run Alfred's long-screenplay and formatting workflows |
| `gilmos-storyboards` | Prepare scene breakdowns, shot lists and storyboard boards with narrative and visual continuity |
| `gilmos-assets` | Prepare characters, locations and props, select reference versions, run asset image and description workflows |
| `gilmos-generation` | Generate or display sandbox images and videos with exact asset tags and follow asynchronous jobs |
| `gilmos-montage` | Read and edit the saved montage timeline with revision checks |
| `gilmos-collaboration` | Read and send project/workspace chat messages and plan production in the agenda |

The plugin also declares one remote MCP server, `gilmos`, at `https://app.gilm.ai/mcp`, which provides the `gilmos_*` tools the skills use.

## Setup

1. In Claude's **Customize → Plugins → Add plugin → Add marketplace**, add `genario-ai/gilmos-claude-plugin`, then add the `gilmos` plugin. In Claude Code, run `claude plugin marketplace add genario-ai/gilmos-claude-plugin` followed by `claude plugin install gilmos@gilmos-marketplace`.
   You can also upload an archive of this plugin in **Customize → Plugins → Add plugin → Upload plugin**.
2. Connect your GilmOS account through the OAuth sign-in window. In Claude Code, run `/mcp` and select `gilmos` if the connection has not opened automatically.
3. Ask, for example: "Help me set up my first GilmOS workspace" or "List my GilmOS projects and summarize the selected project's next production step."

You need a GilmOS account. Installing the plugin does not itself create a workspace or project. The plugin only reaches projects and conversations your account can already access.

This repository is an independent GitHub marketplace. The full GilmOS connector is not listed in Anthropic's official directory because that directory currently excludes standalone AI image, video and audio generation tools.

## Data and network behavior

- **Single destination**: all tool calls go to the GilmOS MCP server at `https://app.gilm.ai/mcp`, operated by Gilm Technologies. The plugin bundles no local code, scripts or hooks.
- **Authentication**: OAuth through the GilmOS identity provider (Supabase Auth). Tokens are stored by the Claude client; the plugin contains no credentials.
- **Permissions**: every tool acts with the connected user's GilmOS permissions, including private conversations the user is a member of.
- **Writes**: on request, tools can create a workspace or project, or create and update screenplays, scenes, shots, assets, montage documents, tasks, calendar events and chat messages in GilmOS. Messages are sent as your account and labelled "via MCP". Selected participants may receive GilmOS invitations and notifications.
- **Credits**: image/video generations and Alfred writing workflows consume your existing GilmOS credits. Creating a workspace or project does not supply generation credits.
- **Limits**: montage tools save edit decisions; rendering or exporting a finished video is not available through this plugin.

## Support and policies

- Help: <https://app.gilm.ai/help>
- Privacy policy: <https://app.gilm.ai/privacy>
- Terms of service: <https://app.gilm.ai/terms>
