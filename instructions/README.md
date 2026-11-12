## Overview

Most agentic interactions take place in a one-to-one chat. In this lab, you will create an agent that primarily works in collaborative spaces like group chats and channels in Microsoft Teams. Today you'll create an agent that achieves the following: 

- Read channel conversations and summarize threads.
- Pull live data from an external contract management system through MCP.
- Post shared Adaptive Cards that the whole channel can update.
- Send targeted messages to the person who took an action on an adaptive card.

We'll build this agent in a real Microsoft 365 Enterprise tenant with an Azure OpenAI key. You'll leave this lab with a fully functional collaborative agent you can resuse to help create your own Teams agent. This agent was built using the Teams SDK. Teams SDK is available in Typescript, .NET, and Python. 

---

## Lab Breakdown
This lab is divided into 5 exercises:
> [+note] **Exercise 0: Build your First Agent & Connect AI Resources**
  - Bootstrap your new agent
  - Open your agent code
  - Configure your Azure OpenAI LLM key
  - Run the agent in Agents Playground

> [+note] **Exercise 1: Bring Your Agent to Teams**
   - Set up a development tunnel
   - Create a Teams app registration
   - Sideload it in 1:1 chat
   - Test streaming

> [+note] **Exercise 2: Enable Collaboration**
   - Update RSC permissions 
   - Add the agent to a channel
   - Test @mentions, thread reading, and summaries

> [+note] **Exercise 3: Connect Tools via MCP**
   - Integrate contract management via MCP

> [+note] **Exercise 4: Create Shared Checklists **
   - Generate a five-point checklist from conversation context
   - Post the checklist as an Adaptive Card
   - Sync updates across channel members

> [+note] **Exercise 5: Send Targeted Notifications **
   - Notify users when checklist items are updated via private targeted messages

---
**Please proceed to the next section.** (Navigation aids are at the bottom right)

---
>[!note] This lab is written in Typescript. No prior knowledge is needed.

>[!help]**If you are stuck in any part of the lab, please raise your hand to ask a proctor for help.** 

============================================================================

## Exercise 0: Build your First Agent & Connect AI Resources

In this exercise, you will: 
   1. Setup your M365 Enterprise tenant to accept sideloaded apps
   2. Setup your development environment
   3. Configure the agent to use Azure OpenAI with Microsoft Foundry
   4. Run the agent locally with Agents Playground

---

### Step 1: Enable Sideloading apps and agents for the Tenant

> This sideloading script will makes changes to your M365 tenant which allows users to upload custom apps (also known as sideloading). Since we aren't publishing our agent to the store, this is how we will use our agent in Teams.

1. [] Log in to the Windows using the following password:

   +++@lab.VirtualMachine(Win11-Pro-Base).Password+++


2. [] Click **Start** and Run the shortcut: **Enable Sideloading**.

> This script may take up to 45 seconds to initialize

3. [] Sign in using the following username & TAP (password):
	- Username: +++PattiF@@lab.CloudCredential(WWLM365E7).TenantName+++

   - TAP (Temporary Access Pass): +++@lab.Variable(TAP2)+++
   ---

If prompted, select **Yes**. Wait for the account to be added to the device, then select **Done**.
!IMAGE[bboldc3g.jpg](instructions361584/bboldc3g.jpg)

Once the Scipt is executed, it will indicate that sideloading is enabled.
!IMAGE[9d6na16t.jpg](instructions361584/9d6na16t.jpg)

5. [] Close the Terminal

---

### Step 2: Setup your development environment

> The `.env` file contains all of the variables your agent needs to connect to AI resources, as well as connect to Teams services to facilitate conversations. These files are typically hidden to prevent leaking API keys and other secrets.


1. [] Open **VS Code** from the **Start Menu**.
    - The Ignite Lab `.env` file should load automatically.

2. [] In the project directory, verify that `index.ts`  under `src` is accessible.

3. [] Review the `.env` file to understand its configuration. This is where we'll input our Azure OpenAI key and client secrets later

---

### Step 3: Configure the agent to use Azure OpenAI with Microsoft Foundry
> Microsoft Foundry & Azure OpenAI are acting as the LLM provider for your agent. When users make requests to your agent, the requests are routed to this LLM provider for processing and response.

