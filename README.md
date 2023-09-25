# tinyagent

This is a minimal software developer agent based on gpt-4 that really works.

It writes unit tested software. It iterates to solve problems, and asks for user input
and guidance as needed.

I recommend starting it and then putting your program spec in a spec.txt file in the
agent's working directory. Then tell it you've put a spec there, and ask it to create
a plan.txt containing its plan. If you're happy with that, tell it to go ahead.

You can use the --start_dir argument to point it to an existing directory to work in.
I recommend telling it to read spec.txt and plan.txt to figure out what it should do.
You can use this capability to stop and restart the agent, and even modify the agent
(its system instructions, or the code here) in between executions.

WARNING: there are no safety checks here. This agent can run arbitrary commands on
your system. In practice I have not seen it try to do anything out of its working.
But I expect you could get it to do something bad if you tried.

## Trying it

```
$ python tinyagent.py
Working dir:  /tmp/agent_20230925_113745
ENTER TO START

<press enter>

User: <type this> write a python function that can determine if a chess board is in checkmate. you should keep the problem spec in spec.txt
```

## Pip deps

The agent will want to run pytest, and the agent itself needs the openai library. It will probably ask you before
installing anything, and can install these in your global python environment if you want.

I use pyenv, and run it like this:

```
pyenv virtualenv 3.10.7 tinyagent
PYENV_VERSION=tinyagent pip install openai pytest
PYENV_VERSION=tinyagent OPENAI_API_KEY=<key> python tinyagent.py
```

## Resuming the agent

You can pass --start_dir to tell the agent to work in an existing directory. For larger problems, you should tell your agent to write a spec.txt or a plan.txt (or you can add spec.txt to the directory yourself). Tell it to update its plan as it goes. That way its easier for subsequent agent runs to get up to speed in the repo.

# TODOs

- when we reach the context limit, we typically get a truncated response from the agent, and since we don't receive a function call, we
  ask the user for input. You can just type "go on" if it seems like the agent has provided a truncated response, and it will resolve.
- rather than asking the agent to manage the git repo, it might be nice to just commit everything the agent does at any step. That way
  we have a clear history of it's changes
- bring back plan.txt and spec.txt as part of the system prompt
