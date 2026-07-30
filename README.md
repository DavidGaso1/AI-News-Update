# AI Daily Newsletter

An automated AI newsletter that fetches, summarizes, and categorizes AI news from multiple sources daily.

## Features

- **Automated daily fetching** at 6 AM via cron
- **Multiple sources**: RSS feeds, Reddit, Hacker News, Twitter/X (via Nitter)
- **LLM-powered summarization** using Hermes Agent
- **Smart categorization**: Breaking News, Research Papers, Industry Updates, Tools & Releases
- **Modern, responsive web UI** with dark/light theme toggle
- **Static site** - no backend needed, just serve the HTML

## Project Structure

```
ai-newsletter/
├── index.html              # Main newsletter page
├── data/
│   └── newsletter.json     # Generated newsletter data
├── scripts/
│   ├── fetch-news.py       # Main fetch & summarization script
│   └── run-fetch.sh        # Cron wrapper script
├── logs/                   # Cron execution logs
├── requirements.txt        # Python dependencies
└── README.md              # This file
```

## Installation

```bash
cd /home/shi/ai-newsletter
pip install -r requirements.txt
```

## Usage

### Manual Run
```bash
cd /home/shi/ai-newsletter
python3 scripts/fetch-news.py
```

### View Newsletter
Open `index.html` in a browser, or serve locally:
```bash
cd /home/shi/ai-newsletter
python3 -m http.server 8080
# Then open http://localhost:8080
```

### Cron Job
The cron job is set to run daily at 6 AM:
```bash
crontab -l
# 0 6 * * * /home/shi/ai-newsletter/scripts/run-fetch.sh
```

Logs are saved to `logs/fetch-news-YYYYMMDD-HHMMSS.log` and symlinked as `logs/latest.log`.

## Data Sources

### RSS Feeds
- arXiv (AI, ML, CV, CL categories)
- Hugging Face Blog
- OpenAI Blog
- Anthropic Blog (when available)
- Google AI Blog
- Microsoft Research Blog
- Meta AI Blog
- NVIDIA Blog
- Google DeepMind Blog
- Cohere Blog
- LangChain Blog
- Weights & Biases Blog
- AssemblyAI Blog
- Replicate Blog
- Together AI Blog

### Reddit
- r/MachineLearning
- r/ArtificialIntelligence
- r/LocalLLaMA
- r/MLQuestions
- r/ComputerVision
- r/LanguageTechnology

### Hacker News
- AI/ML tagged stories via Algolia API

### Twitter/X (via Nitter)
- Top AI researchers and companies

## Configuration

Edit `scripts/fetch-news.py` to:
- Add/remove RSS feeds in `RSS_FEEDS` dict
- Add/remove Reddit sources in `REDDIT_FEEDS` dict
- Adjust categorization keywords in `CATEGORY_KEYWORDS`
- Change LLM model via environment variables

Environment variables:
- `HERMES_API_URL` - LLM API endpoint (default: localhost:8080/v1)
- `HERMES_MODEL` - Model name (default: nemotron-3-ultra-free)
- `HERMES_PROVIDER` - Provider (default: opencode-zen)

## Output Format

The script generates `data/newsletter.json` with:
```json
{
  "generated_at": "2026-07-26T08:00:00Z",
  "article_count": 50,
  "categories": {
    "breaking": [...],
    "research": [...],
    "industry": [...],
    "tools": [...]
  },
  "all_articles": [...]
}
```

Each article has:
- `title`, `url`, `source`
- `summary` (LLM-generated)
- `category` (breaking/research/industry/tools)
- `published_at` (ISO timestamp)
- `author`, `tags`

## Troubleshooting

### No articles fetched
- Check network connectivity
- Some RSS feeds may have changed URLs
- Reddit rate limits may apply (429 errors)
- Nitter instances may be down

### LLM summarization fails
- Ensure Hermes Agent is running with API server
- Check `HERMES_API_URL` environment variable
- Script falls back to extractive summarization

### Cron not running
- Check `crontab -l` shows the job
- Check `logs/latest.log` for errors
- Ensure virtual environment path is correct in `run-fetch.sh`

## License

MIT