1. [] Open the Microsoft Edge profile for +++Azure Admin+++ from the **Start Menu**.
2. [] Navigate to +++ai.azure.com/nextgen+++
3. [] Sign in using the following username:

   +++@lab.CloudPortalCredential(User1).Username+++

4. [] Authenticate using the following TAP (Temporary Access Pass):

   +++@lab.CloudPortalCredential(User1).AccessToken+++

5. [] Wait for Microsoft Foundry to complete the setup process. (Will take ~1 min)

	!IMAGE[t0ix5337.jpg](instructions361584/t0ix5337.jpg)

>  If post setup welcome pop-up appears in Microsoft Foundry, select **Skip** to continue.

6. [] Copy the **[API key]** and **[Azure OpenAI endpoint]** from the appropriate section in the portal.

   ![Azure AI Foundry credentials](instructions361584/npiijcmn.jpg)

7. [] In Visual Studio Code, open the `.env` file and update the copied values as shown below:

> Example <br>
> AZURE_OPENAI_API_KEY  =  5kiBiDOuzD....sws7&X <br>
> AZURE_OPENAI_ENDPOINT =  https://domain.openai.azure.com/openai/v1

8. [] Save the `.env` file after making any updates by pressing ++Ctrl + S++.

---

> Your API Key and URI provide connection to the resources provided by your LLM provider. You are not limited to using Azure OpenAI with the Teams SDK.

1. [] In Microsoft Foundry, navigate to **Explore Models**.

   ![Explore Models](instructions361584/hu35rrxr.jpg)

2. [] Search for +++gpt-5.4-mini+++

> [!alert] gpt-5.4-mini is the only model available for deployment in this lab.
> ![Available model](instructions361584/cvc9kz7s.jpg)


3. [] Open the **gpt-5.4-mini** model details page, then select Custom Deploy.

> In case **Custom Deploy** button is not available select **Deploy > Custom Settings**

4. [] Set the Deployment Name to +++gpt-5.4-mini-ignite+++

5. [] Leave all the other parameters at their default values, do not change anything else, and deploy the model by clicking **Deploy**.

> The value +++gpt-5.4-mini-ignite+++ is already set in `.env` file, in case you use a different value for Deployment Name, please update the `.env` file.


6. [] Close the Edge Browser for Azure Admin.


---

### Step 5: Run the agent locally with Agents Playground

1. [] In VS Code, open a new Terminal.

> [!hint] The terminal should already be open. If it isn't, you can open it using the Ctrl + `

2. [] Once the Terminal is ready, start the server by running

    +++npm run dev+++

3. [] Wait a ~1 min until the terminal displays

    ++listening on port 3978 🚀++

4. [] Open **Agents Playground** from the **Start menu**.

5. [] When Agents Playground starts, a new terminal window will open and load the browser.

> [!alert] Do not close this terminal window while using Agents Playground. Closing it will stop the playground, and you will have to perform the **Step 4** again.

---

### Step 6: Test the Agent Locally

1. [] Once the **Agents Playground** loads, test the agent by interacting with it.

2. [] Use the following sample prompts:
   - +++Hi+++
   - +++What can you do?+++

3. [] After completing the tests, do the following:
   - Close the Microsoft Edge browser window.
   - Close the terminal window running the Agents Playground.
   - In VS Code, stop the locally running agent process by pressing +++Ctrl + C+++ in the terminal.

---
Congrats, you just built your agent and connected it to a model! :tada:

---
**Please proceed to the next section.** 

============================================================================

## Exercise 1: Bring Your Agent to Microsoft Teams

In this exercise you will:
   1. Set up a development tunnel
   2. Setup Teams CLI
   3. Create a Teams app registration
   4. Sideload your agent in 1:1 chat

---

> When you build your agent for production use, you will host your agent on the internet, in doing so you will have a public URL where Teams can send messages and activites. Since we are not publishing our app, we need to create a "tunnel" that bridges the connection between Teams services and your agent. That "tunnel" is a DevTunnel

### Step 1: Set up a development tunnel

