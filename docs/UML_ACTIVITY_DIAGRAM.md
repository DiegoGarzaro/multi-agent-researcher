# Multi-Agent Research System - Activity Diagram (Flowchart)

This diagram shows the detailed flow of the revision loop process.

```plantuml
@startuml
!theme plain
skinparam ActivityBackgroundColor #E3F2FD
skinparam ActivityBorderColor #1976D2
skinparam ActivityDiamondBackgroundColor #FFF9C4
skinparam ActivityDiamondBorderColor #F57C00

start

:User provides topic;

:Initialize revision_loop()
Set round = 0;

partition "Round 0: Initial Execution" #E8F5E9 {
    :Researcher Agent conducts
    web search for topic;

    :Extract sources and create
    research brief;

    :Log Researcher execution
    (round=0, step=research);

    :Writer Agent receives
    research brief;

    :Create persuasive article
    in PT-BR with marketing
    techniques;

    :Log Writer execution
    (round=0, step=write);

    :Editor Agent reviews
    draft article;

    :Check quality, accuracy,
    PT-BR compliance, and
    marketing effectiveness;

    :Log Editor execution
    (round=0, step=edit);
}

repeat :Check Editor verdict;

    if (Editor verdict?) then (APPROVE)
        :Article approved;
        break
    else (REVISE)
        if (rounds >= max_rounds?) then (YES)
            :Max rounds reached;
            :Use current draft as final;
            break
        else (NO)
            :Increment round counter;

            partition "Revision Loop" #FFF3E0 {
                :Researcher receives
                Editor's feedback;

                :Find additional sources
                to address feedback;

                :Update research brief
                with new information;

                :Log Researcher execution
                (round=N, step=revision_research);

                :Writer receives updated
                research brief;

                :Rewrite article incorporating
                new research and maintaining
                source consistency;

                :Log Writer execution
                (round=N, step=revision_write);

                :Editor re-reviews
                revised draft;

                :Log Editor execution
                (round=N, step=edit);
            }
        endif
    endif

repeat while (Continue revision?) is (YES) not (NO)

:Save final article to output.md;

:Save all execution logs
to agents_log/ directory;

:Display success message
with file path to user;

stop

@enduml
```

## Flow Description

### Initial Execution (Round 0)
1. **Research Phase**: Researcher agent searches the web and extracts sources
2. **Writing Phase**: Writer agent creates a persuasive PT-BR article
3. **Review Phase**: Editor agent evaluates quality and compliance

### Decision Point
- **APPROVE**: Article meets all criteria, proceed to save
- **REVISE**: Article needs improvements, enter revision loop (if rounds < max_rounds)

### Revision Loop (Rounds 1-2)
1. **Updated Research**: Researcher addresses Editor's specific feedback
2. **Rewrite**: Writer incorporates new research and revises article
3. **Re-review**: Editor evaluates the revised draft

### Loop Control
- Maximum 2 revision rounds (configurable)
- Each round is tracked and logged separately
- If max rounds reached, use the current draft as final (even if not approved)

### Output Phase
- Save final article to `output.md`
- All agent executions logged to `agents_log/` directory
- Success message displayed to user

## Key Features

- **Iterative Improvement**: Up to 2 revision cycles for quality enhancement
- **Complete Logging**: Every agent interaction is logged with round number and step identifier
- **Graceful Degradation**: System completes even if Editor never approves (after max rounds)
- **Traceability**: Full audit trail of research, writing, and editorial decisions
