# Preface
Thanks to the project [AI Hedge Fund](https://github.com/virattt/ai-hedge-fund). However, the original project was not tailored for Chinese investors and relied on prohibitively expensive data sources. In this restructuring, we preserve the core features of the original project while extending support for AI agent analysis of Chinese stocks, Chinese futures, and U.S. stocks. This adaptation aims to make the framework accessible and practical for investors in China while maintaining its foundational design.

感谢开源项目[AI Hedge Fund](https://github.com/virattt/ai-hedge-fund)，但是原来的项目并不适合中国的投资者且数据源过于昂贵，重构该项目并保留原有项目基本信息，使用免费的[akshare](https://akshare.akfamily.xyz/tutorial.html)数据源，支持中国股票、中国期货、美国股票的AI agents分析

Note: To use the Qwen series models, you need to manually install the [langchain-qwen](https://github.com/mapicccy/langchain_qwen) package, which is not officially provided.

注意：
1. 使用通义千问系列模型需要手动安装[langchain_qwen](https://github.com/mapicccy/langchain_qwen)，放置在langchain的同级目录即可，该包非官方提供
2. akshare数据源有很大的局限性，目前仅适配了Technical Analyst(做技术分析的agent)和Sentiment Analyst(做情感分析的agent)。如果选择所有AI agents，最终的结论将会不可信

# AI Hedge Fund

This is a proof of concept for an AI-powered hedge fund.  The goal of this project is to explore the use of AI to make trading decisions.  This project is for **educational** purposes only and is not intended for real trading or investment.

This system employs several agents working together:

1. Ben Graham Agent - The godfather of value investing, only buys hidden gems with a margin of safety
2. Bill Ackman Agent - An activist investors, takes bold positions and pushes for change
3. Cathie Wood Agent - The queen of growth investing, believes in the power of innovation and disruption
4. Charlie Munger Agent - Warren Buffett's partner, only buys wonderful businesses at fair prices
5. Peter Lynch Agent - Legendary growth investor who seeks "ten-baggers" and invests in what he knows
6. Phil Fisher Agent - Legendary growth investor who mastered scuttlebutt analysis
7. Stanley Druckenmiller Agent - Macro legend who hunts for asymmetric opportunities with growth potential
8. Warren Buffett Agent - The oracle of Omaha, seeks wonderful companies at a fair price
9. Valuation Agent - Calculates the intrinsic value of a stock and generates trading signals
10. Sentiment Agent - Analyzes market sentiment and generates trading signals
11. Fundamentals Agent - Analyzes fundamental data and generates trading signals
12. Technicals Agent - Analyzes technical indicators and generates trading signals
13. Risk Manager - Calculates risk metrics and sets position limits
14. Portfolio Manager - Makes final trading decisions and generates orders
    
<img width="1042" alt="Screenshot 2025-03-22 at 6 19 07 PM" src="https://github.com/user-attachments/assets/cbae3dcf-b571-490d-b0ad-3f0f035ac0d4" />


**Note**: the system simulates trading decisions, it does not actually trade.

[![Twitter Follow](https://img.shields.io/twitter/follow/virattt?style=social)](https://twitter.com/virattt)

## Disclaimer

This project is for **educational and research purposes only**.

- Not intended for real trading or investment
- No warranties or guarantees provided
- Past performance does not indicate future results
- Creator assumes no liability for financial losses
- Consult a financial advisor for investment decisions

By using this software, you agree to use it solely for learning purposes.

## Table of Contents
- [Setup](#setup)
- [Usage](#usage)
  - [Running the Hedge Fund](#running-the-hedge-fund)
  - [Running the Backtester](#running-the-backtester)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [Feature Requests](#feature-requests)
- [License](#license)

## Setup

Clone the repository:
```bash
git clone https://github.com/virattt/ai-hedge-fund.git
cd ai-hedge-fund
```

1. Install Poetry (if not already installed):
```bash
curl -sSL https://install.python-poetry.org | python3 -
```

2. Install dependencies:
```bash
poetry install
```

3. Set up your environment variables:
```bash
# Create .env file for your API keys
cp .env.example .env
```

4. Set your API keys:
```bash
# For running LLMs hosted by openai (gpt-4o, gpt-4o-mini, etc.)
# Get your OpenAI API key from https://platform.openai.com/
OPENAI_API_KEY=your-openai-api-key

# For running LLMs hosted by groq (deepseek, llama3, etc.)
# Get your Groq API key from https://groq.com/
GROQ_API_KEY=your-groq-api-key

# For running LLMs hosted by deepseek
# Get your deepseek API key from https://platform.deepseek.com/api_keys
DEEPSEEK_API_KEY=your-deepseek-api-key

# For getting financial data to power the hedge fund, non-essential (Now we support [akshare](https://akshare.akfamily.xyz/tutorial.html))
# Get your Financial Datasets API key from https://financialdatasets.ai/
FINANCIAL_DATASETS_API_KEY=your-financial-datasets-api-key
```

**Important**: You must set `OPENAI_API_KEY`, `GROQ_API_KEY`, `ANTHROPIC_API_KEY`, or `DEEPSEEK_API_KEY` for the hedge fund to work.  If you want to use LLMs from all providers, you will need to set all API keys.

Financial data for AAPL, GOOGL, MSFT, NVDA, and TSLA is free and does not require an API key.

For any other ticker, you will need to set the `FINANCIAL_DATASETS_API_KEY` in the .env file or [akshare](https://akshare.akfamily.xyz/tutorial.html) adapted order book id.

## Usage

### Running the Hedge Fund
```bash
poetry run python src/main.py --ticker AAPL,MSFT,NVDA
```

**Example Output:**
<img width="992" alt="Screenshot 2025-01-06 at 5 50 17 PM" src="https://github.com/user-attachments/assets/e8ca04bf-9989-4a7d-a8b4-34e04666663b" />

You can also specify a `--show-reasoning` flag to print the reasoning of each agent to the console.

```bash Chinese A shares
poetry run python src/main.py --ticker 601360 --show-reasoning
```
You can optionally specify assets type (now we support A for Chinese A shares, US for U.S. stocks, and others for Chinese futures), the start and end dates to make decisions for a specific time period.

```bash futurepoetry run python src/main.py --ticker JM2505 --assets futures --start-date 2024-01-01 --end-date 2025-03-01
poetry run python src/main.py --ticker JM2505 --type futures --start-date 2024-01-01 --end-date 2025-03-01

### Running the Backtester

```bash
poetry run python src/backtester.py --ticker 601360
```

**Example Output:**
<img width="941" alt="Screenshot 2025-01-06 at 5 47 52 PM" src="https://github.com/user-attachments/assets/00e794ea-8628-44e6-9a84-8f8a31ad3b47" />

You can optionally specify the start and end dates to backtest over a specific time period.

```bash
poetry run python src/backtester.py --ticker AAPL,MSFT,NVDA --start-date 2024-01-01 --end-date 2024-03-01
```

## Project Structure 
```
ai-hedge-fund/
├── src/
│   ├── agents/                   # Agent definitions and workflow
│   │   ├── bill_ackman.py        # Bill Ackman agent
│   │   ├── fundamentals.py       # Fundamental analysis agent
│   │   ├── portfolio_manager.py  # Portfolio management agent
│   │   ├── risk_manager.py       # Risk management agent
│   │   ├── sentiment.py          # Sentiment analysis agent
│   │   ├── technicals.py         # Technical analysis agent
│   │   ├── valuation.py          # Valuation analysis agent
│   │   ├── warren_buffett.py     # Warren Buffett agent
│   ├── tools/                    # Agent tools
│   │   ├── api.py                # API tools
│   ├── backtester.py             # Backtesting tools
│   ├── main.py # Main entry point
├── pyproject.toml
├── ...
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

**Important**: Please keep your pull requests small and focused.  This will make it easier to review and merge.

## Feature Requests

If you have a feature request, please open an [issue](https://github.com/virattt/ai-hedge-fund/issues) and make sure it is tagged with `enhancement`.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