> In this step, you are opening a connection on port 3978 from your machine to the URL being provided. This tunnel mimics the public endpoing you will deploy when you release your agent for production use. 

1. [] Click **Start** and Run the shortcut: **Dev Tunnel**

2. [] Select the following user and continue:

   +++PattiF@@lab.CloudCredential(WWLM365E7).TenantName+++

> In case prompted for TAP, use the following   +++@lab.Variable(TAP2)+++

4. [] Post login, a new window with white background for the devtunnel. Close and minimise buttons will be disabled for this window.

5. [] Verify if the devtunnel window started and you have the Tunnel url. The url will be in the format `domain.asse.devtunnels.ms`, visible next to **Connect via browser:** in the devtunnel.

> [!alert] **Keep this devtunnel terminal running for the remainder of the lab.**
> Closing the devtunnel terminal will cause the Dev Tunnel hostname to change.
> You can minimise the terminal window by clicking the terminal icon on taskbar, but do not close it.
   !IMAGE[jj98omud.jpg](instructions361584/jj98omud.jpg)

> In case you Close this by Mistake Run the **Step 5** command again, it will restart the tunnel but will generate a new domain name, you will have to update this domain name in the Teams App later with Teams CLI. Look into the Last section of the Lab if devtunnel is stopped once you create the app via cli.

---

### Step 2: Setup Teams CLI
> In the following steps, you are starting to use the Teams CLI. Teams CLI is a collection of tools available to help developers build and modify their agent settings without having to leave their development environment. It also allows programatic access to these tools via coding agents. 

1. [] Open the Microsoft Edge profile for +++PattiF+++ from the **Start Menu**.

	Navigate to +++portal.office.com+++

   login with  +++PattiF@@lab.CloudCredential(WWLM365E7).TenantName+++

   Authenticate using the following TAP (Temporary Access Pass):

   +++@lab.Variable(TAP2)+++

	Once the login is done close the Browser

1. [] Open A NEW **Terminal** from the **Start Menu**. The devtunnel terminal will continue running in the background.

2. [] Run the following command to login to teams cli

   +++teams login+++

   It should open browser and auto login, in case prompted use the login from **Step 1**

> When the browser *says Authentication successful!*, you can close the browser

3. [] Run the following command to login to azure cli (You can use the same terminal)

   +++az login --allow-no-subscriptions+++

> Teams CLI usees resources across your tenant and Azure subscriptions. We log into all these services so the CLI can take action across services and save developers the headache of working in multiple systems.

4. [] Select the following user and continue:

   +++PattiF@@lab.CloudCredential(WWLM365E7).TenantName+++

> In case prompted for TAP, use the following   +++@lab.Variable(TAP2)+++

5. [] Post login you should see Select Subscription

> Select a subscription and tenant (Type a number or Enter for no changes):

Press Enter and continue

---

### Step 3: Create a Teams app registration
> The agent in this demo uses a bot registration behind the scenes. Your Client ID and Secret are tied to your bot registration and bridge the connection between your running code and the bot registration.


1. [] Run the following command to launch the Teams CLI:

   +++teams app+++

2. [] Select **Create App**. (↑↓ navigate • ⏎ select)
3. [] Enter +++Zava-Help-Desk+++ as the app name.
4. [] For the **Endpoint URL**, copy the Dev Tunnel URL and append ++/api/messages++

> [!hint] The Dev Tunnel will be running in the background. To access it, hover over the Terminal icon on the taskbar to display the active terminal windows, then select the **Dev Tunnel** terminal.

!IMAGE[vm22dw8g.jpg](instructions361584/vm22dw8g.jpg)

> Example:<br>
> https://sjdjdlpm-3978.asse.devtunnels.ms/api/messages

5. [] Path to credentials file : **Do not type anything, press ENTER and continue**

6. [] Advanced options? : **Do not type anything, press ENTER and continue**

7. [] Confirm creation? : **Type Y and ENTER**

6. [] Wait for the app process to complete. Copy the generated output values into the `.env` file in your codebase.

>   CLIENT_ID    = generated_client_id<br>
>   CLIENT_SECRET= generated_client_secret<br>
>   TENANT_ID    = generated_tenant_id<br>

