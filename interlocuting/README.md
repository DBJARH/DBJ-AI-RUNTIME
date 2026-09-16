---
version: 0.1
---

# dbj interlocuting

DBJ's AI Runtime simple message bus. One JSON file plus a protocol, so team members on
the same repo can leave each other messages.

By team member we mean all team member: Agents, Harnesses and Humans

## How to

1. top level folder in your repo make and name `.colocuting`
   1. this is a convention right now. but is is reserved and likely will be used by tools
2. in there copy the script `to` you see in here
   1. `./to --help` works
3. in that `.colocuting` folder save the file `colocutor_names.json`. see [`example/colocutor_names.json`](example/colocutor_names.json)
   1. concept is you say who you are when using the `to`, not just to whom, you will send a multiline message that will be saved in message bus json file called `transcript.json`
   2. see [`example/transcript.json`](example/transcript.json)
   2. `to` will not work if there is no `colocutor_names.json` in the same folder
4. Name your harness/agent 
   1. Ask it to repeatedly check  `.colocuting\transcript.json` 
   2. Claude Code `calls` that polling, and uses the `/loop` skill on an interval (e.g. `/loop 30s check .colocuting\transcript.json` ) for changes

Questions, comments; please use that email bellow. Or much simple: the issues section in this repo. Just please prefix your issue with "colocuting:".

> Funny name? "colocuting" is apparently legal archaic word that means "interlocuting". It means "having a conversation". Interlocutor is the one you are having a conversation with.

---

(c) 2026 by dbj@dbj.org | MIT license
