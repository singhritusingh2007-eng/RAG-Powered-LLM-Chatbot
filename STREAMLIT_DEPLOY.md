# 🚀 Streamlit Cloud Deployment Checklist

**Complete this checklist before deploying your RAG Chatbot to production!**

## Pre-Deployment Checklist

### Code Quality
- [ ] All Python files pass syntax check (`python -m py_compile *.py`)
- [ ] No hardcoded secrets or API keys
- [ ] All imports are correct and available in `requirements.txt`
- [ ] Code is properly indented (4 spaces)
- [ ] No unused imports or variables
- [ ] Functions have docstrings
- [ ] Error handling is implemented

### Repository Setup
- [ ] Repository is public on GitHub
- [ ] `.env.example` file exists with template variables
- [ ] `.gitignore` properly excludes sensitive files
- [ ] `requirements.txt` has all dependencies with versions pinned
- [ ] `README.md` is comprehensive and up-to-date
- [ ] License file exists (LICENSE.md or LICENSE)
- [ ] All files are committed and pushed to main branch

### Configuration Files
- [ ] `streamlit_config.toml` exists in root directory
- [ ] `.streamlit/config.toml` created locally (not pushed)
- [ ] Entry point is `chatbot_streamlit_combined.py`

### Testing
- [ ] Tested locally with `streamlit run chatbot_streamlit_combined.py`
- [ ] Tested document upload functionality
- [ ] Tested vector store creation
- [ ] Tested chat functionality
- [ ] Tested with sample documents

---

## Streamlit Cloud Deployment Steps

### Step 1: Prepare GitHub Repository

```bash
# 1. Verify all changes are committed
git status

# 2. Push to main branch
git push origin main

# 3. Verify on GitHub
# Open https://github.com/singhritusingh2007-eng/RAG-Powered-LLM-Chatbot
```

**Checklist**:
- [ ] All files visible on GitHub
- [ ] No sensitive data in public files
- [ ] `requirements.txt` is visible
- [ ] `chatbot_streamlit_combined.py` is visible

### Step 2: Create Streamlit Cloud Account

1. Go to [share.streamlit.io](https://share.streamlit.io)
2. Click **Sign up**
3. Choose **GitHub** authentication
4. Authorize Streamlit to access your repositories
5. Click **Create account**

**Checklist**:
- [ ] Account created successfully
- [ ] GitHub authorization complete
- [ ] Email verified

### Step 3: Deploy Application

1. Click **Create app**
2. Select deployment method: **GitHub**
3. Choose:
   - **Repository**: `singhritusingh2007-eng/RAG-Powered-LLM-Chatbot`
   - **Branch**: `main`
   - **Main file path**: `chatbot_streamlit_combined.py`

**Checklist**:
- [ ] Repository selected
- [ ] Branch is `main`
- [ ] Main file is `chatbot_streamlit_combined.py`
- [ ] URL format looks correct

### Step 4: Configure Secrets

1. **IMPORTANT**: Before deployment, add secrets:
   - Click **Advanced settings** at the bottom
   - Scroll to **Secrets**
   - Add your secret:

```toml
HUGGINGFACEHUB_API_TOKEN = "hf_your_actual_token_here"
```

2. **DON'T** add `.env` file to GitHub!
3. Secrets are environment variables only available at runtime

**Checklist**:
- [ ] Token is correct format: `hf_...`
- [ ] Copied from huggingface.co/settings/tokens
- [ ] No quotes around the token value in the secrets panel
- [ ] Secret is marked as secret (hidden field)

### Step 5: Deploy

1. Click **Deploy** button
2. Wait for deployment (2-5 minutes)
3. Watch the logs for errors
4. App will be live at: `https://your-app-name.streamlit.app`

**Checklist**:
- [ ] Deployment started
- [ ] No errors in logs
- [ ] App is live and accessible
- [ ] URL is bookmarked

---

## Post-Deployment Verification

### Functionality Testing

1. **Open the App**
   - Navigate to: `https://your-app-name.streamlit.app`
   - Should load without errors
   - Sidebar should display all options

2. **Test Document Upload**
   - Go to "Document Embedding" tab
   - Upload a test PDF or text file
   - Check: Progress indicator appears

3. **Test Vector Store Creation**
   - Click "Save vector store"
   - Should complete without errors
   - New vector store should appear in dropdown

4. **Test Chat Functionality**
   - Go to "RAG Chatbot" tab
   - Click "Initialize the LLM Model"
   - Select vector store
   - Click "Launch chatbot"
   - Ask a question about uploaded documents
   - Should receive relevant answers

**Checklist**:
- [ ] App loads successfully
- [ ] Document upload works
- [ ] Vector store creation works
- [ ] Chat responds appropriately
- [ ] No console errors

### Performance Checks

- [ ] Page loads in <5 seconds
- [ ] Document processing completes in <2 minutes
- [ ] Chat response time <30 seconds
- [ ] No memory errors in logs

---

## Troubleshooting

### Issue: "App failed to deploy"

**Solution Steps**:
1. Check app logs: Click "Manage app" → "View logs"
2. Common causes:
   - `ModuleNotFoundError`: Add to `requirements.txt`
   - `IndentationError`: Check Python syntax
   - `ImportError`: Verify import statement

3. Fix and push:
   ```bash
   git push origin main
   ```
4. Rerun deployment in Streamlit Cloud

### Issue: "API token not working"

**Solution**:
1. Verify token in Streamlit Cloud secrets
2. Verify token format: Should start with `hf_`
3. Check token at: [huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)
4. Create new token if needed
5. Update secret and redeploy

### Issue: "Vector store not found"

**Solution**:
1. This is expected on first deploy
2. Upload documents and create vector store from UI
3. Vector store persists between refreshes
4. Contact if it persists after 5 minutes

### Issue: "Out of memory"

**Solution**:
1. Reduce chunk size: 200 → 100
2. Reduce overlap: 50 → 25
3. Use smaller model if available
4. Contact Streamlit support for upgrade

### Issue: "Slow response time"

**Solution**:
1. Streamlit Free tier has resource limits
2. Upgrade to Streamlit Growth tier
3. Or deploy to AWS/GCP for better performance

---

## Post-Deployment Maintenance

### Weekly
- [ ] Check app is still running
- [ ] Monitor error logs
- [ ] Test basic functionality

### Monthly
- [ ] Review usage stats
- [ ] Check for updates to dependencies
- [ ] Backup vector stores

### As Needed
- [ ] Update code and push to GitHub
- [ ] App automatically redeploys
- [ ] Monitor logs during deployment

---

## Important Notes

⚠️ **DO NOT**:
- Commit `.env` file to GitHub
- Share API tokens publicly
- Use free tier for production (limited resources)
- Disable secure headers

✅ **DO**:
- Use Streamlit secrets for sensitive data
- Regularly update dependencies
- Monitor app performance
- Keep GitHub repository updated
- Use strong passwords

---

## Getting Help

- **Streamlit Docs**: [docs.streamlit.io](https://docs.streamlit.io)
- **GitHub Issues**: [Open an issue](https://github.com/singhritusingh2007-eng/RAG-Powered-LLM-Chatbot/issues)
- **Streamlit Community**: [discuss.streamlit.io](https://discuss.streamlit.io)
- **Hugging Face Docs**: [huggingface.co/docs](https://huggingface.co/docs)

---

**Your app is now live! 🎉**

Visit your app at: `https://your-app-name.streamlit.app`