Replace the placeholders with the actual generated values by the CLI.

!IMAGE[7vjm9yf4.jpg](instructions361584/7vjm9yf4.jpg)

7. [] Also Update the following setting in the `.env` file to `false`

> LOCAL_DEBUG=false

> [!hint] Remember to save the `.env` file after making any updates by pressing ++Ctrl + S++.

8. [] After updating the environment variables, start the server by running the following command in the VS Code terminal:

   +++npm run dev+++

9. [] Go back to the Teams CLI, select **Install in Teams**, this should launch Teams in the brower.
10. [] Select **Use the web app instead**.
11. [] Sign in using the following username (If prompted):

    +++PattiF@@lab.CloudCredential(WWLM365E7).TenantName+++

   Authenticate using the following TAP (Temporary Access Pass):

    +++@lab.Variable(TAP2)+++

12. [] Select Add to add the agent in Teams, once added click `Open`.

!IMAGE[seah00ta.jpg](instructions361584/seah00ta.jpg)

> [!hint] If you don't see the **Add** button, wait ~1 minute, Teams might still be loading.

---

### Step 4: Sideload your agent in 1:1 chat

1. [] Once the Teams agent loads, test the agent by interacting with it.

2. [] Use the following sample prompts:
   - +++Hi+++
   - +++What can you do?+++

3. [] After completing the tests, do the following:
   - Close the Microsoft Edge browser window.
   - Close the terminal window running the Teams CLI.
   - In VS Code, stop the locally running agent process by pressing +++Ctrl + C+++ in the terminal.
   - DO NOT close the devtunnel terminal

---

Congratulations, you've taken your agent from running in a local environment to running in Teams :tada: <br>
**Please proceed to the next section.**

============================================================================

## Exercise 2: Enable Collaboration

In this exercise you will: </br>
   1. Create mock data in your M365 Enterprise tenant
   2. Update RSC permissions for your agent
   3. Add your agent to a channel & run your agent
   4. Enable @mentions, thread reading, and summaries
   5. Test external data fetching
   6. Cleanup your envrionment
   
---

### Step 1: Create mock data in your M365 Enterprise tenant

1. [] To test collaborative agents, we need conversations involving multiple users.
2. [] The script below, will create mock conversations and a Teams channel that will be used in later steps.
3. [] Open a new terminal from the **Start Menu**.
4. [] Run the following PowerShell script:

   +++.\helper_scripts\Create-TeamConversation.ps1+++

5. [] When prompted for details, enter the following username:

   +++PattiF@@lab.CloudCredential(WWLM365E7).TenantName+++

6. [] You can proceed to the next step while the script continues running in the background. Do not close this terminal Until the script finishes. This takes ~2 min.

!IMAGE[93mehqi5.jpg](instructions361584/93mehqi5.jpg)

---

### Step 2: Update RSC permissions for your agent
> In this step, adding RSC permissions to your agent allows it to view and interact with messages in Channels. By default, agents can only see messages where the agent has been @mentioned, RSC helps expand that to allow the agent to see all messages in a channel without granting access to every message in your organization.

1. [] Open a New Terminal Tab 

!IMAGE[x3ugdci3.jpg](instructions361584/x3ugdci3.jpg)

2. [] Launch the Teams CLI by running:

   +++teams app+++

3. [] Navigate to **Select App**.

4. [] Select +++Zava-Help-Desk+++

5. [] Navigate to **Permissions (RSC)**. You can use ↑↓ navigate • ⏎ select

6. [] Select **Edit Team Permissions**.

7. [] Add the following permissions (See the below hint on how to add these easily) :
	- `ChannelMember.ReadWrite.Group`
   - `ChannelMessage.Read.Group`
   - `ChannelMessage.Send.Group`

> [!hint] The permissions are in the 9th, 10th, and 11th positions. Press 9 to jump to the 9th position, then select that permission (using space) and the next two. Use ↑ / ↓ to navigate, Space to select each permission, and Enter to submit once all three permissions are selected.

!IMAGE[6wdcu2et.jpg](instructions361584/6wdcu2et.jpg)

8. [] Apply the RSC permissions, by entering *Y*.

