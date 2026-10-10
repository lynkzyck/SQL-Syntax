# CLAUDE.md

## task-observer activation

Before the first tool call of any session — and before writing or
proposing a plan, not merely before executing one — invoke the
task-observer skill AND execute its Session Start Protocol (storage
check, frontmatter scan, review trigger). Loading the skill and running
the protocol are separate steps; a session that loads the file and stops
has activated nothing. Any turn that will involve a tool call counts; do
not classify the session as "too simple" from its opening message.

Select skills on the DECISION the request is about, not on the artefact it
arrived as. Name what the user is deciding, then match the installed skill
descriptions against that — a request handed over as a file to review
still needs the skill whose description names its subject.

After completing each task, list the observation records written this
session in the same turn and report a one-line summary from that listing
(ids and titles, or "none logged and why") — never from memory of
writing them. This is the activation backstop: it forces a look at the log,
so a session that silently skipped the protocol is discovered at the
first task boundary instead of never.

Loading a skill is not complete until you have queried the observation
log for OPEN observations naming it and read their bodies. Where scripts
run, the lookup and its checkpoint line are one command:
  bash "/home/user/SQL-Syntax/.agents/skills/task-observer/scripts/skill-load.sh" "/home/user/SQL-Syntax/.task-observer" <skill-name>
(<skill-name> is the bare name: drop a plugin: prefix, up to the last
colon.) The Skill invocation and that call are ONE batch — emit them
together, one call per skill loaded, never the call in a later batch.
Apply their insights to the current work — meaning: let them change what
you do in THIS task. Editing the skill file, or writing the rule into any
other file a later session reads, is acting on the observation and waits
for the review. See "Log, don't act" in SKILL.md.

The task-observer workspace for this project is:
  /home/user/SQL-Syntax/.task-observer
Every path the skill uses derives from that root and nothing else:
  /home/user/SQL-Syntax/.task-observer/skill-observations/observation-log/   (the log)
  /home/user/SQL-Syntax/.task-observer/skill-observations/cross-cutting-principles.md
  /home/user/SQL-Syntax/.task-observer/skill-updates/                        (staging root)
  /home/user/SQL-Syntax/.task-observer/skill-updates/PENDING.md              (staging manifest)
Never resolve any of them from the current working directory. Never place
the workspace inside a skills-discovery directory or any path linked into
one. If the repository is checked out somewhere other than
/home/user/SQL-Syntax, the workspace is `.task-observer/` at that
checkout's root.

**Persistence:** sessions run in ephemeral cloud containers, so the
workspace survives only through git. Before ending any session that wrote
to `.task-observer/`, commit those changes and push them to the session's
working branch.

A subagent dispatched by a session that already runs this protocol does
not run it again and does not write to the log. The controller observes;
a subagent that notices something worth logging says so in its final
report, and the controller writes it.

An observation file comes into being only through
  bash "/home/user/SQL-Syntax/.agents/skills/task-observer/scripts/new-observation.sh" <slug> "/home/user/SQL-Syntax/.task-observer"
which prints the path to write into. Never by copying another file's
header or counting a listing — including on the turn after a
compaction, when the skill body is out of context.
