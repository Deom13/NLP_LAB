# OpenCode

## Goal
The goal of this task is to get an understanding of OpenCode by instructing it to implement an Exploratory Data Analysis (EDA) on text data (German doctor reviews to be precise).

## What is OpenCode
OpenCode is an open-source AI coding tool that helps developers write, debug, and refactor code. It lets users interact with AI models in natural language and instruct them to write code or perform other tasks. This interaction can happen in a terminal, web app, desktop app, or IDE.

## Introduction
Go to OpenCode's [web page](https://opencode.ai/) and have a short glance to get a general impression.

You could also watch one of these videos to get an overview of OpenCode:
- [Master OpenCode in 28 minutes](https://www.youtube.com/watch?v=WOOzCHaQipU)
- [OpenCode Tutorial for Beginners](https://www.youtube.com/watch?v=QzqaZshQcJI)
- [Learn OpenCode - Playlist](https://www.youtube.com/watch?v=QnDpKxNe5UA&list=PLAIphj_4bMivPm5KCdvTsR4QP6zLkLJP_)

Or you might want to skim through:
- [OpenCode's Documentation](https://opencode.ai/docs/)
- [How to Use OpenCode](https://docs.kanaries.net/topics/AICoding/opencode-how-to-use)


## Installation
Install OpenCode by following their [installation instruction](https://opencode.ai/docs#installation) that meets your hardware.

Be aware that you have following alternatives to use OpenCode:
- [OpenCode TUI](https://opencode.ai/docs/de/tui/) (terminal user interface)
- [OpenCode Desktop](https://opencode.ai/de/download) (install and run a Desktop GUI)
- [OpenCode Web](https://opencode.ai/docs/web/) (run on a dedicated/isolated hardware providing the same environment to all team members)
- [OpenCode Sandboxed](https://docs.docker.com/ai/sandboxes/agents/opencode/) (using Docker and being more secure - see below)

The simplest approach is to follow the official recommendation and use the TUI.


## First Steps

### Start OpenCode

Open a terminal in your project directory and

```bash
mkdir eda
cd eda
opencode
```

### Configuration
Configure OpenCode and use a [free model](https://opencode.ai/docs/zen/#preise) (e.g. Big Pickle - but be aware that they might use your data to improve their models) or use one of the supported [providers](https://opencode.ai/docs/providers/) (configure and connect with your API key).

### Interact with your Agent
Ask the agent a first question (go into `Plan` mode by using <TAB>).

```bash
What are the specs of this computer?
```

In `Plan` mode the agent asks for permission (e.g. to access a specific folder) in `Build` mode it just proceeds. This can be dangerous as we probably want to keep our secrets (like ssh keys etc.). Go to [permissions](https://opencode.ai/docs/permissions/) to get an impression on how to define permissions. Additionally, it might be worth sandboxing OpenCode with Docker and only mount necessary folders.


### Agents.md
Get an understanding what [AGENTS.md](https://agents.md/) is and what it is used for.

Create an AGENTS.md file for this project. You can ask OpenCode to create an initial AGENTS.md file for you (by using the `/init` command): 

```bash
/init Create an AGENTS.md file for a project that performs an Exploratory Data Analysis (EDA) on text data using python. Use common guidelines and standards used for python projects. Always use a virtual environment to install dependencies and run code. Never access a directory outside the project folder. Ask questions for further guidance or if something is unclear. 
```

Use this AGENTS.md as basis and modify it later based on your needs. Alternatively, you can also use the AGENTS.md file from the `eda_ref` folder.

My experience (pros/cons):
+ The agent asked meaningful questions (python version, where an how to get the data, project structure, analyses I want to perform, ...)
- In addition to generating the AGENTS.md file the agent also performed the complete analysis
- It happened that the agent installed everything into my system Python


## Setup the Project
Ask the agent to setup the project (you might want to do this in `Plan` mode first or directly go into `Build` mode. Here I assume you use the provided AGENTS.md. Otherwise you might need to provide additional instructions.

```bash
Setup the python environment.
```

Download the German Doctor Reviews into a local folder.

```bash
Create a data_loader.py script with a download() function to download the data to a local folder and load() function to load the data from the local folder into a dataframe.
```

```bash
Download the data into the local folder.
```


## EDA

### Analysis of Labels

Ask the agent to perform an analysis of the labels (you might want to use `Plan` mode first, verify the plan, switch to `Build` mode and instruct the agent to proceed).

```bash
Perform an analysis on the distribution of the labels. Create a nice graph.
```

Ask the agent to interpret the distribution of the labels. 

```bash
Provide an interpretation of the label distribution. Discuss the consequences for training a model. Format your answer as markdown and store/append it to the file 'outputs/analysis.md'.
```

### Analysis of ...

Ask the agent to perform another analysis you are interested in...

or

Ask the agent to propose an analysis you could perform on this data...


### Improve Writing 

Ask the agent to do a spelling correction and improve the writing style.

```bash
Take the 'outputs/analysis.md' file and perform a spelling correction and improve the writing style.
```


### PDF Conversion 

Ask the agent to convert the analysis markdown file into a PDF that you can hand in as part of your project.

```bash
Convert the file 'outputs/analysis.md' into 'outputs/analysis.pdf' 
```

The agent finally managed to create a PDF from the Markdown file. However, this process burned quite a few tokens and took a while. As an alternative, you could provide a [tool](https://opencode.ai/docs/de/custom-tools/) that the agent could call to create the PDF. You could even instruct the agent to create a conversion script and bind it into a tool that calls it. 


## Jupyter Notebook

It would be nice to have a Jupyter Notebook that does the EDA (code and analysis). I tried to instruct the agent to create a Jupyter Notebook and it almost succeeded (some cells had errors). In order to succeed, I assume the agent would need the ability to execute Jupyter Notebooks. At the time of writing, this is [not yet integrated natively into OpenCode](https://github.com/anomalyco/opencode/issues/11409). However, there is a [Jupyter MCP Server](https://github.com/datalayer/jupyter-mcp-server) available...