---
### Step 3: Add your agent to a channel & run your agent
> This step may seem repetitive, everytime you change permissions on your agent, it needs to be restarted and reinstalled to grant the new permissions. 

1. [] Navigate **Back** > select **Get App Details**.

2. [] Select > **Install in Teams** > **Use the web app instead**.

3. [] Install the app in the **Ignite Demo Team**.

!IMAGE[2rd4j84k.jpg](instructions361584/2rd4j84k.jpg)
!IMAGE[jziu8vb0.jpg](instructions361584/jziu8vb0.jpg)

4. [] Minimize Edge and open **VS Code**

5. [] In the Terminal, start the server by running

    +++npm run dev+++

6. [] Wait a ~1 min until the terminal displays

    ++listening on port 3978 🚀++

---

### Step 4: Enable @mentions, thread reading, and summaries.

1. [] Go back to Edge and open the **Ignite Team**, which already contains conversation threads.
2. [] Navigate to the top of the messages.
3. [] Start by @mentioning the agent in a Teams thread.

   !IMAGE[prsyv437.jpg](instructions361584/prsyv437.jpg)

4. [] In the thread titled *"Can we review the contract....."*, ask the agent to summarize the conversation and provide the next steps. Use the following prompt (press Send after typing the prompt):

   +++Can you summarize this thread and give the next steps ?+++

>[!hint] Make sure to @ mention and resolve the Agent name first before giving this prompt
!IMAGE[vltgs6ph.jpg](instructions361584/vltgs6ph.jpg)

5. [] The agent should acknowledge the message by replying with an emoji.
6. [] Once processing is complete, the agent will reply with a summary of the conversation.

	!IMAGE[dq15bn62.jpg](instructions361584/dq15bn62.jpg)

---

### Step 5: Test external data fetching

1. [] In the same conversation thread, @mention the agent and ask it to fetch the contract details using the following prompt:

   +++Can you fetch net-60 Contract Details ?+++

> [!hint] Make sure to @mention the agent and select its name from the suggestions before submitting the prompt.

2. [] The agent **should refuse** this request because it is not yet connected to an external MCP server.

   !IMAGE[1vfbtepe.jpg](instructions361584/1vfbtepe.jpg)

> [!hint] This is expected behavior, as we've defined the agent's instructions in `src\azureOpenAi\agentInstruction.md`. If the agent cannot retrieve external information, it should not hallucinate or make things up. In the next exercise, we'll connect the agent to MCP.

---

### Step 6: Cleanup

1. [] After completing the tests, perform the following cleanup steps:
   - Close the terminal window running the Teams CLI.
   - Close the terminal window runnng the Helper Conversation Script.
   - In VS Code, stop the locally running agent process by pressing **Ctrl + C** in the terminal.
   - **Do not close the Dev Tunnel terminal.** Keep it running for the remainder of the lab.

---

============================================================================

## Exercise 3: Connect the Agent to External Tools

In this exercise, you will connect the agent to an MCP server to enable it to interact with the Contract Management System.

---

### Step 1: Explore the Contract Management System
> Zava Contract Services is an MCP server running locally on your machine. Outside of this lab, you would use a public MCP server or one you create and host yourself to enable the use of available MCP tools.

1. [] Open the **Start Menu** and launch **Zava Contract Services**.

 !IMAGE[vod2mpth.jpg](instructions361584/vod2mpth.jpg)

2. [] Open a browser and navigate to +++localhost:3000+++
3. [] Explore the UI of Contract Management application, make sure you can see the list of contracts.

	!IMAGE[fvwsjclh.jpg](instructions361584/fvwsjclh.jpg) 

---

### Step 2: Connect the MCP Server to the Agent
> In this step, the connection to the MCP server has been written for you (lines 20 & 21), uncommenting the code allows the agent to run the connection now that you have started the server in Step #1 above. 

1. [] Open the agent codebase in VS Code.
2. [] Open the `index.ts` file.
3. [] Uncomment lines 20 and 21 to connect the MCP server to the application.

   !IMAGE[8gp8vfgr.png](instructions361584/8gp8vfgr.png)

