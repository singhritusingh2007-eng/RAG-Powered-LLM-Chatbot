# 🚀 Quick Start Guide

Get your RAG Chatbot running in 5 minutes!

## Step 1: Get Your Hugging Face Token

1. Go to [huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)
2. Create a new token with **Read** permissions
3. Copy and save your token (you'll need it in Step 3)

## Step 2: Clone the Repository

```bash
git clone https://github.com/singhritusingh2007-eng/RAG-Powered-LLM-Chatbot.git
cd RAG-Powered-LLM-Chatbot
```

## Step 3: Create `.env` File

```bash
echo "HUGGINGFACEHUB_API_TOKEN=hf_your_token_here" > .env
```

Replace `hf_your_token_here` with your actual token from Step 1.

## Step 4: Install & Run

### Option A: Using Virtual Environment (Recommended)

```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Run the app
streamlit run chatbot_streamlit_combined.py
```

### Option B: Direct Installation

```bash
pip install -r requirements.txt
streamlit run chatbot_streamlit_combined.py
```

## Step 5: Access the App

- Open your browser to: **http://localhost:8501**
- The app will automatically open in your default browser

---

## 📚 First Time Usage

### 1. Create a Knowledge Base

1. Go to **"Document Embedding"** tab
2. Upload PDF or TXT files
3. Click **"Save vector store"**
4. Wait for processing (takes 1-2 minutes)

### 2. Chat with Your Documents

1. Go to **"RAG Chatbot"** tab
2. Click **"Initialize the LLM Model"**
3. Select your vector store from the dropdown
4. Click **"Launch chatbot"**
5. Start asking questions!

---

## 🛠️ System Requirements

- **Python**: 3.8 or higher
- **RAM**: 8GB minimum, 16GB recommended
- **Storage**: 10GB free space
- **GPU**: Optional but recommended for faster inference

## ⚠️ Common Issues

### "Module not found" Error
```bash
# Make sure virtual environment is activated, then:
pip install -r requirements.txt
```

### "Token not found" Error
```bash
# Verify .env file exists:
cat .env
# Should show: HUGGINGFACEHUB_API_TOKEN=hf_...
```

### "Vector store not found" Error
```bash
# Create vector store first:
# 1. Go to Document Embedding tab
# 2. Upload files and save vector store
```

### Out of Memory
```bash
# Reduce chunk size in Document Embedding:
# Change "Chunk Size" from 200 to 100
```

---

## 📖 Full Documentation

- [README.md](README.md) - Complete documentation
- [DEPLOYMENT.md](DEPLOYMENT.md) - Production deployment guides
- [GitHub Issues](https://github.com/singhritusingh2007-eng/RAG-Powered-LLM-Chatbot/issues) - Get help

---

## 🎉 Next Steps

1. ✅ Try the chatbot with your own documents
2. ✅ Experiment with different settings (temperature, chunk size)
3. ✅ Deploy to production ([see DEPLOYMENT.md](DEPLOYMENT.md))
4. ✅ Star the repository ⭐
5. ✅ Share your feedback!

---

**Need help?** Open an issue on GitHub or check the README.md for more details!
