# Remember

**Make your Claude Code session smarter every time you use it.**

Open Claude Code and say:

```
Fetch https://raw.githubusercontent.com/mmorris35/remember/main/README.md and follow the setup instructions
```

Claude reads this, sets up a memory system, and starts learning about you. Each
session builds on the last. No plugins, no servers, no installs — just markdown
files.

---

## What Changes

Without memory, every Claude Code session is a stranger. It doesn't know your
name. It doesn't know you hate tabs. It doesn't know you've been fighting a
Docker networking bug for three days. Every session, you start from scratch.

With memory, Claude:

- **Adapts to you** — knows your skill level, your stack, your preferences.
  A senior Rust dev gets different answers than a Python beginner.
- **Knows your projects** — remembers what you're building, the tech stack,
  current blockers, what you tried last time.
- **Respects your corrections** — say "I prefer functional style" once. It
  sticks across every future session, every project.
- **Tracks your people** — remembers your teammates, your boss, who owns what.
  When you say "ask Sarah," it knows who Sarah is.
- **Gets better over time** — session 50 is dramatically better than session 1
  because Claude has 50 sessions of context about how you work.

Everything is plain markdown files in `~/wiki/` on your machine. Readable,
editable, deletable. Nothing hidden.

---

## For Claude: Setup and Teaching Instructions

**You are reading this because a user wants their Claude Code sessions to get
smarter over time.** Your job is to:

1. Set up the memory system (one-time)
2. Teach the user how it works (so they understand, not just receive)
3. Install instructions that make every future session use the memory

Walk through each step. Explain what you're doing and why. The user should
finish this feeling like they understand the system, not like magic happened
behind their back.

### Step 1: Create the Wiki

Create this structure in the user's home directory:

```
~/wiki/
  index.md
  log.md
  me/profile.md
  topics/       (empty directory)
  projects/     (empty directory)
  people/       (empty directory)
```

**~/wiki/index.md:**
```markdown
# Wiki Index

Everything I know about you and your work. I read this first every session.

## About You
- [me/profile.md](me/profile.md) — Who you are, how you work, what you prefer

## Projects

## Topics

## People
```

**~/wiki/log.md:**
```markdown
# Session Log

What we talked about, what I learned, what changed.

---
```

**~/wiki/me/profile.md:**
```markdown
# About You
Last updated: (today's date)

## Identity

## Work Style

## Preferences

## Opinions

(I'll fill these in as I learn about you.)
```

Tell the user what you just did and why:
> "I created ~/wiki/ — a folder of markdown files where I'll keep everything I
> learn about you. Think of it as my notebook. Right now it's mostly empty, but
> every conversation fills it in. You can open these files anytime to see what
> I know, correct anything I got wrong, or add your own notes."

### Step 2: Install Memory Instructions

Add the following to `~/.claude/CLAUDE.md` (the user's global config that loads
into every Claude Code session). If the file exists, **append** — never
overwrite existing content.

```markdown
## Memory System

You have a persistent wiki at ~/wiki/. Use it to be a better assistant — not
just to store facts, but to genuinely improve the user's experience over time.

### On Session Start

1. Read ~/wiki/index.md
2. Read ~/wiki/me/profile.md
3. Based on what the user says first, read any relevant project/topic/people
   pages
4. If ~/wiki/ doesn't exist, create it (index.md, log.md, me/profile.md, and
   empty directories for topics/, projects/, people/)

### How to Use What You Know

The wiki is only valuable if you USE it. Here's how:

**Adapt your communication.** If the user prefers terse responses, be terse.
If they like detailed explanations, explain. If they hate emojis, don't use
them. Match THEIR style, not a default.

**Match their skill level.** A senior engineer doesn't need you to explain what
a callback is. A beginner does. Read me/profile.md and calibrate — don't
over-explain to experts or under-explain to learners.

**Remember their stack.** When suggesting solutions, use the tools and languages
they actually use. If they're a Go shop, don't suggest Python solutions. If they
use pnpm, don't write npm commands. Check their project pages.

**Know their projects.** When they mention a project by name, you should already
know what it is, what it's built with, and what they were working on last time.
Don't make them re-explain. Read the project page and pick up where they left
off.

**Apply their preferences.** If they told you they prefer functional style,
write functional code — every time, without being asked again. If they hate
ORMs, don't suggest one. Preferences in the wiki are standing instructions.

**Use people context.** When they mention a colleague by name, you should know
who that person is and their role. "Can you help me review Sarah's PR" should
not prompt "Who is Sarah?"

**Anticipate based on patterns.** If they always start Monday sessions by
checking CI, expect that. If they tend to context-switch between two projects,
be ready for both. The session log (log.md) reveals patterns — use them.

### When to Write

Update the wiki whenever you learn something that would make future sessions
better:

- **User info** → ~/wiki/me/profile.md
  Name, role, company, timezone, skill level, communication style,
  preferences, opinions, tools they use, things they hate
- **Projects** → ~/wiki/projects/[name].md
  What it is, tech stack, status, current focus, blockers, decisions made
- **Topics** → ~/wiki/topics/[name].md
  Things they care about, their opinions, gotchas they've discovered
- **People** → ~/wiki/people/[name].md
  Name, role, relationship, context — enough to not ask "who?"
- **Corrections** → Update the relevant page IMMEDIATELY
  A correction is more important than the original fact. When the user says
  "actually, I prefer X" or "no, we switched to Y" — update the page right
  away and remove the wrong information.

### How to Write

- Bullet points, not paragraphs
- "Last updated: YYYY-MM-DD" at the top of each page
- Link between related pages: [see project](../projects/foo.md)
- One page per subject — don't cram
- Be specific: "prefers Tailwind over Bootstrap" not "has CSS preferences"

### Session Logging

Before the conversation ends, append to ~/wiki/log.md:
```
## [YYYY-MM-DD] Brief topic summary
- What we discussed
- What I learned about user
- Pages created: (list)
- Pages updated: (list)
```

Update ~/wiki/index.md if any new pages were created.

### Never Save

- Passwords, API keys, tokens, secrets — absolutely never
- One-off questions that won't matter tomorrow
- Anything the user explicitly asks you to forget

### Maintenance

Periodically scan the wiki for:
- Contradictions between pages — resolve using the most recent info
- Stale information — update or remove
- Pages that overlap — merge them
- Missing index entries — add them
```

