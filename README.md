# 🤖 RAG-Powered LLM Chatbot

[![Python Version](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://rag-chatbot.streamlit.app)

> **Advanced Retrieval-Augmented Generation (RAG) Chatbot** - Chat with your documents using Meta Llama 3 LLM, powered by LangChain and Streamlit.

## 🌟 Features

- 📚 **Multi-Document Support** - Upload PDF and TXT files to build your custom knowledge base
- 🔍 **Vector Search** - Efficient document retrieval using FAISS embeddings
- 💬 **Conversational AI** - Context-aware responses with conversation memory
- 🦙 **Meta Llama 3 Integration** - State-of-the-art open-source LLM
- 🎨 **Interactive UI** - Beautiful Streamlit interface with chat history
- 📝 **Source Attribution** - Track which documents the chatbot references
- ⚡ **GPU Memory Management** - Optimized for resource-constrained environments
- 🔐 **Secure Token Management** - Environment variable-based API key handling

## 🚀 Quick Start

### Prerequisites

- Python 3.8 or higher
- Hugging Face API token ([Get one here](https://huggingface.co/settings/tokens))
- GPU or CPU (GPU recommended for faster inference)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/singhritusingh2007-eng/RAG-Powered-LLM-Chatbot.git
   cd RAG-Powered-LLM-Chatbot
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Set up environment variables**
   ```bash
   # Create a .env file in the project root
   echo "HUGGINGFACEHUB_API_TOKEN=your_token_here" > .env
   ```

5. **Run the application**
   ```bash
   streamlit run chatbot_streamlit_combined.py
   ```

The app will open at `http://localhost:8501`

## 📖 Usage Guide

### 1. Document Embedding

1. Navigate to **"Document Embedding"** in the sidebar
2. Upload PDF or TXT files containing your knowledge base
3. Configure embedding parameters:
   - **Model**: Sentence transformers model (default: `sentence-transformers/all-MiniLM-L6-v2`)
   - **Chunk Size**: Document splitting size (default: 200)
   - **Chunk Overlap**: Overlapping tokens between chunks (default: 10)
4. Select or create a vector store to save embeddings
5. Click **"Save vector store"**

### 2. RAG Chatbot

1. Navigate to **"RAG Chatbot"** in the sidebar
2. Click **"Initialize the LLM Model"** to expand settings
3. Configure LLM parameters:
   - **Hugging Face Token**: Your API token (pre-filled if set in .env)
   - **Vector Store**: Select your saved knowledge base
   - **Temperature**: Controls randomness (0-1, default: 1.0)
   - **Max Length**: Maximum response length (default: 300)
4. Click **"Launch chatbot"** to initialize
5. Ask questions in the chat input
6. View **Chat History and Source Information** for document references

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────┐
│           Streamlit Web Interface                   │
├─────────────────────────────────────────────────────┤
│  Document Upload       │       RAG Chatbot          │
│  • PDF/TXT Parser     │  • Query Input              │
│  • Text Splitting     │  • Conversation Memory      │
│  • Embeddings         │  • Source Attribution       │
├─────────────────────────────────────────────────────┤
│  LangChain Framework                                │
│  • ConversationalRetrievalChain                     │
│  • ConversationBufferWindowMemory                   │
├─────────────────────────────────────────────────────┤
│  Vector Store & Embeddings                          │
│  • FAISS Vector Database                            │
│  • Sentence Transformers                            │
├─────────────────────────────────────────────────────┤
│  LLM Layer                                          │
│  • Meta Llama 3 (8B parameters)                     │
│  • Hugging Face Endpoint                            │
└─────────────────────────────────────────────────────┘
```

## 📂 Project Structure

```
RAG-Powered-LLM-Chatbot/
├── chatbot_streamlit_combined.py  # Main Streamlit application
├── falcon.py                       # RAG pipeline & utility functions
├── requirements.txt                # Python dependencies
├── .env.example                    # Environment variables template
├── vector store/                   # Local vector store storage
│   └── naruto_snake/               # Example knowledge base
├── Document-pdfs/                  # Sample documents
└── README.md                       # This file
```

## 🔧 Configuration

### Environment Variables

Create a `.env` file in the project root:

```env
# Required: Your Hugging Face API token
HUGGINGFACEHUB_API_TOKEN=hf_your_token_here

# Optional: Model configurations (use defaults if not set)
# LLM_MODEL=meta-llama/Meta-Llama-3-8B
# EMBEDDING_MODEL=sentence-transformers/all-MiniLM-L6-v2
```

### Advanced Settings

Modify parameters in `chatbot_streamlit_combined.py`:

```python
# Temperature: Controls creativity (0 = deterministic, 1 = creative)
temperature = st.number_input("Temperature", value=1.0, step=0.1)

# Max Length: Maximum tokens in response
max_length = st.number_input("Maximum character length", value=300, step=1)

# Chunk Size: Document splitting size
chunk_size = st.number_input("Chunk Size", value=200, min_value=0, step=1)
```

## 📦 Dependencies

- **langchain** (0.1+) - LLM framework
- **langchain_community** - Community integrations
- **streamlit** (1.28+) - Web interface
- **faiss-cpu** - Vector database
- **sentence-transformers** (2.2.2) - Embedding model
- **pypdf** - PDF processing
- **python-dotenv** - Environment management
- **torch** - Deep learning framework
- **pynvml** - GPU memory monitoring

See `requirements.txt` for complete list.

## 🚀 Deployment

### Option 1: Streamlit Cloud (Recommended)

1. **Push to GitHub**
   ```bash
   git add .
   git commit -m "Initial RAG chatbot release"
   git push origin main
   ```

2. **Deploy on Streamlit Cloud**
   - Go to [share.streamlit.io](https://share.streamlit.io)
   - Click **"New app"**
   - Select your repository, branch, and main file
   - Add secrets in **"Advanced settings"**:
     ```
     HUGGINGFACEHUB_API_TOKEN = "your_token_here"
     ```
   - Click **"Deploy"**

### Option 2: Docker Deployment

```dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8501

CMD ["streamlit", "run", "chatbot_streamlit_combined.py"]
```

**Build and run:**
```bash
docker build -t rag-chatbot .
docker run -p 8501:8501 -e HUGGINGFACEHUB_API_TOKEN=your_token rag-chatbot
```

### Option 3: Traditional Server

1. **Set up on your server**
   ```bash
   git clone <your-repo>
   cd RAG-Powered-LLM-Chatbot
   pip install -r requirements.txt
   ```

2. **Run with systemd or supervisor**
   ```bash
   streamlit run chatbot_streamlit_combined.py --server.port=8501
   ```

3. **Use Nginx as reverse proxy** (optional)

## 📊 Performance Metrics

| Task | Time | Memory |
|------|------|--------|
| Document Upload (10 PDFs) | ~30 seconds | 2-3 GB |
| Vector Store Creation | ~1 minute | 1-2 GB |
| Query Response | 5-15 seconds | 4-6 GB |
| Conversation Memory | Real-time | <500 MB |

*Metrics are approximate and depend on document size and hardware*

## 🐛 Troubleshooting

### Issue: "API Token not found"
**Solution:** Ensure `.env` file exists with `HUGGINGFACEHUB_API_TOKEN` set
```bash
echo "HUGGINGFACEHUB_API_TOKEN=your_token" > .env
```

### Issue: "CUDA out of memory"
**Solution:** Clear GPU cache or use CPU
```python
# In chatbot_streamlit_combined.py
import torch
torch.cuda.empty_cache()
```

### Issue: "Vector store not found"
**Solution:** 
1. Create vector store in "Document Embedding" tab first
2. Ensure `vector store/` directory exists

### Issue: "Slow response times"
**Solution:**
- Reduce chunk overlap and size
- Increase temperature threshold
- Use CPU mode if GPU is bottlenecked

## 📝 Example Usage

```python
# Basic setup
from falcon import prepare_rag_llm, generate_answer
from dotenv import load_dotenv
import os

load_dotenv()

# Initialize
qa_chain = prepare_rag_llm(
    vector_store_list="my_knowledge_base",
    temperature=0.7,
    max_length=300
)

# Ask question
answer, sources = generate_answer(
    question="What is RAG?",
    token=os.getenv("HUGGINGFACEHUB_API_TOKEN")
)

print(f"Answer: {answer}")
print(f"Sources: {sources}")
```

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Meta Llama 3](https://www.llama.com/) - Large language model
- [LangChain](https://www.langchain.com/) - LLM framework
- [Streamlit](https://streamlit.io/) - Web app framework
- [FAISS](https://github.com/facebookresearch/faiss) - Vector similarity search
- [Hugging Face](https://huggingface.co/) - Model hosting and APIs

## 📧 Contact & Support

- **GitHub Issues**: [Open an issue](https://github.com/singhritusingh2007-eng/RAG-Powered-LLM-Chatbot/issues)
- **Email**: Contact via GitHub profile
- **Documentation**: See [Docs](https://github.com/singhritusingh2007-eng/RAG-Powered-LLM-Chatbot/wiki)

## 🔗 Useful Links

- [Hugging Face API Docs](https://huggingface.co/docs/api)
- [LangChain Documentation](https://python.langchain.com/)
- [Streamlit Documentation](https://docs.streamlit.io/)
- [FAISS Index Documentation](https://github.com/facebookresearch/faiss/wiki)

---

**⭐ If you find this project helpful, please consider giving it a star!**

Made with ❤️ by [Ritusi Singh](https://github.com/singhritusingh2007-eng)