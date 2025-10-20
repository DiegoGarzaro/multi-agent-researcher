# Multi-Agent Research System - Documentation

Welcome to the comprehensive documentation for the Multi-Agent Research System. This system uses three specialized AI agents to conduct web research, write persuasive articles in Portuguese (PT-BR), and ensure quality through editorial review.

## 📚 Documentation Overview

This documentation includes four UML diagrams that provide different perspectives on the system:

### 1. [Sequence Diagram](./UML_SEQUENCE_DIAGRAM.md) 🔄
**Purpose**: Shows the chronological flow of execution

**Best for understanding**:
- How agents interact over time
- The order of operations
- Message flow between components
- The revision loop process

**Use when**: You want to understand "what happens when" during execution

---

### 2. [Component Diagram](./UML_COMPONENT_DIAGRAM.md) 🏗️
**Purpose**: Shows the system architecture and component relationships

**Best for understanding**:
- Overall system structure
- How different modules connect
- Dependencies between components
- External service integrations

**Use when**: You want to understand "how it's built" and component organization

---

### 3. [Activity Diagram (Flowchart)](./UML_ACTIVITY_DIAGRAM.md) 🔀
**Purpose**: Shows the detailed workflow and decision logic

**Best for understanding**:
- The revision loop flow
- Decision points (APPROVE vs REVISE)
- Round counting logic
- Complete workflow from start to finish

**Use when**: You want to understand "what decisions are made" and the process flow

---

### 4. [Class Diagram](./UML_CLASS_DIAGRAM.md) 📦
**Purpose**: Shows the object-oriented structure and relationships

**Best for understanding**:
- Class hierarchies and inheritance
- Data structures
- Design patterns used
- Relationships between classes

**Use when**: You want to understand "how it's designed" from an OOP perspective

---

## 🎯 Quick Start Guide

### Which Diagram Should I Look At?

**I want to understand the overall system flow:**
→ Start with [Activity Diagram](./UML_ACTIVITY_DIAGRAM.md)

**I want to see how agents communicate:**
→ Check [Sequence Diagram](./UML_SEQUENCE_DIAGRAM.md)

**I want to understand the architecture:**
→ Review [Component Diagram](./UML_COMPONENT_DIAGRAM.md)

**I'm a developer who wants to extend the system:**
→ Study [Class Diagram](./UML_CLASS_DIAGRAM.md)

---

## 🔍 System Overview

### The Three Agents

1. **Researcher Agent** 🔎
   - **Role**: Web search & source extraction
   - **Tools**: DuckDuckGo, Trafilatura
   - **Output**: Research brief with verified sources
   - **Language**: Searches in PT-BR when possible, uses English sources if needed

2. **Writer Agent** ✍️
   - **Role**: Marketing-focused copywriter
   - **Specialty**: Persuasive content in Portuguese (PT-BR)
   - **Output**: Engaging article with citations
   - **Style**: Conversational, benefits-focused, emotionally engaging

3. **Editor Agent** 📋
   - **Role**: Quality control & fact-checking
   - **Responsibilities**:
     - Verify PT-BR language compliance
     - Check accuracy and source credibility
     - Evaluate marketing effectiveness
     - Ensure audience appeal
   - **Output**: APPROVE or REVISE with specific feedback

### The Workflow

```
┌─────────────┐
│  Research   │  Round 0: Initial execution
│  (Agent 1)  │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Write     │  Create article from research
│  (Agent 2)  │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│    Edit     │  Review and decide
│  (Agent 3)  │
└──────┬──────┘
       │
       ▼
   APPROVE? ◄────┐ NO (and rounds < 2)
       │         │
      YES        │
       │         │
       ▼         │
   [Output with timestamp]     [Revision Loop]
```

### Revision Loop

- **Maximum rounds**: 2 (configurable)
- **Trigger**: Editor returns REVISE verdict
- **Process**:
  1. Researcher finds additional sources based on Editor feedback
  2. Writer rewrites article with updated research
  3. Editor reviews again
- **Outcome**: Final article (approved or after max rounds)

---

## 📝 Logging System

Every agent execution is logged to `agents_log/` directory:

### Log File Format
```
YYYYMMDD_HHMMSS_AgentName_roundN_step.json
```

### Example Log Files
```
20251019_143022_Researcher_round0_research.json
20251019_143145_Writer_round0_write.json
20251019_143230_Editor_round0_edit.json
20251019_143310_Researcher_round1_revision_research.json
20251019_143425_Writer_round1_revision_write.json
20251019_143512_Editor_round1_edit.json
```

### Log Content
Each log file contains:
- Timestamp (ISO format)
- Agent name
- Round number
- Step identifier
- Complete input prompt
- Complete output/response

---

## 🎨 How to View the Diagrams

### Option 1: PlantUML Online Server
1. Copy the PlantUML code from any diagram file
2. Go to http://www.plantuml.com/plantuml/uml/
3. Paste the code and view the rendered diagram

### Option 2: VS Code Extension
1. Install "PlantUML" extension in VS Code
2. Open any diagram `.md` file
3. Use `Alt+D` to preview the diagram

### Option 3: Local PlantUML
1. Install PlantUML: `brew install plantuml` (macOS)
2. Render diagram: `plantuml diagram_file.md`

### Option 4: IDE with PlantUML Support
- IntelliJ IDEA
- PyCharm
- Eclipse

---

## 🚀 Usage

```bash
# Run the system
uv run multi_agent_research.py "Your topic here"

# Output files created:
# - output_YYYYMMDD_HHMMSS.md (timestamped article, keeps history)
# - output_latest.md (copy of latest article for convenience)
# - agents_log/*.json (execution logs with timestamps)
```

---

## 📐 Design Patterns Used

1. **Factory Pattern**: Agent builders create specialized agents
2. **Strategy Pattern**: Different agents with different behaviors
3. **Observer Pattern**: Logging system observes all executions
4. **Template Method**: All step functions follow same pattern
5. **Facade Pattern**: CLI provides simple interface

---

## 🔗 File Structure

```
research_multi_agent/
├── multi_agent_research.py    # Main system
├── ai_prompts.py              # Agent instructions
├── .env                       # API keys and model configuration
├── .env.example              # Configuration template
├── output_*.md               # Generated articles with timestamps
├── output_latest.md          # Latest article
├── agents_log/               # Execution logs
│   └── *.json
└── docs/                     # This documentation
    ├── README.md
    ├── UML_SEQUENCE_DIAGRAM.md
    ├── UML_COMPONENT_DIAGRAM.md
    ├── UML_ACTIVITY_DIAGRAM.md
    └── UML_CLASS_DIAGRAM.md
```

---

## 🤝 Contributing

When extending the system, please:

1. Review the **Class Diagram** for design patterns
2. Update the **Sequence Diagram** if adding new interactions
3. Modify the **Activity Diagram** if changing workflow logic
4. Update the **Component Diagram** if adding new components
5. Add Google-style docstrings to all new functions
6. Ensure logging is added for all agent executions

---

## 📞 Support

For questions or issues:
- Review the appropriate diagram for your use case
- Check the execution logs in `agents_log/`
- Ensure `.env` file has valid `OPENAI_API_KEY`

---

## 📄 License

This documentation and the Multi-Agent Research System are provided as-is for educational and research purposes.

---

**Last Updated**: 2025-10-19
**Version**: 0.1.0