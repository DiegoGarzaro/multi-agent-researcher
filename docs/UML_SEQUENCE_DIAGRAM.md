# Multi-Agent Research System - Sequence Diagram

This diagram shows the flow of execution through the multi-agent system, including the revision loop.

```plantuml
@startuml
!theme plain
skinparam sequenceMessageAlign center
skinparam BoxPadding 10

actor User
participant "CLI\n(main)" as CLI
participant "revision_loop()" as RL
participant "Researcher\nAgent" as R
participant "Writer\nAgent" as W
participant "Editor\nAgent" as E
participant "Agent\nLogger" as Log
database "File\nSystem" as FS

User -> CLI: Run with topic
activate CLI
CLI -> RL: run(topic)
activate RL

note over RL: Round 0 - Initial Execution

RL -> R: research_step_fn(topic)
activate R
R -> R: Web search &\nextract sources
R -> Log: log_agent_execution("Researcher", round=0)
activate Log
Log -> FS: Save agents_log/...\nResearcher_round0_research.json
deactivate Log
R --> RL: Research Brief
deactivate R

RL -> W: write_step_fn(brief, topic)
activate W
W -> W: Create article\nfrom research
W -> Log: log_agent_execution("Writer", round=0)
activate Log
Log -> FS: Save agents_log/...\nWriter_round0_write.json
deactivate Log
W --> RL: Draft Article
deactivate W

RL -> E: edit_step_fn(draft)
activate E
E -> E: Review article\nquality
E -> Log: log_agent_execution("Editor", round=0)
activate Log
Log -> FS: Save agents_log/...\nEditor_round0_edit.json
deactivate Log
E --> RL: Verdict\n(APPROVE or REVISE)
deactivate E

alt Verdict is REVISE (and rounds < max_rounds)
    note over RL: Round 1 - Revision Loop

    RL -> R: research_step_fn(topic + editor feedback)
    activate R
    R -> R: Find additional\nsources
    R -> Log: log_agent_execution("Researcher", round=1)
    activate Log
    Log -> FS: Save agents_log/...\nResearcher_round1_revision_research.json
    deactivate Log
    R --> RL: Updated Research Brief
    deactivate R

    RL -> W: write_step_fn(updated_brief, topic)
    activate W
    W -> W: Rewrite article
    W -> Log: log_agent_execution("Writer", round=1)
    activate Log
    Log -> FS: Save agents_log/...\nWriter_round1_revision_write.json
    deactivate Log
    W --> RL: Revised Draft
    deactivate W

    RL -> E: edit_step_fn(revised_draft)
    activate E
    E -> E: Re-review article
    E -> Log: log_agent_execution("Editor", round=1)
    activate Log
    Log -> FS: Save agents_log/...\nEditor_round1_edit.json
    deactivate Log
    E --> RL: Verdict\n(APPROVE or REVISE)
    deactivate E

    opt Verdict is still REVISE (and rounds < max_rounds)
        note over RL: Round 2 - Final Revision\n(Same process as Round 1)
    end
end

RL --> CLI: Final Article
deactivate RL
CLI -> FS: Write output.md
CLI --> User: Success message\nwith file path
deactivate CLI

@enduml
```

## Key Points

- **Round 0**: Initial research, write, and edit cycle
- **Round 1-2**: Revision cycles triggered by Editor's REVISE verdict
- **Logging**: Every agent execution is logged to `agents_log/` directory
- **Max Rounds**: Default is 2 revision rounds maximum
- **Output**: Final article saved to `output.md`

## Agent Roles

1. **Researcher**: Web search & source extraction
2. **Writer**: Marketing-focused copywriting in PT-BR
3. **Editor**: Quality control, fact-checking, and PT-BR validation
