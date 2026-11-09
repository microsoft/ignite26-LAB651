## Before you're done

This repo has been created for your Microsoft Ignite 2026 session. Here's how to get it ready.

**Easiest path — use the agent (recommended):**

- Open GitHub Copilot Chat and say `help me initialize repo`. The agent will walk you through getting the README populated.
- When you're ready to publish, say `help me finalize repo`. The agent will clean up unused folders, validate everything, and remove this "Before you're done" section and other extra stuff that attendees don't need to see.
- Curious how it works? Read the [agent workflow](.github/AGENT-WORKFLOW.md).

**Doing it manually?**

Fill in the sections below yourself, then:

- Delete any placeholder folders you don't need (`data/`, `infra/`, etc.)
- Delete this "Before you're done" section
- Delete `.github/agents/`, `.github/tests/`, `.github/copilot-instructions.md`, and `.github/AGENT-WORKFLOW.md` — these are template tooling, not part of your published repo

**Folder conventions:**

- Attendee step-by-step guidance goes in `instructions/`. If you use MkDocs or a docs site instead, put it in `docs/` and link to it from this README.
- Reference material and background reading go in `docs/`.
- You can add a `.devcontainer/` folder if needed.

---

<a name="start-building"></a>

<p align="center">
<img src="img/banner-ignite-26.png" alt="Microsoft Ignite 2026" width="100%"/>
</p>

# [Microsoft Ignite 2026](https://ignite.microsoft.com)

## 🔥LAB651: Build AI Agents in Teams
### Session description

Agents are only useful when they can act. Join the Teams Platform team to build one hands-on: scaffold an agent with the Teams SDK, ground it in your data, and give it real reach with Channel Agent + MCP tools. Then go no-code — create an agent-powered workflow in natural language and see how 1P agents like Researcher and Analyst, plus third-party low-code agents, land in Teams. Leave with working code and patterns you can ship. No prior Teams development experience required.

### 🚀 Getting started

#### In a guided session

If you're following along during a live session:

1. Open [`instructions/`](instructions/README.md) and follow along in the Skillable lab environment

#### On your own

If you're learning at your own pace:

1. Clone this repository
2. Set up your environment
3. Follow the session guidance in [`instructions/`](instructions/README.md)

### 🎯 Learning outcomes

By the end of this session, you will have:

- Built a **Microsoft Teams agent** that evolved from a local prototype into a collaborative teammate capable of working directly within Teams conversations.

- Configured an **Azure OpenAI** LLM resource on **Microsoft Foundry** and integrated the resource into your agnet. 

- Used the **Teams CLI** and **Dev Tunnel** to create, configure, sideload, and test the agent in Microsoft Teams.

- Used **Socket Mode** to connect your agent to Teams during development.

- Enabled collaboration through **Resource-Specific Consent (RSC)** permissions, allowing the agent to participate in channels, read conversation threads, respond to @mentions, and generate contextual summaries and next steps.

- Connected the agent to an external **Contract Management System using MCP**, enabling it to retrieve contract information and create new contract records from natural-language requests.

- Implemented **Adaptive Cards** that transform conversation context into a shared five-item checklist that multiple channel members can interact with and update in real time.

- Explored how Adaptive Card events are orchestrated, rendered, and synchronized across users.

- Added **targeted messaging**, enabling the agent to acknowledge individual users' card interactions privately without notifying other channel members.

### 💻 Technologies used

- Teams SDK [TypeScript]
- Microsoft Foundry with Azure OpenAI
- Teams CLI
- VSCode
- Model Context Protcol (MCP)
- PowerShell

### 📚 Continue your learning

Pick your next step based on your learning style:

| Resource | What you'll get |
|----------|-----------------|
| **[Ignite 2026 Next Steps](https://aka.ms/ignite26-next-steps)** | Additional session repos and materials from Microsoft Ignite 2026 |
| **[Microsoft AI Skills Navigator](https://aka.ms/ignite26-asn)** | Discover AI skills, learning paths, and resources for your next step |
| **[Microsoft Learn](https://aka.ms/ignite26-learn)** | Official documentation and guided learning paths on these topics |
| **[Microsoft Foundry Community](https://aka.ms/MicrosoftFoundryDiscord-Ignite26)** | Connect with other learners and experts in our Discord community |

### 🌟 Microsoft Learn MCP Server

<!-- Remove this section if the Microsoft Learn MCP Server is not relevant to the session. -->

The Microsoft Learn MCP Server gives your AI agent direct access to Microsoft's official documentation — grounded, up-to-date answers about the topics in this session.

**GitHub Copilot CLI** — Install with:

```shell
copilot plugin install microsoftdocs/mcp
```

**VS Code** — One-click install:  
[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install_Microsoft_Learn_MCP-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://vscode.dev/redirect/mcp/install?name=microsoft-learn&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Flearn.microsoft.com%2Fapi%2Fmcp%22%7D)

For more information, visit the [Learn MCP Server repo](https://aka.ms/learnmcp).

### 👥 Content owners

<!-- TODO: Add yourself as a content owner
1. Change the src in the image tag to {your github url}.png
2. Change INSERT NAME HERE to your name
3. Change the github url in the final href to your url. -->

<table>
<tr>
    <td align="center"><a href="http://github.com/MSFTRickyCastaneda">
        <img src="https://github.com/MSFTRickyCastaneda.png" width="100px;" alt="INSERT NAME HERE"/><br />
        <sub><b>Ricky Castaneda</b></sub></a><br />
            <a href="https://github.com/MSFTRickyCastaneda" title="talk">📢</a>
    </td>
</tr></table>
<tr>
    <td align="center"><a href="http://github.com/lilyydu">
        <img src="https://github.com/lilyydu.png" width="100px;" alt="INSERT NAME HERE"/><br />
        <sub><b>Ricky Castaneda</b></sub></a><br />
            <a href="https://github.com/lilyydu" title="talk">📢</a>
    </td>
</tr></table>

### ⚖️ Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft trademarks or logos is subject to and must follow [Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/legal/intellectualproperty/trademarks/usage/general). Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.

Any use of third-party trademarks or logos are subject to those third-party's policies.