4. [] Save the file (use CTRL + S).
5. [] Start the server by running the following command in the terminal:

   +++npm run dev+++

6. [] Wait a ~1 min until the terminal displays

    ++listening on port 3978 🚀++

---

### Step 3: Fetch Contracts from an External Source
> In this step, we are explicitly telling our agent to use the MCP tools available to it, this helps us ensure consistentcy in the lab envrionemnt. Outside of this lab, your agent will reason over request and decide whether using MCP tools makes sense in those cases. 

1. [] Return to the same thread from the previous exercise, where the agent was unable to fetch the contract details.
2. [] Ask the agent to fetch the details again using the prompt shown below.
  
   +++try again and fetch net-60 details, retry the tool call+++

	
> [!hint] Make sure to @mention the agent and select its name from the suggestions before submitting the prompt.

   !IMAGE[8hra1z67.jpg](instructions361584/8hra1z67.jpg)


3. [] This time, the agent should be able to fetch the contract details from the contract management system.

   !IMAGE[uzjihndx.jpg](instructions361584/uzjihndx.jpg)

4. [] **(Optional)** Open the UI in a separate tab by navigating to +++localhost:3000+++. Verify the accuracy of the retrieved details by comparing the Net-60 contract details displayed in the UI with the data retrieved by the agent.


---

### Step 4: Create New Contracts

1. [] Look for thread in the channel, created by **Alex Wilber**.
2. [] Ask the agent by @ mentioning, to create a new contract item using the following prompt:

   +++Can you create a new contract item for this request and update here ?+++

> [!hint] Make sure to @mention the agent and select its name from the suggestions before submitting the prompt.

   !IMAGE[t4l7skg5.jpg](instructions361584/t4l7skg5.jpg)

3. [] The agent should create a new item and respond in the thread.

   !IMAGE[u7i9n2hh.jpg](instructions361584/u7i9n2hh.jpg)

4. [] **(Optional)** Open the UI in a separate tab to verify that the item was created successfully.  Open +++localhost:3000+++ and verify the new item.

	!IMAGE[ld1kshfz.jpg](instructions361584/ld1kshfz.jpg)

---

============================================================================

## Exercise 4: Generate Interactive Adaptive Cards

In this exercise, you will enhance the agent to generate interactive Adaptive Cards in Microsoft Teams. When you @mention the agent and ask it to create a card, it will render an interactive card that users can interact with and update dynamically.

The Card will contain 5 action Items based on the Thread context, and users will be able to update the card realtime

---

### Step 1: Test Interactive Adaptive Cards

> In this step, we'll use different accounts so you can see how adaptive cards can be utilized by users across your organization. 

1. [] Find the thread in the channel created by **Megan Bowen**.
2. [] @mention the agent and enter exactly `card`

   !IMAGE[o1alylm1.jpg](instructions361584/o1alylm1.jpg)

3. [] The agent should respond with an interactive Adaptive Card.

   !IMAGE[os26jaq0.jpg](instructions361584/os26jaq0.jpg)

4. [] Interact with the generated card by clicking the buttons. The card should update to mark the selected items as **Done**.

5. [] From the **Start Menu**, open the Microsoft Edge profile for +++AlexW+++ and navigate to Teams using the following URL:

   +++https://aka.ms/mstfw+++

6. [] Sign in using the following credentials:

   - **Username:** +++AlexW@@lab.CloudCredential(WWLM365E7).TenantName+++
   - **TAP:** +++@lab.Variable(TAP)+++

   Skip any Teams messages prompting you to install the app or complete the getting-started steps.

7. [] Navigate to the same team, **Ignite Demo Team**, and interact with the card.

8. [] Notice how updates made to the card by one user are reflected for the other user as well.

   !IMAGE[plrtrq6h.jpg](instructions361584/plrtrq6h.jpg)

---

### Step 2: Explorie Adaptive Card Rendering logic

1. [] Open the agent codebase in VS Code and navigate to `statusTrackerCard`, where the Adaptive Card rendering logic is already implemented. Review the files in this folder to understand how the functionality is organized and identify the components involved in preparing and displaying the card.

