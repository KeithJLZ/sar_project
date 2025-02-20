# Search and Rescue (SAR) Agent Framework - CSC 581

## Introduction

This is for my implementation of Medical Team Leader, one of the SAR agents for CSC 581. More can be found below:

https://docs.google.com/spreadsheets/d/1QZK5HAdDC-_XNui6S0JZTbJH5_PbYJTp8_gyhXmz8Ek/edit?usp=sharing
https://docs.google.com/spreadsheets/d/11rBV9CbKNeQbWbaks8TF6GO7WcSUDS_-hAoH75UEkgQ/edit?usp=sharing

To run, just install the depencies and set up your environment variables. Then, run the cells for `medical_agent_lead.ipynb` in `src/sar_project/agents`. 
The query for the agent will be through the `answer_question` function. 

The agent is my attempt at implementing RAG. There are three PDF files that it draws from. One for help in triaging, one as a medical guide, and one with some sample data 
of people who are hurt. They are stored in vector databases. The text splitter is `RecursiveCharacterTextSplitter.from_tiktoken_encoder(chunk_size=300, chunk_overlap=50)` from `langchain`.

The first task of the agent is to ask it to create 3 subquestions from the user query (the number of subquestions could be dynamically decided in a future update). 
Then for each subquestion, it is asked which knowledge base(s) would be best to use to answer the subquestion. The agent will select documents from the knowledge base(s) 
to use to help answer them. This is to have better context for the main query to use (future update could be to rank the documents like with RAG fusion and to 
limit the number of documents returned). Each following question/subquestion also has access to the previous ones as context. A chat history is implemented for previous queries to use as context. 

Testing for the medical agent mainly just included a few queries asking about triaging the hurt people in the sample data and checking that it pulls correct information from the knowledge bases. See the 
end of `medical_agent_lead.ipynb` for the tests and outputs. 

## Prerequisites

- Python 3.8 or higher
- pyenv (recommended for Python version management)
- pip (for dependency management)

## Setup and Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd sar-project
```

2. Set up Python environment:
```bash
# Using pyenv (recommended)
pyenv install 3.9.6  # or your preferred version
pyenv local 3.9.6

# Create and activate virtual environment
python -m venv .venv
source .venv/bin/activate  # On Unix/macOS
# or
.venv\Scripts\activate     # On Windows
```

3. Install dependencies:
```bash
pip install -r requirements.txt
pip install -e .
```

4. Configure environment variables:

#### OpenAI:
- Obtain required API keys:
  1. OpenAI API key: Sign up at https://platform.openai.com/signup
- Update your `.env` file with the following:
    ```
    OPENAI_API_KEY=your_openai_api_key_here
    ```

Make sure to keep your `.env` file private and never commit it to version control.

## Project Structure

```
sar-project/
├── src/
│   └── sar_project/         # Main package directory
│       └── agents/          # Agent implementations
│       └── config/          # Configuration and settings
│       └── knowledge/       # Knowledge base implementations
├── tests/                   # Test directory
├── pyproject.toml           # Project metadata and build configuration
├── requirements.txt         # Project dependencies
└── .env                     # Environment configuration
```

## Development

This project follows modern Python development practices:

1. Source code is organized in the `src/sar_project` layout
2. Use `pip install -e .` for development installation
3. Run tests with `pytest tests/`
4. Follow the existing code style and structure
5. Make sure to update requirements.txt when adding dependencies


## FAQ

### Assignment Questions

**Q: How do I choose a role for my agent?**

**A:** Review the list of SAR roles above and consider which aspects interest you most. Your agent should provide clear value to SAR operations through automation, decision support, or information processing.

**Q: What capabilities should my agent have?**

**A:** Your agent should handle tasks relevant to its role such as: data processing, decision making, communication with other agents, and providing actionable information to human operators.

**Q: Can I add new dependencies?**

**A:** Yes, you can add new Python packages to requirements.txt as needed for your implementation.


### Technical Questions

**Q: Why am I getting API key errors?**

**A:** Ensure you've properly set up your .env file and obtained valid API keys from the services listed above.

**Q: How do I test my agent?**

**A:** Use the provided test framework in the tests/ directory. Write tests that verify your agent's core functionality.

**Q: Can I use external libraries for my agent?**

**A:** Yes, you can use external libraries as long as they are compatible.
