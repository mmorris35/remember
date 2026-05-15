# Remember

**Make your Claude Code session remember you.**

Tell your Claude Code session:

```
Fetch https://raw.githubusercontent.com/mmorris35/remember/main/README.md and follow the setup instructions
```

That's it. Claude will read this file, walk you through setting up a persistent
memory system, and start learning about you immediately.

No plugins. No servers. No installs. Just markdown files.

---

## What You're Getting

Claude Code starts every session with amnesia. It doesn't remember your name,
your projects, or yesterday's conversation.

After this setup, your AI will:
- **Remember you** across sessions — your name, role, projects, preferences
- **Build a knowledge wiki** that grows every time you talk
- **Learn from corrections** — tell it once, it sticks forever
- **Never ask the same thing twice**

It's all plain markdown files in a `~/wiki/` folder on your machine. You can
read them, edit them, or delete them anytime.

---

## For Claude: How to Set This Up

**You are reading this because a user asked you to set up their memory system.**

Walk them through each step below. Explain what you're doing — the user is
learning. Don't silently configure. Teach as you go.

### Step 1: Create the Wiki

Create this folder structure in the user's home directory (`~`):

```
~/wiki/
  index.md
  log.md
  me/profile.md
  topics/       (empty directory)
  projects/     (empty directory)
  people/       (empty directory)
```

Seed the starter files:

**~/wiki/index.md**
```markdown
# Wiki Index

Master catalog of everything I know. I read this first every session.

## About You
- [me/profile.md](me/profile.md) — Who you are, what you do, how you work

## Projects
(nothing yet — I'll add these as we work together)

## Topics
(nothing yet — I'll add these as they come up)

## People
(nothing yet — I'll remember people when you mention them)
```

**~/wiki/log.md**
```markdown
# Session Log

Chronological record of our conversations.

---
```

**~/wiki/me/profile.md**
```markdown
# About You
Last updated: (today's date)

(I don't know anything about you yet. Tell me about yourself and I'll
fill this in.)
```

Tell the user:
> "I created your wiki at ~/wiki/. This is where I'll store everything I learn
> about you. It's all plain markdown — you can open it in any text editor and
> see, edit, or delete anything."

### Step 2: Add Memory Instructions to Global Config

The user's global config is `~/.claude/CLAUDE.md`. This file loads into every
Claude Code session across all their projects.

If the file doesn't exist, create it with the content below.
If it already exists, **append** the content below — never overwrite.

**Add this to ~/.claude/CLAUDE.md:**

```markdown
## Memory System

You maintain a persistent wiki at ~/wiki/ that compounds your knowledge over
time. This is your long-term memory — it survives across sessions and projects.

### Every Session Start

1. Read ~/wiki/index.md — this is your table of contents
2. Based on the user's first message, read the most relevant wiki pages
3. Use this knowledge naturally — greet by name, reference projects, respect
   preferences
4. If ~/wiki/ doesn't exist, create the structure (index.md, log.md, me/,
   topics/, projects/, people/)

### During Conversation

When you learn something new, write it to the wiki immediately:

- About the user → update ~/wiki/me/profile.md
- A project they mention → create or update ~/wiki/projects/[name].md
- A topic worth remembering → create or update ~/wiki/topics/[name].md
- A person they mention → create or update ~/wiki/people/[name].md
- A correction ("actually I prefer X") → fix the relevant page immediately

Page guidelines:
- Bullet points over paragraphs — keep them scannable
- Add "Last updated: YYYY-MM-DD" at the top of each page
- Link between related pages: [related](../topics/foo.md)
- One page per subject

### Session End

Before conversation ends:
1. Append a log entry to ~/wiki/log.md:
   ```
   ## [YYYY-MM-DD] Brief topic summary
   - Key things discussed
   - What I learned about user
   - Pages created: (list)
   - Pages updated: (list)
   ```
2. Update ~/wiki/index.md if new pages were created

### What to Remember

- Identity: name, role, company, location, timezone
- Work style: preferred tools, coding conventions, communication preferences
- Opinions: likes, dislikes, pet peeves, enthusiasms
- Projects: what they build, tech stack, status, goals, blockers
- People: colleagues, family, collaborators — names and context
- Corrections: when they fix a mistake, the correction matters most
- Recurring themes: topics that keep coming up

### Never Save

- Passwords, API keys, tokens, secrets — never
- Throwaway questions that won't matter tomorrow
- Anything the user asks you not to remember

### Maintenance

When idle, check the wiki for:
- Contradictions between pages
- Outdated info to update or remove
- Overlapping pages to merge
- Orphan pages missing from index.md
```