Tell the user:
> "I installed memory instructions in your global Claude config. From now on,
> every Claude Code session — in any project, any folder — starts by reading
> your wiki. But it's not just about remembering facts. It's about using what I
> know to actually help you better: matching your style, knowing your stack,
> remembering your projects, respecting your preferences. The more we work
> together, the more I can do that."

### Step 3: First Conversation

Ask the user:
> "Let's get started. Tell me about yourself — your name, what you do, what
> you're working on, and how you like your AI to communicate. Terse? Detailed?
> Casual? Technical? Whatever you prefer."

When they respond:
1. Update ~/wiki/me/profile.md with everything — identity, role, style, stack
2. Create project pages for anything they mention
3. Update ~/wiki/index.md with new pages
4. Show them the updated profile so they can see what you captured

Then:
> "That's your wiki with real data now. Next session, I'll read all of this
> before I say a word to you. I'll know your name, your projects, and how you
> like to work. Try closing this and starting fresh — you'll see the
> difference immediately."

### Step 4: Teach the User

Explain the system so they own it:

> "Here's what you have:
>
> **~/wiki/** is a folder of plain markdown files — your AI's long-term memory.
> I read it at the start of every session and write to it as I learn things.
>
> **~/.claude/CLAUDE.md** is your global config. It tells every Claude Code
> session to use the wiki and HOW to use it — not just storing facts, but
> applying them to give you better answers.
>
> You're in complete control. Open ~/wiki/ in any editor. Read what I know.
> Fix anything wrong. Add your own notes — I'll pick them up. Delete something
> you don't want me to remember — gone.
>
> The system compounds. Session 1 is generic. Session 10 is personalized.
> Session 50, I know your stack, your team, your opinions, your patterns. I
> stop being a generic AI and start being YOUR AI."

### Step 5: What's Next (only if they ask)

If the user wants to go further, offer these in order:

1. **Project CLAUDE.md** — Drop a CLAUDE.md in any repo with project-specific
   instructions (coding standards, architecture notes, deployment process).
   Claude reads both global and project configs.

2. **Built-in memory** — Claude Code has a native `/memory` system with typed
   memories per-project. Type `/init` in any repo to set it up. Works alongside
   the wiki.

3. **Context Mode** — Semantic codebase indexing. Claude can search your whole
   project intelligently. Install:
   `npx -y @anthropic-ai/context-mode@latest init`

4. **Nellie** — A semantic memory server with lessons, checkpoints, and
   knowledge graphs across all your projects.

Don't push. Just mention they exist.

---

## How It Works (For Humans)

### The Idea

Andrej Karpathy proposed that LLMs should maintain their own persistent
knowledge base — a wiki of plain text files that the AI reads and writes across
sessions. Instead of starting every conversation from zero, the AI consults its
notes first.

We extended that idea with structure (typed pages, indexes, cross-references),
a session lifecycle (read → use → learn → log), and most importantly: **rules
for how to apply the knowledge**, not just how to store it.

The result: an AI that doesn't just remember your name — it remembers that you
prefer functional style, you're building a Go microservice, your teammate Alice
owns the auth module, and you hate being asked questions you already answered.

### The Wiki

```
~/wiki/
  index.md              Table of contents — read first every session
  log.md                Session history
  me/profile.md         Who you are and how you work
  projects/myapp.md     A project — stack, status, goals
  topics/docker.md      A topic — notes, preferences, gotchas
  people/alice.md       A person — role, context
```

### The Lifecycle

```
 Start Session           During Conversation           End Session
      |                         |                          |
      v                         v                          v
 Read wiki ──>            Learn something ──>        Log what happened
 Know the user            Write to wiki              Update index
      |                         |
      v                         v
 Better answers            Better wiki
```

### Why It Works

- **Compounds** — each session adds to the knowledge base. Early sessions are
  generic; later sessions are deeply personalized.
- **Corrections stick** — tell Claude once that you prefer X. It updates the
  wiki. Every future session reads that preference.
- **User-controlled** — it's your files. Read them, edit them, delete them.
  Nothing hidden, nothing locked.
- **Zero maintenance** — Claude manages the wiki automatically. You only touch
  it if you want to.

## FAQ

**Will this slow down my sessions?**
No. Reading small markdown files takes milliseconds.

**Can I edit the wiki?**
Yes — it's your data. Edit, add, delete. Claude picks up changes next session.

**Does this work with the Claude desktop app?**
No. This is for Claude Code (CLI and IDE extensions) which has filesystem access.

**What about per-project memory?**
Claude Code has a built-in per-project system. Run `/init` in any repo. The wiki
and built-in memory complement each other.

**Can I sync the wiki across machines?**
Yes. It's just files. Use git, Dropbox, iCloud Drive, Syncthing — whatever you
use for files.

## Credits

Based on Andrej Karpathy's concept of LLMs maintaining persistent wiki-style
knowledge bases. Extended with structured page types, session lifecycle,
cross-referencing, and behavioral instructions for applying knowledge to improve
user experience.

Built by [Mike Morris](https://github.com/mmorris35).

## License

MIT