2. [] Open `eventHandler.ts` and look around line 62 for the logic that handles the `card` prompt. Examine how the incoming message is checked and follow the function call that routes the request to the status tracker flow.

3. [] Open `trackerOrchestrator.ts` and review the functions invoked by the event handler. Follow their execution order to understand how the orchestrator coordinates the flow and passes information to the card rendering components.

4. [] Follow the relevant calls into the rendering logic inside `statusTrackerCard`. Explore how the card's layout, content, and displayed values are assembled, then trace how the completed card is returned or sent to the conversation.

---

============================================================================

## Exercise 5: Send Targeted Messages from Adaptive Card Interactions

In this exercise, you will enhance the agent to send targeted messages when a user interacts with an Adaptive Card in Microsoft Teams. The response should be visible only to the user who performed the action, without notifying other participants in the conversation.

---

### Step 1: Explore the Targeted Messaging Logic

1. [] In VS Code, stop the running server by pressing **Ctrl + C** in the terminal.
2. [] Open `src\statusTrackerCard\statusTrackerCard.ts` and uncomment **line 85**.

   !IMAGE[rxc8uh88.jpg](instructions361584/rxc8uh88.jpg)

3. [] The implementation for targeted messaging is already available in `src\targetedMessage\sendActionRecordedMessage.ts`.
4. [] Start the server by running the following command in the terminal:

   +++npm run dev+++

5. [] Wait approximately one minute for the terminal to display:

   ++listening on port 3978 🚀++

---

### Step 2: Test Targeted Messages

1. [] Have either user interact with the card again.

2. [] After the interaction, a targeted message should be posted. This message should be visible only to the user who interacted with the card.

3. [] Verify this with the other user. The targeted message should not be visible to them.

!IMAGE[g1cb9hsb.jpg](instructions361584/g1cb9hsb.jpg)

---


============================================================================

## Conclusion and Key Takeaways

Congratulations! You've succesfully completed Lab 651: Build AI Agents in Teams. :tada:

In this lab you accomplished the following: 

- Built a **Microsoft Teams agent** that evolved from a local prototype into a collaborative teammate capable of working directly within Teams conversations.

- Configured an **Azure OpenAI** LLM resource on **Microsoft Foundry** and integrated the resource into your agnet. 

- Used the **Teams CLI** and **Dev Tunnel** to create, configure, sideload, and test the agent in Microsoft Teams.

- Used **Socket Mode** to connect your agent to Teams during development.

- Enabled collaboration through **Resource-Specific Consent (RSC)** permissions, allowing the agent to participate in channels, read conversation threads, respond to @mentions, and generate contextual summaries and next steps.

- Connected the agent to an external **Contract Management System using MCP**, enabling it to retrieve contract information and create new contract records from natural-language requests.

- Implemented **Adaptive Cards** that transform conversation context into a shared five-item checklist that multiple channel members can interact with and update in real time.

- Explored how Adaptive Card events are orchestrated, rendered, and synchronized across users.

- Added **targeted messaging**, enabling the agent to acknowledge individual users' card interactions privately without notifying other channel members.

Today's lab gives you hands on experience building collabroative agents while giving you a resuable base agent you can customize to meet your business needs. 


============================================================================

## Troubleshooting

### In Case Azure AI Foundry Setup Fails
1. If the Azure AI Foundry setup fails, you will see a screen pre-filled with default values.
2. Click **Create** and ensure the **Set up recommended...** toggle is **OFF**.
!IMAGE[qur9oqte.jpg](instructions361584/qur9oqte.jpg)


### In case devtunnel is stopped
1. Run the devtunnel again on the same white terminal window
2. +++wt --focus --size 100,30 new-tab --colorScheme "Tango Light" powershell -NoExit -Command "devtunnel host -p 3978 --allow-anonymous"; exit;+++
3. Get the new terminal endpoint, (Connect via browser url)
4. Goto Teams CLI
5. +++teams app+++
6. Select App > Select Zava-Help-Desk
7. Update App > Endpoint > Enter the new tunnel endpoint with /api/messages
8. Example: https://DOMAIN.asse.devtunnels.ms/api/messages
9. On Cli > Navigate Back
10. Get app details
11. Install in Teams