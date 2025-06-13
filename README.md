# 🧠 agenticai-camunda-chatgpt

This is a **“For Dummies”-style hands-on tutorial** for experimenting with Agentic AI using the Camunda 8 Process Automation platform.

By the end of this session, you'll have built a simplified insurance claims process that demonstrates:

* Image classification using ChatGPT
* Image-to-text conversion
* Contextual summarisation with GenAI
* Retrieval-Augmented Generation (RAG) for business rule alignment
* Automated decision-making (i.e. *inference* — the “A” in Agentic AI)

![Tutorial](<Assets/Tutorial Overview.gif>)

---

## 📋 Quick Facts

* **Estimated Duration:** 2–3 hours
* **Target Audience:** Process Managers, Enterprise Architects, Automation Experts, AI Architects
* **Pre-requisites:**

  * Ability to install software (JDK, Camunda)
  * A paid [ChatGPT subscription](https://chat.openai.com/) with API access and some credit (a few cents will do)

> ⚠️ This guide was developed on macOS. If you're using Windows or Linux, the core steps still apply — feel free to ask ChatGPT to adapt this README to your platform.

---

## 👨‍🏫 About the Author

**LinkedIn:** [linkedin.com/in/lukeaudie](https://www.linkedin.com/in/lukeaudie/)

Luke is a transformational technologist with 15+ years of experience spanning the UK, Europe, and Australia. He specialises in business architecture, automation, and solution design across industries like financial services, public sector, and telecommunications. Having held senior roles at leading consulting and tech firms, Luke combines deep technical expertise with strategic leadership, enabling organisations to modernise legacy processes with fresh new ways of working.

---

## 📂 Contents

1. [Getting Set Up](#1-getting-set-up)
2. [Quick Context & Overview](#2-quick-context--overview)
3. [Hello World — Quick Hands-On](#3-quick-end-2-end-hello-world)
4. [Exercise - Build an Automated Claims Process](#4-exercise---build-an-automated-claims-process)
5. [Summary](#5-summary)

---

# 1. Getting Set Up

## 1.1 Clone This Tutorial

Open Terminal and run:

```bash
cd ~/Projects  # or any folder you prefer
git clone https://github.com/ch-lukas/agenticai-camunda.git
cd agenticai-camunda
```

💡 Suggest using [Visual Studio Code](https://code.visualstudio.com/). Once the project is open, right-click `README.md` and select **"Open Preview"** to follow along.

---

## 1.2 Install JDK 21

Check your Java version:

```bash
java -version
```

If it's below version 21, install via Homebrew:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install openjdk@21
```

Then configure your environment:

```bash
nano ~/.zshrc
```

Add:

```bash
export JAVA_HOME="/opt/homebrew/opt/openjdk@21"
export PATH="$JAVA_HOME/bin:$PATH"
```

Save and reload:

```bash
source ~/.zshrc
java -version
```

---

## 1.3 Install Camunda Platform

You’ll need two tools: the **Camunda Run Server** and the **Camunda Modeler**.

### 1.3.1 Camunda Run (Server)

Download from: [Camunda Downloads](https://camunda.com/download/)

Direct macOS link: [Camunda 8.7 Darwin ZIP](https://downloads.camunda.cloud/release/camunda/c8run/8.7/camunda8-run-8.7-darwin-x86_64.zip)

Once unzipped:

```bash
cd c8run
./start.sh
```

Wait \~30 seconds. You'll see URLs and default credentials:

```
Username: demo
Password: demo

Operate:                http://localhost:8080/operate
Tasklist:               http://localhost:8080/tasklist
Camunda API:            http://localhost:8080/v2/
...
```

🔍 Tip: Use **"New Terminal at Folder"** from Finder for easy access.

---

### 1.3.2 Camunda Modeler

Download here: [Camunda Modeler](https://camunda.com/download/modeler/)

Use this to design BPMN processes and forms.

---

## 1.4 Set Up ChatGPT API Access

A Plus or Pro (paid) OpenAI account is required for API access. Here’s how to get set up:

1. Visit [platform.openai.com](https://platform.openai.com/)
2. Go to **Settings** > **API Keys** ([Direct link](https://platform.openai.com/settings/organization/api-keys))
3. Generate an API key and assign it to a project
4. Save the key securely — you’ll need it for the process
5. Add some credit: [Billing Page](https://platform.openai.com/settings/organization/billing/overview)
6. For more help on this, visit - [https://help.openai.com/en/articles/9186755-managing-projects-in-the-api-platform](https://help.openai.com/en/articles/9186755-managing-projects-in-the-api-platform)

💸 Add the minimum amount — this tutorial uses only a few cents.

---

# 2. Quick Context & Overview

## 2.1 BPMN Diagrams with Camunda

**BPMN** (Business Process Model and Notation) is the industry standard for modelling business workflows. It’s both business- and tech-friendly.

![BPMN Diagram](<Assets/Camunda Modeler-BPMN Diagrams.jpg>)

---

## 2.2 Building Forms in Camunda

While Camunda is often used as a **headless orchestration engine**, the **built-in form designer** is ideal for rapid prototyping and internal use cases.

![Camunda Forms](<Assets/Camunda Modeler-Forms.jpg>)

---

## 2.3 Tasklist — User Tasks

User Tasks in BPMN are activities that require human input. When the process reaches such a task, a notification can be sent, and users complete the task via the **Tasklist UI**.

* URL: [http://localhost:8080/tasklist](http://localhost:8080/tasklist)
* Login: `demo / demo`

![Tasklist](<Assets/Camunda Modeler-Task List.jpg>)

---

## 2.4 Operate — Process Monitoring

Camunda **Operate** is the monitoring dashboard for workflows. You can track progress, investigate issues, and view process states.

* URL: [http://localhost:8080/operate](http://localhost:8080/operate)
* Login: `demo / demo`

![Operate](<Assets/Camunda Modeler-Operate.jpg>)

---

# 3. Quick End-2-End Hello World

🌟 *Coming up next: A hands-on "Hello World" project — or rather, a quick capital city question to illustrate AI-driven process automation.*

---

## Overview

This simple process takes a human input (i.e. Country name), identifies the capital city, and adjusts the process flow accordingly.

This illustrates how dynamic data and context can influence the way people experience processes — which is what process improvement seeks to standardise and automate.

The process has three steps:

1. A human task where the user enters a country name (e.g. "Ireland", "France", or any other — unlisted values trigger a fallback path)
2. ChatGPT is used to determine the capital city
3. The workflow proceeds based on the city identified

![Process Overview](<Exercises/0 - Hello World/Assets/Process Overview.png>)

## Exercise

1. Open **Camunda Modeler** and load `What is the Countries Capital.bpmn` from `Exercises > 0 - Hello World`.
2. Ensure the **right-hand properties panel** is visible.
3. Click the "What is the Capital?" step.
4. In the configuration panel, paste your **OpenAI API key**.
5. Review additional fields like "Model" and "Prompt".
6. Save the model.

![Modeler Step](<Exercises/0 - Hello World/Assets/Screenshot1.png>)

7. Ensure the Camunda server is running (`./start.sh` inside `c8run`).
8. In Modeler, first click the **Rocket icon** to deploy the model then -> Click **Play icon** at the bottom → click **Next**.

N.B. Everytime you update and save a model you will need to the deploy it again.

![Deploy and Run](<Exercises/0 - Hello World/Assets/Screenshot2.png>)

9. Click **Start** — you’ll see a confirmation message that the instance has started.
10. Open **Operate** to observe the process:
    [http://localhost:8080/operate/processes?active=true\&incidents=true\&completed=true\&canceled=true](http://localhost:8080/operate/processes?active=true&incidents=true&completed=true&canceled=true)
11. Click the most recent process instance (should be green if successful).
12. Observe the **blue progress line** and the current active step (likely the human task).

![Process Monitor](<Exercises/0 - Hello World/Assets/Screenshot3.png>)

13. Go to [Tasklist](http://localhost:8080/tasklist)

14. Click the latest task → **Assign to me**

15. Enter a country name (e.g. Ireland, France, or anything else)

16. Click **Complete Task**

17. Back in **Operate**, refresh the instance — you should see the blue line reach the end, indicating full process completion.

![Completion Screenshot](<Exercises/0 - Hello World/Assets/Screenshot4.png>)

---

## 🧪 Try This!

ChatGPT is impressively robust. Try entering a **misspelt** country name like `Eirlnd`, or a **foreign-language** version. The model may still infer the correct capital (e.g. Dublin) — a perfect illustration of LLM inference and flexibility!

# 4. Exercise - Build an Automated Claims Process

🌟 *Coming up next: A hands-on walkthrough of an Insurance Claim process utilising Visio AI, NLP, Gen AI and most importantly - automated reasoning.*

---

## Overview

This process simulates an automated claims workflow that leverages both visual and generative AI components to arrive at an automated decision.

It includes four key steps:

1. **Analyse Claim Image** — uses AI vision to analyse the submitted image.
2. **Match Image to Registered Vehicle** — compares extracted image data to the client's vehicle records.
3. **Import Processing Rule** — placeholder to simulate policy/business rule alignment.
4. **Decide on Claim** — evaluates all data to approve or reject the claim.

N.B. When orchestrated together, these components showcase the true power of Agentic AI for straight-through-processing.

![Process](<Exercises/1 - Insurance Claim/Assets/Process Overview.png>)

## Exercise

1. Open **Camunda Modeler** and load `Insurance Claim.bpmn` from `Exercises > 1 - Insurance Claim`.
2. Configure each ChatGPT-enabled task by inserting your API key.
3. Refer to the model configuration panels to inspect prompts and expected formats.

![Fields](<Exercises/1 - Insurance Claim/Assets/Screenshot2.png>)

4. Deploy and execute:

   * Click the **Rocket icon** to deploy
   * Click the **Play icon**, and paste in JSON data from `customer_and_claim_data.json`

![JSON](<Exercises/1 - Insurance Claim/Assets/Screenshot3.png>)

Before running, examine the input JSON as it drives all downstream AI behaviour.

![Data](<Exercises/1 - Insurance Claim/Assets/Screenshot4.png>)

5. Open [Operate](http://localhost:8080/operate) to monitor the process.
6. Click the grey background in Operate to inspect the global variables.

![Variables](<Exercises/1 - Insurance Claim/Assets/Screenshot5.png>)

### Sample Outputs

| Step                   | Variable              | Value                                           |
| ---------------------- | --------------------- | ----------------------------------------------- |
| Analyse Claim Image    | `contextFromImage`    | {"description":"...","accident":"true"}         |
| Match Image to Vehicle | `contextFromMatch`    | {"matched":"true","score":"1.0","notes":"..."}  |
| Decide on Claim        | `contextFromDecision` | {"decision":"true","score":"0.9","notes":"..."} |

---

🔁 **Experiment Further**

Why not re-run the workflow and modify the input JSON to see how the process adapts? Try changing values like the insurance type (e.g. switch to third-party), the licence expiry date, or add some new processing rules. Observe how the flow evolves and pay close attention to the `"contextFromDecision"` variable — it contains useful insight into how and why decisions were made.

⚠️ **Note on Business Rules and AI**

Some claim AI will replace traditional rule-based logic. In my view, it’s an **“and”, not an “or”**. In regulated sectors, clear accountability is essential — someone must always be able to stand behind a decision (including in a court of law). Having worked in banking for many years, I understand the critical need for transparent, auditable processes. I'm happy to chat more about this — just reach out.

# 5. Summary

If you’ve made it this far — fantastic! I’d love to hear your thoughts on automating business processes, Agentic AI, or any feedback you may have to make this tutorial even more engaging. The easiest way to get in touch is via LinkedIn: [https://www.linkedin.com/in/lukeaudie/](https://www.linkedin.com/in/lukeaudie/).

Here are a few final thoughts:

🔍 While this walkthrough includes multiple steps, ChatGPT (or similar models) could potentially handle the entire process in a single prompt. Large Language Models (LLMs) are incredibly powerful.

🔐 Concerned about data sovereignty? Private LLMs might be the answer — and they’re more accessible than you might think.

⚖️ Considering regulatory implications? Agentic AI can enhance your current systems and complement existing business rules without sacrificing accountability.

Thanks again for following along — Luke
