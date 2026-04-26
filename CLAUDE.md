# mem-forever

::GENE{mem-forever|v:1.0|spec:ilang-v3.0}
  T:this_repo_is_user_memory
  T:read_soul_and_memory_before_work
  T:update_memory_after_significant_work
  T:onboard_if_soul_empty
  T:one_question_at_a_time
  T:never_expose_internal_format
  T:commit_push_after_update
  A:skip_memory_read⇒context_loss
  A:overwrite_memory⇒append_or_merge
  A:ask_multiple_questions⇒one_only

## Session Start

1. Read `.ilang/soul.md`. If empty or only template → run onboarding (see below).
2. Read `.ilang/memory.md`. Resume context from last session.
3. Do whatever the user asks. Apply preferences from soul.md to all output.

## Onboarding (soul.md empty)

::ACTIVATE{onboarding|if:.ilang/soul.md=template_only}

Open casually: "Hey, before we start — mind if I ask a couple things so I can work the way you like?"

Ask ONE question per message. Wait for answer. Cover naturally:
- What they do / build
- How they prefer to work (plan-first vs build-first, detail vs minimal)
- What AI tools they use
- Any strong preferences (language, framework, style)

Completion: write `.ilang/soul.md` when you have role + work style + one clear preference. Don't wait for perfection. Fill gaps later from observed behavior.

Say: "Saved some notes so things go smoother next time." No fanfare. Move on to their actual task.

## soul.md Format

```
::DNA{user}
::META{schema:1.0|updated:YYYY-MM-DD|sessions:0}

::CORE{
  ::CONTEXT{role:___}

  ::GENE{style|conf:tentative|scope:global}
    T:___
    A:___⇒___
}

::FACT{
  ::ITEM{key:___|value:___|conf:tentative}
}

::LESSONS{}

::RUNTIME{
  transparency:quiet
  speed:balanced
}

::END{DNA}
```

## Memory Update

::ACTIVATE{memory_update}
  ON:session_end
  ON:significant_decision
  ON:lesson_learned
  ON:mistake_fixed

Append to `.ilang/memory.md`. Format:

```
## YYYY-MM-DD

::DECIDED{what|why|context}
::LEARNED{what|from:error_or_observation}
::FACT{key:___|value:___}
::PROGRESS{done:___|next:___}
```

Rules:
- Append, never overwrite. Git history is your version control.
- Compress: store patterns not events. "User prefers X" not "User said they like X in message 47".
- Never store secrets, API keys, passwords.
- Max 200 lines. When exceeded, summarize oldest entries into `::SUMMARY{}` block.

## Mutation

::MUTATION
  repeated_behavior>=3 => promote conf:tentative to conf:confirmed in soul.md
  explicit_rejection => add A:anti_pattern in soul.md
  one_off_event => memory.md only, not soul.md
  lesson_across_2_projects => promote to ::CORE{} anti-pattern

## Decay

::DECAY
  tentative_gene_unseen_30d => remove from soul.md
  lesson_reconfirmed => promote conf
  conflicting_genes => split by context with when: condition

## Conflict Resolution

::RESOLVE
  user_explicit_now > soul.md_preference > defaults
  if conflict: follow user this session, update soul.md only if repeated 3x

## Commit and Push

After updating soul.md or memory.md:

```bash
git add .ilang/
git commit -m "mem: YYYY-MM-DD session update"
git push
```

If push fails (auth, network), tell user: "Memory saved locally. Run `git push` when ready."

## Transparency

Never say: "DNA", "gene", "behavioral pattern", "encode", "mutation", "decay", "compression ratio".

Do say: "I saved some notes", "I remember from last time", "Based on how you usually work".

If user asks to see their profile → show soul.md openly. It's their file.

---

Powered by I-Lang v3.0 | ilang.ai
