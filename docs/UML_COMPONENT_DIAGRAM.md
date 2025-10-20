# Multi-Agent Research System - Component Diagram

This diagram shows the system architecture and component relationships.

```plantuml
@startuml
!theme plain
skinparam componentStyle rectangle

package "Multi-Agent Research System" {

    package "Entry Point" {
        [CLI Interface\n__main__] as CLI
        [run() function] as RUN
    }

    package "Core Orchestration" {
        [revision_loop()\nWorkflow Controller] as RL
        [research_step_fn()] as RSF
        [write_step_fn()] as WSF
        [edit_step_fn()] as ESF
    }

    package "Agent Builders" {
        [build_researcher()] as BR
        [build_writer()] as BW
        [build_editor()] as BE
    }

    package "Agent Instances" #E8F5E9 {
        component "Researcher Agent" as RA {
            [DuckDuckGo Search]
            [Trafilatura Extractor]
        }
        component "Writer Agent" as WA {
            [Marketing Copywriter]
            [PT-BR Translator]
        }
        component "Editor Agent" as EA {
            [Quality Control]
            [Fact Checker]
        }
    }

    package "Configuration" #FFF3E0 {
        file ".env\nOPENAI_API_KEY" as ENV
        file "ai_prompts.py\nInstructions" as PROMPTS
    }

    package "Logging System" #FCE4EC {
        [log_agent_execution()] as LOG
        folder "agents_log/\nJSON logs" as LOGDIR
    }

    package "Output" #F3E5F5 {
        file "output.md\nFinal Article" as OUTPUT
    }
}

cloud "External Services" {
    [OpenAI API\ngpt-4o-mini] as OPENAI
    [Web Search\nDuckDuckGo] as WEB
}

CLI --> RUN
RUN --> RL

RL --> RSF
RL --> WSF
RL --> ESF

RSF --> BR
WSF --> BW
ESF --> BE

BR --> RA
BW --> WA
BE --> EA

RA --> OPENAI
RA --> WEB
WA --> OPENAI
EA --> OPENAI

PROMPTS ..> RA : provides instructions
PROMPTS ..> WA : provides instructions
PROMPTS ..> EA : provides instructions

ENV ..> RA : API key
ENV ..> WA : API key
ENV ..> EA : API key

RSF --> LOG : logs execution
WSF --> LOG : logs execution
ESF --> LOG : logs execution

LOG --> LOGDIR : saves to

RL --> OUTPUT : writes final article

@enduml
```

## Component Descriptions

### Entry Point
- **CLI Interface**: Command-line entry point that accepts topic argument
- **run()**: Wrapper function that calls revision_loop with max_rounds=2

### Core Orchestration
- **revision_loop()**: Main workflow controller that orchestrates the multi-agent process
- **research_step_fn()**: Executes Researcher agent and logs results
- **write_step_fn()**: Executes Writer agent and logs results
- **edit_step_fn()**: Executes Editor agent and logs results

### Agent Builders
- **build_researcher()**: Creates Researcher agent with web tools
- **build_writer()**: Creates Writer agent with marketing instructions
- **build_editor()**: Creates Editor agent with quality control instructions

### Agent Instances
- **Researcher Agent**: Uses DuckDuckGo and Trafilatura for web research
- **Writer Agent**: Marketing-focused copywriter for PT-BR content
- **Editor Agent**: Quality control and fact-checking specialist

### Configuration
- **.env File**: Contains OpenAI API key
- **ai_prompts.py**: Contains instruction prompts for all agents

### External Services
- **OpenAI API**: GPT-4o-mini model for all agents
- **Web Search**: DuckDuckGo for finding sources

### Logging System
- **log_agent_execution()**: Logging utility function
- **agents_log/**: Directory containing JSON logs for each execution

### Output
- **output.md**: Final article in markdown format with sources

## Key Relationships

1. **Solid arrows (→)**: Direct function calls or data flow
2. **Dotted arrows (..>)**: Configuration or dependency injection
3. **Colored packages**: Logical grouping by function
   - Green: Agent instances
   - Orange: Configuration
   - Pink: Logging
   - Purple: Output