Tell the user:
> "I added memory instructions to ~/.claude/CLAUDE.md. Every Claude Code
> session you open — in any project — will now read your wiki and keep it
> updated."

### Step 3: First Run

Ask the user:
> "Let's test the system. Tell me about yourself — your name, what you do, and
> what you're working on right now."

When they respond:
1. Update ~/wiki/me/profile.md with what they shared
2. Create project pages for anything they mentioned
3. Update ~/wiki/index.md with new pages
4. Read the updated profile back to them so they can see it

Then say:
> "Your wiki is live. Try closing this session and starting a new one — I'll
> already know who you are."

### Step 4: Explain What Happened

> "Here's what you have now:
>
> Every time you open Claude Code, I read ~/wiki/index.md first. That tells me
> what pages exist. Based on what you ask, I pull in the relevant ones.
>
> As we talk, I update pages with new things I learn. When we're done, I log
> what happened.
>
> It's all plain text. Open ~/wiki/ in any editor — that's everything I know
> about you. Edit anything, I'll pick up your changes next session.
>
> Each session builds on the last. The more we work together, the better I
> get."

### Step 5: Going Further (only if they ask)

If the user asks "what else can I do?", mention these in order:

1. **Project CLAUDE.md** — Put a CLAUDE.md in any git repo for project-specific
   instructions. Claude reads both global and project configs.

2. **Built-in memory** — Claude Code has a native memory system with typed
   memories per-project. Type `/init` in any project to set it up.

3. **Context Mode** — Indexes your codebase for semantic search. Great for
   large projects: `npx -y @anthropic-ai/context-mode@latest init`

4. **Nellie** — Persistent semantic memory server with knowledge graphs across
   all projects and sessions.

Don't push these. Just plant the seed.

---

## How It Works (For Humans)

### The Core Idea

Your AI maintains a personal wiki — a folder of markdown files about you, your
projects, and things you care about. Before answering anything, it reads the
wiki. After learning something, it writes to the wiki. Over time, it
accumulates real understanding of your context.

### What's in the Wiki

```
~/wiki/
  index.md              Table of contents — read first every session
  log.md                Session history — what you talked about and when
  me/profile.md         Your name, role, preferences, how you work
  projects/myapp.md     A project — tech stack, status, goals
  topics/docker.md      A topic — your notes, preferences, gotchas
  people/alice.md       A person — role, relationship, context
```

### The Session Lifecycle

```
 Start Session          During Conversation          End Session
      |                        |                         |
      v                        v                         v
 Read index.md ──>       Learn something ──>       Append to log.md
 Read relevant pages     Write to wiki page         Update index.md
      |
      v
 Personalize responses
```

### Why Plain Markdown

- **Portable** — just files. Back them up, sync them, version them
- **Readable** — open in any editor. No database, no binary blobs
- **Editable** — you're in control. Fix anything Claude got wrong
- **Durable** — no server to crash, no account to expire

## FAQ

**Where does the wiki live?**
`~/wiki/` — your home directory. Works across all projects.

**Can I edit the wiki?**
Yes. It's your data. Edit anything, Claude picks up changes next session.

**Does this work with the Claude desktop app?**
No — this is for Claude Code (the CLI and IDE extensions). The desktop app
doesn't have filesystem access.

**Will this slow things down?**
No. Reading a few small markdown files takes milliseconds.

**What about per-project memory?**
Claude Code has a built-in per-project system. Run `/init` in any repo. The
wiki and built-in memory work side by side.

## Credits

Based on Andrej Karpathy's concept of LLMs maintaining persistent wiki-style
knowledge bases. Extended with structured page types, session lifecycle,
cross-referencing, and an interactive teaching flow.

Built by [Mike Morris](https://github.com/mmorris35).

## License

MIT
