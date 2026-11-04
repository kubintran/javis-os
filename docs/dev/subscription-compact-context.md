# Compact subscription context

This opt-in changes the dashboard oauth subscription base prompt, not native Codex instructions, tools or thread history. Persona prompts and other provider kinds retain their existing path.

Merge these fields through config.update_settings; never replace the whole context_runtime section:

```json
{"context_runtime":{"subscription_prompt_mode":"compact-v1","memory_relevance_mode":"strict-v1"}}
```

The switches are independent. Remove either field or use another value to restore its prior behavior. Protected profile, project, session, Core, Coding, language and learning-state blocks use existing readers with their existing source limits. Native skill mirroring still runs. Compiler fallback restores memory and skill sources while retaining compact context; source-reader failures fall back to the existing prompt and emit an error event.

Strict memory retrieval removes common request vocabulary and terms occurring in more than 15% of indexed records (for corpora of at least ten records). Non-core records must meet the configured confidence threshold and match at least two terms when three or more terms remain. Core identity survives; explicit recall preserves the existing fallback. This is lexical filtering, not a semantic relevance guarantee; evaluate named entities, short queries, multilingual recall and active-state continuation during rollout.

Do not clear existing threads automatically. Existing Codex threads can retain their initial developer instructions, so evaluate a new user-created conversation. Roll back switches if routing, recall or protected instructions regress.

Run python tests/run.py subscription_compact and python tests/run.py adaptive_context_phase8. Count text components separately from provider wire usage. Base-prompt savings do not establish total token, plan quota or latency savings. Evaluate chat, recall, Wiki lookup, notes, calendar and external actions before enabling broadly.

## Request context measurement

Codex app-server now exposes the latest `tokenUsage.last.inputTokens` separately from the turn sum.
A new SQLite column `codex_last_request_input_tokens` controls Codex rotation. Old summed values are
not migrated into this column. Exec fallback without request-level measurements records unknown as
zero, disabling token-triggered rotation until a measurement arrives. Native compaction remains available.
The runtime trace labels notification counts `usage_event_count`, never request counts. Existing turn
billing counters are unchanged and still sum usage notifications; duplicate notification accounting is
not addressed here. The last input is provider-reported input, not an exact count of every retained
transcript item. Claude and other engines retain their previous rotation behavior in this patch.
