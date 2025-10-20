# Multi-Agent Research System

A powerful multi-agent AI system that conducts web research, writes persuasive articles in Portuguese (PT-BR), and performs quality control through iterative editorial review.

## Features

- **Three Specialized AI Agents**:
  - **Researcher**: Conducts web searches and extracts reliable sources
  - **Writer**: Creates engaging, persuasive articles in PT-BR using marketing techniques
  - **Editor**: Performs quality control, fact-checking, and ensures PT-BR compliance

- **Iterative Improvement**: Up to 2 revision rounds based on Editor feedback
- **Complete Logging**: Every agent execution is logged with full input/output
- **Configurable Models**: Use different OpenAI models for each agent
- **Marketing-Focused**: Copywriting techniques for general public engagement

## Requirements

- Python 3.12+
- OpenAI API key
- uv package manager

## Quick Start

### 1. Installation

```bash
# Clone the repository
git clone https://github.com/DiegoGarzaro/multi-agent-researcher
cd multi-agent-researcher

# Install dependencies using uv
uv sync
```

### 2. Configuration

Copy the example environment file and configure it:

```bash
cp .env.example .env
```

Edit `.env` and add your OpenAI API key:

```env
# OpenAI API Configuration
OPENAI_API_KEY=your_openai_api_key_here

# OpenAI Models for Each Agent
RESEARCHER_MODEL=gpt-4o-mini
WRITER_MODEL=gpt-4o-mini
EDITOR_MODEL=gpt-4o-mini
```

### 3. Usage

```bash
uv run multi_agent_research.py "Your research topic here"
```

**Example:**

```bash
uv run multi_agent_research.py "Desafio Água cristalina - os tres passos para transformar agua de torneira em agua de peixe"
```

### 4. Output

After execution, you'll find:

- **`output_YYYYMMDD_HHMMSS.md`**: The final article with timestamp (keeps history)
- **`output_latest.md`**: Copy of the latest article for convenience
- **`agents_log/*.json`**: Detailed logs of all agent executions

**Example output files:**
```
output_20251019_143022.md  # First run
output_20251019_153145.md  # Second run
output_20251019_163230.md  # Third run
output_latest.md           # Always points to most recent
```

## Configuration

### OpenAI Models

You can configure different models for each agent in the `.env` file:

```env
# Fast and cost-effective (default)
RESEARCHER_MODEL=gpt-4o-mini
WRITER_MODEL=gpt-4o-mini
EDITOR_MODEL=gpt-4o-mini

# Or use more powerful models
RESEARCHER_MODEL=gpt-4o
WRITER_MODEL=gpt-4o-turbo
EDITOR_MODEL=gpt-4o
```

**Available Models:**
- `gpt-4o-mini`: Fastest and most cost-effective
- `gpt-4o`: Better quality, balanced performance
- `gpt-4-turbo`: Highest quality for complex tasks
- `gpt-4`: Previous generation flagship model
- `gpt-3.5-turbo`: Budget option

**Model Selection Tips:**
- Use `gpt-4o-mini` for all agents: Most cost-effective, good for most tasks
- Mix models: `gpt-4o-mini` for research, `gpt-4o` for writing, `gpt-4o-mini` for editing
- Use `gpt-4o` or `gpt-4-turbo` for all: Best quality, higher cost

### Revision Rounds

The default maximum revision rounds is 2. You can modify this in `multi_agent_research.py`:

```python
def run(topic: str) -> str:
    return revision_loop(topic, max_rounds=2)  # Change this value
```

## Documentation

Comprehensive UML documentation is available in the `docs/` directory:

- **[Sequence Diagram](docs/UML_SEQUENCE_DIAGRAM.md)**: Chronological flow of execution
- **[Component Diagram](docs/UML_COMPONENT_DIAGRAM.md)**: System architecture
- **[Activity Diagram](docs/UML_ACTIVITY_DIAGRAM.md)**: Workflow and decision logic
- **[Class Diagram](docs/UML_CLASS_DIAGRAM.md)**: Object-oriented structure
- **[Documentation Guide](docs/README.md)**: Complete documentation overview

## How It Works

### The Workflow

