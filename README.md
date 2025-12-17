# AI Personal Assistant

A conversational AI assistant powered by OpenAI GPT-4 that represents you on your website, answers questions about your background, and captures leads automatically.

## 🎯 What It Does

- **Answers questions** about your professional background using your resume/LinkedIn
- **Engages visitors** in natural conversation 24/7
- **Captures leads** intelligently (email, name, conversation context)
- **Sends notifications** in real-time when someone wants to connect (via Pushover)

## 🛠️ Tech Stack

- **OpenAI GPT-4** - Conversational AI
- **Gradio** - Web UI framework
- **Python 3.12+** - Backend
- **Pushover API** - Optional push notifications
- **pypdf** - LinkedIn PDF parsing

## 📁 Project Structure

```
ai-personal-assistant/
├── app.py              # Main application (Gradio + OpenAI)
├── me/
│   ├── summary.txt    # Your professional summary
│   └── linkedin.pdf   # Your LinkedIn profile PDF
├── requirements.txt    # Python dependencies
├── .env.example       # Environment variables template
├── .gitignore         # Git ignore rules
├── Procfile          # Heroku deployment config
├── render.yaml       # Render deployment config
├── Dockerfile        # Docker configuration
└── docker-compose.yml # Docker Compose setup
```

## 🚀 Quick Start

### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

### 2. Configure Environment

Create `.env` file:
```bash
OPENAI_API_KEY=sk-your-openai-key-here
PUSHOVER_TOKEN=your-pushover-token  # Optional
PUSHOVER_USER=your-pushover-user    # Optional
```

### 3. Customize Your Data

**Update `me/summary.txt`:**
Replace with your professional background (2-3 sentences)

**Replace `me/linkedin.pdf`:**
- Go to your LinkedIn profile
- Click "More" → "Save to PDF"
- Save as `me/linkedin.pdf`

**Update your name in `app.py`:**
```python
# Line 81
self.name = "Your Name"  # Change this!
```

### 4. Run Locally

```bash
python app.py
```

Visit http://localhost:7860 and chat with your AI!

## 🌐 Deployment Options

### Hugging Face Spaces (Free - Recommended)

1. Create Space at [huggingface.co/spaces](https://huggingface.co/spaces)
2. Choose **Gradio** SDK
3. Clone and copy files:
```bash
git clone https://huggingface.co/spaces/YOUR_USERNAME/ai-assistant
cd ai-assistant
cp ../ai-personal-assistant/* .
cp -r ../ai-personal-assistant/me .
git add .
git commit -m "Initial deployment"
git push
```
4. Add secrets in Space Settings:
   - `OPENAI_API_KEY`
   - `PUSHOVER_TOKEN` (optional)
   - `PUSHOVER_USER` (optional)

### Railway

1. Connect GitHub repo at [railway.app](https://railway.app)
2. Add environment variables
3. Set start command: `python app.py`
4. Deploy!

### Render

1. Connect repo at [render.com](https://render.com)
2. Config is in `render.yaml`
3. Add environment variables
4. Deploy!

### Heroku

```bash
heroku create your-app-name
heroku config:set OPENAI_API_KEY=sk-...
git push heroku main
```

### Docker

```bash
docker build -t ai-assistant .
docker run -p 7860:7860 -e OPENAI_API_KEY=your_key ai-assistant
```

Or use Docker Compose:
```bash
docker-compose up -d
```

## 💡 How It Works

### Architecture

```
User → Gradio Interface → OpenAI GPT-4 → Function Calling
                              ↓
                    [record_user_details]
                    [record_unknown_question]
                              ↓
                    Pushover Notification
```

### Key Features

1. **Context Loading**: Reads your `summary.txt` and `linkedin.pdf` on startup
2. **Function Calling**: Uses OpenAI's function calling to:
   - Capture visitor contact info
   - Log questions it can't answer
3. **Real-time Notifications**: Pushover alerts when leads are captured
4. **Stateless**: Each conversation is independent (no database needed)

## 💰 Costs

**OpenAI API (gpt-4o-mini):**
- ~$0.01-0.05 per conversation
- Set spending limits at [platform.openai.com](https://platform.openai.com/account/limits)

**Hosting:**
- Hugging Face Spaces: **Free**
- Railway: $5 credit/month free
- Render: 750 hours/month free
- Heroku: $7+/month

## 🔒 Security

- ✅ `.env` file excluded from Git (contains API keys)
- ✅ Environment variables for production secrets
- ✅ No conversation data stored permanently
- ✅ User data only sent to Pushover (if configured)

## 🎨 Customization

### Change AI Personality

Edit `system_prompt()` in `app.py`:
```python
system_prompt = f"You are {self.name}. You are [add personality traits]..."
```

### Modify Data Collection

Edit `record_user_details_json` in `app.py` to add/remove fields:
```python
"properties": {
    "email": {...},
    "name": {...},
    "company": {...},  # Add new fields
}
```

### Switch AI Model

In `chat()` method:
```python
model="gpt-4o-mini"  # Or "gpt-4o" for more capability (higher cost)
```

## 📊 Monitoring

- **API Usage**: [OpenAI Dashboard](https://platform.openai.com/usage)
- **Set Limits**: Account settings → Usage limits
- **Logs**: Check hosting platform logs or terminal output

## 🐛 Troubleshooting

**"No module named 'openai'"**
```bash
pip install -r requirements.txt
```

**"OpenAI API key not found"**
- Verify `.env` file exists with `OPENAI_API_KEY=sk-...`
- For deployment, add in platform's environment variables

**PDF not loading**
- Ensure file is at `me/linkedin.pdf`
- Try re-downloading from LinkedIn

**Port already in use**
```python
# In app.py, change launch line:
gr.ChatInterface(me.chat, type="messages").launch(server_port=7861)
```

## 📄 License

MIT License - Feel free to use for your own portfolio!

## 🤝 Contributing

This is a portfolio project, but improvements are welcome:
- Bug fixes
- Documentation improvements
- Alternative deployment guides

## 📧 Questions?

Open an issue or customize this for your own use!

---

**Built with** 🤖 **OpenAI GPT-4, Gradio, and Python**
