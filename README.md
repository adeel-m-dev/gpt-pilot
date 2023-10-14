# 🧑‍✈️ GPT PILOT
### GPT Pilot helps developers build apps 20x faster

When you want to build an app, you can specify the type of app you'd like to create using the GPT Pilot. The Pilot will then ask you questions to clarify your requirements, create the necessary product and technical specifications, set up the environment, and start coding the app step by step, much like a real developer. You'll oversee the development process and be asked to review each task the Pilot completes or to provide assistance when it needs help. Essentially, the GPT Pilot will act as a coder while you function as a lead developer who reviews code and provides help when necessary.

---

<!-- TOC -->
* [🔌 Requirements](#-requirements)
* [🚦How to start using gpt-pilot?](#how-to-start-using-gpt-pilot)
    * [🐳 How to start gpt-pilot in docker?](#-how-to-start-gpt-pilot-in-docker)
* [🧑‍💻️ CLI arguments](#%EF%B8%8F-cli-arguments)
* [🔎 Examples](#-examples)
    * [Real-time chat app](#-real-time-chat-app)
    * [Markdown editor](#-markdown-editor)
    * [Timer app](#-timer-app)
* [🏛 Main pillars of GPT Pilot](#-main-pillars-of-gpt-pilot)
* [🏗 How GPT Pilot works?](#-how-gpt-pilot-works)
* [🕴How's GPT Pilot different from _Smol developer_ and _GPT engineer_?](#hows-gpt-pilot-different-from-smol-developer-and-gpt-engineer)
* [🍻 Contributing](#-contributing)
* [🔗 Connect with us](#-connect-with-us)
* [🌟 Star history](#-star-history)
<!-- TOC -->

---

GPT Pilot aims to research how much GPT-4 can be utilized to generate fully working, production-ready apps while the developer oversees the implementation.

**The main idea is that AI can write most of the code for an app (maybe 95%), but for the rest, 5%, a developer is and will be needed until we get full AGI**.

I've broken down the idea behind GPT Pilot and how it works in the following blog posts:

**[[Part 1/3] High-level concepts + GPT Pilot workflow until the coding part](https://blog.pythagora.ai/2023/08/23/430/)**

**_[[Part 2/3] GPT Pilot coding workflow](https://blog.pythagora.ai/2023/09/04/gpt-pilot-coding-workflow-part-2-3/)_**

**_[Part 3/3] Other important concepts and future plans (COMING UP)_**

---


<div align="center">

### **[👉 Examples of apps written by GPT Pilot 👈](#-examples)**

</div>

<br>

https://github.com/Pythagora-io/gpt-pilot/assets/10895136/0495631b-511e-451b-93d5-8a42acf22d3d

# 🔌 Requirements

- **Python 3**
- **PostgreSQL** (optional, projects default is SQLite)
   - DB is needed for multiple reasons like continuing app development. If you have to stop at any point or the app crashes, go back to a specific step so that you can change some later steps in development, and easier debugging, in future we will add functionality to update project (change some things in existing project or add new features to the project and so on)...


# 🚦How to start using gpt-pilot?
After you have Python and PostgreSQL installed, follow these steps:
1. `git clone https://github.com/Pythagora-io/gpt-pilot.git` (clone the repo)
2. `cd gpt-pilot`
3. `python -m venv pilot-env` (create a virtual environment)
4. `source pilot-env/bin/activate` (or on Windows `pilot-env\Scripts\activate`) (activate the virtual environment)
5. `pip install -r requirements.txt` (install the dependencies)
6. `cd pilot`
7. `mv .env.example .env` (create the .env file)
8. Add your environment (OpenAI/Azure), your API key, and the SQLite/PostgreSQL database info to the `.env` file
   - to change from SQLite to PostgreSQL in your .env, just set `DATABASE_TYPE=postgres`
9. `python db_init.py` (initialize the database)
10. `python main.py` (start GPT Pilot)

After, this, you can just follow the instructions in the terminal.

All generated code will be stored in the folder `workspace` inside the folder named after the app name you enter upon starting the pilot.

**IMPORTANT: To run GPT Pilot, you need to have PostgreSQL set up on your machine**
<br>

## 🐳 How to start gpt-pilot in docker?
1. `git clone https://github.com/Pythagora-io/gpt-pilot.git` (clone the repo)
2. Update the `docker-compose.yml` environment variables, which can be done via `docker compose config`
3. run `docker compose build`. this will build a gpt-pilot container for you.
4. run `docker compose up`.
5. access the web terminal on `port 7681`
6. `python db_init.py` (initialize the database)
7. `python main.py` (start GPT Pilot)

This will start two containers, one being a new image built by the `Dockerfile` and a Postgres database. The new image also has [ttyd](https://github.com/tsl0922/ttyd) installed so that you can easily interact with gpt-pilot. Node is also installed on the image and port 3000 is exposed.


# 🧑‍💻️ CLI arguments

## `app_type` and `name`
If not provided, the ProductOwner will ask for these values:

`app_type` is used as a hint to the LLM as to what kind of architecture, language options and conventions would apply. If not provided, `prompts.prompts.ask_for_app_type()` will ask for it.

See `const.common.ALL_TYPES`: 'Web App', 'Script', 'Mobile App', 'Chrome Extension'


## `app_id` and `workspace`
Continue working on an existing app using **`app_id`**
```bash
python main.py app_id=<ID_OF_THE_APP>
```

_or_ **`workspace`** path:

```bash
python main.py workspace=<PATH_TO_PROJECT_WORKSPACE>
```

Each user can have their own workspace path for each App.


## `user_id`, `email`, and `password`
These values will be saved to the User table in the DB.

```bash
python main.py user_id=me_at_work
```

If not specified, `user_id` defaults to the OS username but can be provided explicitly if your OS username differs from your GitHub or work username. This value is used to load the `App` config when the `workspace` arg is provided.

If not specified `email` will be parsed from `~/.gitconfig` if the file exists.

See also [What's the purpose of arguments.password / User.password?](https://github.com/Pythagora-io/gpt-pilot/discussions/55)


## `advanced`
The Architect, by default, favors certain technologies, including: 

- Node.JS
- MongoDB
- PeeWee ORM
- Jest & PyUnit
- Bootstrap
- Vanilla JavaScript
- Socket.io

If you have your own preferences, you can have a deeper conversation with the Architect.

```bash
python main.py advanced=True
```


## `step`
Continue working on an existing app from a specific **`step`** (eg: `user_tasks`)
```bash
python main.py app_id=<ID_OF_THE_APP> step=<STEP_FROM_CONST_COMMON>
```


## `skip_until_dev_step`
Continue working on an existing app from a specific **development step**
```bash
python main.py app_id=<ID_OF_THE_APP> skip_until_dev_step=<DEV_STEP>
```
This is basically the same as `step` but during the development process. If you want to play around with gpt-pilot, this is likely the flag you will often use.
<br>

Erase all development steps previously done and continue working on an existing app from the start of development.

```bash
python main.py app_id=<ID_OF_THE_APP> skip_until_dev_step=0
```


## `delete_unrelated_steps`


## `update_files_before_start`



# 🔎 Examples

Here are a couple of example apps GPT Pilot created by itself:

### 📱 Real-time chat app
- 💬 Prompt: `A simple chat app with real-time communication`
- ▶️ [Video of the app creation process](https://youtu.be/bUj9DbMRYhA)
- 💻️ [GitHub repo](https://github.com/Pythagora-io/gpt-pilot-chat-app-demo)


### 📝 Markdown editor
- 💬 Prompt: `Build a simple markdown editor using HTML, CSS, and JavaScript. Allow users to input markdown text and display the formatted output in real-time.`
- ▶️ [Video of the app creation process](https://youtu.be/uZeA1iX9dgg)
- 💻️ [GitHub repo](https://github.com/Pythagora-io/gpt-pilot-demo-markdown-editor.git)


### ⏱️ Timer app
- 💬 Prompt: `Create a simple timer app using HTML, CSS, and JavaScript that allows users to set a countdown timer and receive an alert when the time is up.`
- ▶️ [Video of the app creation process](https://youtu.be/CMN3W18zfiE)
- 💻️ [GitHub repo](https://github.com/Pythagora-io/gpt-pilot-timer-app-demo)

<br>

# 🏛 Main pillars of GPT Pilot:

Creating a fully functional app with the help of AI requires the involvement of a developer in the app creation process. The developer needs to have the ability to make changes to the code at any given moment, and the AI needs to continue functioning with those changes. This could include adding an API key or fixing any issues that arise if the AI gets stuck.

The app needs to be written step-by-step in the same way a developer would write it. When a developer creates an app, they don't write out the entire codebase at once. Instead, they implement the basic features, run them, and debug any issues that arise before moving on to the next task. The same approach should be used when AI codes. The AI will make mistakes, so it needs to be developed step-by-step, just like a developer would code it. For example, the developer should set up routes, add a database connection, etc.

The approach to building the app with the help of AI needs to be scalable to create a production-ready app. There are three important aspects to consider:

1. Context rewinding - the context size of the first message to the LLM has to be relatively the same for solving each development task. This means that the conversation needs to be rewound to the first message upon each task. 

2. Recursive conversations - LLM conversations need to be set up to be used "recursively." If GPT Pilot detects an error, it needs to debug it. If another error occurs during the debugging process, GPT Pilot needs to stop debugging the first issue, fix the second one and then return to fixing the first issue. The process works by rewinding the context and explaining each error in the recursion separately. Once the deepest level error is fixed, we move up in the recursion and continue fixing that error. We do this until the entire recursion is completed.

3. TDD (Test Driven Development) - GPT Pilot needs to create new code without breaking previously written code. The best way to do this is to work with TDD methodology. For each code that GPT Pilot writes, it needs to write tests that check if the code works as intended so that all previous tests can be run whenever new changes are made.

In summary, the idea is that AI cannot create apps from scratch without the developer's involvement. An interactive tool has been created that generates code but also requires the developer to check each step to understand what's going on and for the AI to have a better overview of the entire codebase. While AI cannot create a production-ready app on its own, the general concept of how this could work is there.

# 🏗 How GPT Pilot works?
Here are the steps GPT Pilot takes to create an app:

![GPT Pilot workflow](https://github.com/Pythagora-io/gpt-pilot/assets/10895136/d89ba1d4-1208-4b7f-b3d4-76e3ccea584e)

1. You enter the app name and the description.
2. **Product Owner agent** asks a couple of questions to understand the requirements better.
3. **Product Owner agent** writes user stories and asks you if they are all correct (this helps it create code later on).
4. **Architect agent** writes up technologies that will be used for the app.
5. **DevOps agent** checks if all technologies are installed on the machine and installs them if not.
6. **Tech Lead agent** writes up development tasks that the Developer must implement. This is an important part because, for each step, the Tech Lead needs to specify how the user (real-world developer) can review if the task is done (e.g. open localhost:3000 and do something).
7. **Developer agent** takes each task and writes up what needs to be done to implement it. The description is in human-readable form.
8. Finally, the **Code Monkey agent** takes the Developer's description and the existing file and implements the changes. We realized this works much better than giving it to the Developer right away to implement changes.

For more details on the roles of agents employed by GPT Pilot, please take a look at [AGENTS.md](https://github.com/Pythagora-io/gpt-pilot/blob/main/pilot/helpers/agents/AGENTS.md)

![GPT Pilot Coding Workflow](https://github.com/Pythagora-io/gpt-pilot/assets/10895136/53ea246c-cefe-401c-8ba0-8e4dd49c987b)


<br>

# 🕴How's GPT Pilot different from _Smol developer_ and _GPT engineer_?

- **GPT Pilot works with the developer to create a fully working production-ready app** - I don't think AI can (at least in the near future) create apps without a developer being involved. So, **GPT Pilot codes the app step by step** just like a developer would in real life. This way, it can debug issues as they arise throughout the development process. If it gets stuck, you, the developer in charge, can review the code and fix the issue. Other similar tools give you the entire codebase at once - this way, bugs are much harder to fix for AI and for you as a developer.
  <br><br>
- **Works at scale** - GPT Pilot isn't meant to create simple apps but rather so it can work at any scale. It has mechanisms that filter out the code, so in each LLM conversation, it doesn't need to store the entire codebase in context, but it shows the LLM only the relevant code for the current task it's working on. Once an app is finished, you can continue working on it by writing instructions on what feature you want to add.

# 🍻 Contributing
If you are interested in contributing to GPT Pilot, I would be more than happy to have you on board and also help you get started. Feel free to ping [zvonimir@pythagora.ai](mailto:zvonimir@pythagora.ai), and I'll help you get started.

## 🔬️ Research
Since this is a research project, there are many areas that need to be researched on both practical and theoretical levels. We're happy to hear how the entire GPT Pilot concept can be improved. For example, maybe it would work better if we structured functional requirements differently, or maybe technical requirements need to be specified in a different way.

## 🖥 Development
Other than the research, the GPT Pilot needs to be debugged to work in different scenarios. For example, we realized that the quality of the code generated is very sensitive to the size of the development task. When the task is too broad, the code has too many bugs that are hard to fix, but when the development task is too narrow, GPT also seems to struggle in getting the task implemented into the existing code.

# 🔗 Connect with us
🌟 As an open-source tool, it would mean the world to us if you starred the GPT-pilot repo 🌟

💬 Join [the Discord server](https://discord.gg/HaqXugmxr9) to get in touch.


# 🌟 Star History

[![Star History Chart](https://api.star-history.com/svg?repos=Pythagora-io/gpt-pilot&type=Date)](https://star-history.com/#Pythagora-io/gpt-pilot&Date)