```
1. Research Phase (Round 0)
   - Researcher conducts web search
   - Extracts reliable sources
   - Creates research brief

2. Writing Phase
   - Writer receives research brief
   - Creates persuasive PT-BR article
   - Applies marketing techniques

3. Review Phase
   - Editor reviews article
   - Checks quality, accuracy, PT-BR compliance
   - Returns APPROVE or REVISE

4. Revision Loop (if REVISE and rounds < max_rounds)
   - Researcher finds additional sources
   - Writer rewrites with updated research
   - Editor reviews again
   - Repeat up to max_rounds

5. Output Phase
   - Save final article to output.md
   - Save all logs to agents_log/
```

### Agent Roles

**Researcher Agent**
- **Tools**: DuckDuckGo, Trafilatura
- **Output**: Research brief with numbered sources
- **Language**: PT-BR sources preferred, English sources when needed

**Writer Agent** 

- **Specialty**: Marketing-focused copywriting in PT-BR
- **Style**: Conversational, persuasive, emotionally engaging
- **Output**: Article with citations and sources section

**Editor Agent**
- **Responsibilities**:
  - Verify PT-BR language compliance
  - Check accuracy and source credibility
  - Evaluate marketing effectiveness
  - Ensure audience appeal
- **Output**: APPROVE or REVISE with specific feedback

## Logging System

Every agent execution is automatically logged to `agents_log/` directory.

### Log File Format

```
YYYYMMDD_HHMMSS_AgentName_roundN_step.json
```

### Example Logs

```
agents_log/20251019_143022_Researcher_round0_research.json
agents_log/20251019_143145_Writer_round0_write.json
agents_log/20251019_143230_Editor_round0_edit.json
agents_log/20251019_143310_Researcher_round1_revision_research.json
agents_log/20251019_143425_Writer_round1_revision_write.json
agents_log/20251019_143512_Editor_round1_edit.json
```

### Log Contents

Each log file contains:
- Timestamp (ISO format)
- Agent name
- Round number (0 = initial, 1-2 = revisions)
- Step identifier
- Complete input prompt
- Complete output/response

## Project Structure

```
research_multi_agent/
- multi_agent_research.py    # Main system
- ai_prompts.py              # Agent instructions
- .env                       # Configuration (not in git)
- .env.example              # Configuration template
- .gitignore                # Git ignore rules
- pyproject.toml            # Project dependencies
- README.md                 # This file
- output*.md                 # Generated article with timestamp (gitignored)
- output_latest.md         # Latest article generated (gitignored)
- agents_log/              # Execution logs (gitignored)
   - *.json
   - docs/                    # UML documentation
      - README.md
      - UML_SEQUENCE_DIAGRAM.md
      - UML_COMPONENT_DIAGRAM.md
      - UML_ACTIVITY_DIAGRAM.md
      - UML_CLASS_DIAGRAM.md
```

## Development

### Code Style

The project follows these standards:
- **Linting**: Ruff
- **Docstrings**: Google-style with Args, Returns, and types
- **Principles**: SOLID principles, clean code
- **Type Hints**: Full type annotations

### Running Linter

```bash
ruff check .

# Auto-fix issues
ruff check . --fix
```

### Adding New Features

1. Review the [Class Diagram](docs/UML_CLASS_DIAGRAM.md) for design patterns
2. Update relevant UML diagrams if changing workflow
3. Add Google-style docstrings to all functions
4. Ensure logging is added for all agent executions
5. Run ruff before committing

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Follow the code style guidelines
4. Update documentation as needed
5. Submit a pull request

## License

This project is provided as-is for educational and research purposes.

## Acknowledgments

Built with:
- [Agno](https://github.com/agno-agi/agno) - Multi-agent framework
- [OpenAI](https://openai.com) - GPT models
- [DuckDuckGo](https://duckduckgo.com) - Web search
- [Trafilatura](https://github.com/adbar/trafilatura) - Web content extraction

## Support

For issues or questions:
- Check the [documentation](docs/README.md)
- Review execution logs in `agents_log/`
- Ensure `.env` has valid `OPENAI_API_KEY`
- Verify model names are correct in `.env`

---

**Version**: 0.1.0 \
**Last Updated**: 2025-10-19
