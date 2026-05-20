# EquityBot: RAG-LangChain News Research Tool 📈

A powerful Retrieval-Augmented Generation (RAG) chatbot built with LangChain and Streamlit that retrieves real-time financial information from equity-related websites and articles to generate context-aware, accurate financial responses.

## Overview

EquityBot combines the power of Large Language Models (LLMs) with retrieval-augmented generation to provide informed answers about financial news and equity research. Instead of relying solely on the model's training data, it dynamically retrieves relevant information from provided sources, ensuring responses are grounded in current, accurate data.

## Features

- 🔗 **Multi-URL Support**: Load and process up to 3 financial news articles or equity research URLs simultaneously
- 📚 **Intelligent Document Splitting**: Recursively splits documents into manageable chunks with smart separators
- 🧠 **Vector Embeddings**: Uses OpenAI's embedding model to create semantic vector representations
- 🔍 **FAISS Vector Store**: Efficient similarity search across document embeddings
- 💬 **Context-Aware Responses**: Generates answers with direct source attribution
- 📖 **Source Attribution**: Automatically tracks and displays sources for each response
- 💾 **Persistent Storage**: Caches FAISS indices locally for faster subsequent queries

## Technology Stack

- **Framework**: Streamlit (UI/UX)
- **LLM Integration**: LangChain with OpenAI
- **Vector Database**: FAISS (Facebook AI Similarity Search)
- **Embeddings**: OpenAI Embeddings
- **Language**: Python 3.x

## Prerequisites

Before running the application, ensure you have:

- Python 3.8 or higher
- An OpenAI API key (for both GPT models and embeddings)
- pip package manager

## Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/kevinyliu0603-dotcom/RAG-LangChain.git
   cd RAG-LangChain
   ```

2. **Create a virtual environment** (recommended)
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Set up environment variables**
   Create a `.env` file in the project root:
   ```
   OPENAI_API_KEY=your_openai_api_key_here
   ```

## Usage

1. **Start the Streamlit app**
   ```bash
   streamlit run main.py
   ```

2. **Access the application**
   The app will open in your default browser at `http://localhost:8501`

3. **Input URLs**
   - Enter up to 3 financial news or equity research URLs in the sidebar
   - Supported sources: News articles, research reports, equity analysis pages

4. **Process URLs**
   - Click the "Process URLs" button
   - Wait for the application to:
     - Load and parse the content
     - Split text into chunks
     - Generate embeddings
     - Build the FAISS index

5. **Ask Questions**
   - Once processing is complete, enter your financial research question
   - The chatbot retrieves relevant information and provides context-aware answers
   - Sources used for the response are displayed below the answer

## Project Structure

```
RAG-LangChain/
├── main.py                     # Main Streamlit application
├── faiss_store_openai.pkl      # Cached FAISS vector store
├── README.md                   # Project documentation
├── requirements.txt            # Python dependencies
└── .env                        # Environment variables (not committed)
```

## How It Works

### Document Processing Pipeline

1. **URL Loading**: `UnstructuredURLLoader` fetches and extracts text from provided URLs
2. **Text Splitting**: `RecursiveCharacterTextSplitter` divides content into overlapping chunks
   - Chunk size: 1000 characters
   - Separators: `\n\n`, `\n`, `.`, `,` (in order of priority)
3. **Embedding Generation**: OpenAI's embedding model converts text chunks to vectors
4. **Vector Storage**: FAISS creates an efficient index for similarity search
5. **Persistence**: Index is serialized and cached locally as `faiss_store_openai.pkl`

### Query Processing

1. **User Question**: Input natural language question
2. **Retrieval**: FAISS finds the most relevant document chunks
3. **Context Building**: Retrieved chunks are combined as context
4. **LLM Response**: OpenAI's GPT generates an answer using the context
5. **Source Attribution**: Original sources are extracted and displayed

## Configuration

### Text Splitting Parameters

- **`chunk_size`**: 1000 characters per chunk
- **`separators`**: `['\n\n', '\n', '.', ',']` (hierarchical priority)

### LLM Parameters

- **Temperature**: 0.9 (higher creativity, more variation in responses)
- **Max Tokens**: 500 (response length limit)

Modify these values in `main.py` to adjust behavior:
```python
llm = OpenAI(temperature=0.9, max_tokens=500)
text_splitter = RecursiveCharacterTextSplitter(
    separators=['\n\n', '\n', '.', ','],
    chunk_size=1000
)
```

## Example Queries

- "What are the latest developments in AI stocks?"
- "Compare the performance of Tesla and Ford"
- "What is the outlook for renewable energy companies?"
- "Summarize the key findings from the earnings report"

## API Costs

Please note that this application uses OpenAI's API services, which incur costs:
- **Embeddings**: ~$0.02 per 1M tokens
- **GPT Queries**: Varies by model (3.5-turbo or 4)

Monitor your API usage in the OpenAI dashboard to control costs.

## Limitations

- URLs must be publicly accessible
- Processing time depends on article length and number of URLs
- Quality of responses depends on source quality and relevance
- Rate limited by OpenAI API quotas

## Future Improvements

- [ ] Support for PDF and document uploads
- [ ] Multiple embedding model options
- [ ] Query history and analytics
- [ ] Custom prompt engineering
- [ ] Support for more than 3 URLs
- [ ] Real-time data integration
- [ ] Multi-language support
- [ ] API endpoint for integration

## Troubleshooting

### Issue: "API key not found"
**Solution**: Ensure your `.env` file contains `OPENAI_API_KEY` and is in the project root directory.

### Issue: "Failed to load URLs"
**Solution**: Verify URLs are publicly accessible and contain text content. Some websites may block scrapers.

### Issue: "Empty sources returned"
**Solution**: Try asking more specific questions related to the document content, or ensure documents have sufficient content.

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests to improve the application.

## License

This project is open source and available for educational and research purposes.

## Contact & Support

For questions or issues, please open an issue on the GitHub repository.

---

**Built with ❤️ using LangChain, Streamlit, and OpenAI APIs**
