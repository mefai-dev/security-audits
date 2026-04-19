# DeAgentAI Security Audit Report

## Project Information

| Field | Value |
|-------|-------|
| Project | DeAgentAI |
| Token | AIA |
| Chain | BNB Smart Chain |
| Audit Date | April 19 2026 |
| Auditor | MEFAI Security Research |
| Risk Score | 9/100 (CRITICAL) |

---

## Executive Summary

This audit compares DeAgentAI whitepaper claims against their actual codebase implementation. The analysis reveals systematic misrepresentation of technical capabilities and extensive code plagiarism from multiple third party projects. All backend services are offline and smart contracts show zero usage despite claims of millions of users.

---

## Critical Finding 1: Whitepaper Technical Claims Do Not Exist in Code

The whitepaper advertises advanced AI and blockchain technologies. None of them exist in the codebase.

| Whitepaper Claim | Code Search Result |
|------------------|-------------------|
| zkML (Zero Knowledge Machine Learning) | 0 lines of code |
| MPC (Multi Party Computation) | 0 implementations |
| Lobe Consensus Protocol | 0 implementations |
| Entropy Based Agent Selection | 0 lines of code |
| Agent to Agent Communication Protocol | Not implemented |
| Proprietary AI Engine | Does not exist |

### What They Actually Built

The entire AI system is a wrapper around third party APIs:

```python
# File: agents/bubbleagent/__main__.py line 31
headers = {
    "authorization": "Bearer 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"
}
```

```python
# File: agents/bubbleagent/config.py line 7
OPENAI_URL: str = "https://api.openai.com"
```

There is no proprietary AI. The project simply calls Perplexity and OpenAI APIs.

### How To Verify

```bash
git clone https://github.com/DeAgentAI/deagent-alpha
grep -r "zkml" .
grep -r "mpc" .
grep -r "lobe" .
grep -r "entropy" .
```

All searches return zero results.

---

## Critical Finding 2: Code Stolen From BubbleAI

The codebase is copied from a project called BubbleAI. They forgot to remove the original branding.

```python
# File: agents/bubbleagent/__main__.py line 68
"content": "You are BubbleAI LLM model and be precise and concise"
```

```python
# File: agents/bubbleagent/__main__.py line 14
# welcome_msg = """Hello, I am the smart assistant from BubbleAI!
```

```python
# File: agents/bubbleagent/utils/fetch_data.py line 277
url = f"https://bubbleai.xyz/api/v1/bubble/trending_token"
```

The AI still identifies itself as BubbleAI. 47 files import from bubbleagent module.

### How To Verify

```bash
git clone https://github.com/DeAgentAI/deagent-alpha
grep -r "BubbleAI" .
grep -r "bubbleai.xyz" .
```

---

## Critical Finding 3: ElizaOS Repository Has Zero Original Code

The eliza-alphax repository is a pure fork of ElizaOS with zero contributions from DeAgentAI.

| Metric | Value |
|--------|-------|
| Total Commits | 15134 |
| Commits from ElizaOS Contributors | 15134 |
| Commits from DeAgentAI | 0 |

They forked ElizaOS and claimed it as their own technology.

### How To Verify

```bash
git clone https://github.com/DeAgentAI/eliza-alphax
git log --oneline --format="%an" | sort | uniq -c | sort -rn | head -10
```

Every single commit is from ElizaOS developers. DeAgentAI contributed nothing.

---

## Critical Finding 4: Fake Transaction Hash

The transaction module does not execute real blockchain transactions. It returns a hardcoded fake hash.

```python
# File: agents/bubbleagent/agent/tx.py
def do_func(self, token, to):
    return json.dumps({
        "status": "successful",
        "transaction hash": "0x17076a6f0e3dc0fcedfefb7a9c410261cf24cefb3cdf588c47733c103a72533f",
    })
```

This function returns the exact same hash every time regardless of input. No blockchain transaction is executed.

### How To Verify

```bash
git clone https://github.com/DeAgentAI/deagent-alpha
cat agents/bubbleagent/agent/tx.py
```

---

## Critical Finding 5: 17 Million Users and 192 Million Interactions Are Fake

The project claims 17 million users and 192 million on chain interactions. The smart contracts tell a different story.

| Claimed | Actual |
|---------|--------|
| 17 Million Users | Contract nonce is 1 |
| 192 Million Interactions | 0 events on chain |

The BSC contract has only one transaction which is the deployment. Zero user interactions exist.

### How To Verify

```bash
curl -X POST "https://bsc-dataseed1.binance.org" \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_getLogs","params":[{"address":"0xA07F71451eD702669E9e08d97BAd2124777eD612","fromBlock":"0x0","toBlock":"latest"}],"id":1}'
```

Returns empty array. Zero events.

---

## Critical Finding 6: 8 AI Agents Exposed

The project advertises 8 AI agents. Here is what we found:

| Agent | Endpoint | Code Exists | Technology | Status |
|-------|----------|-------------|------------|--------|
| Chat Agent | /api/v1/chat | Yes | OpenAI/Perplexity wrapper | Offline (503) |
| Prediction Agent | /api/v1/predict | No | None | Offline (503) |
| Signal Agent | /api/v1/signal | No | None | Offline (503) |
| Token Agent | /api/v1/token | Yes | Third party API calls | Offline (503) |
| Price Agent | /api/v1/price | Yes | CoinGecko API wrapper | Offline (503) |
| Trending Agent | /api/v1/trending | Yes | DexTools API wrapper | Offline (503) |
| User Agent | /api/v1/user | No | None | Offline (503) |
| Swap Agent | /api/v1/swap | Yes | Returns fake TX hash | Offline (503) |

Summary: No proprietary AI technology exists. Working agents are simple API wrappers. All endpoints are offline.

### How To Verify

```bash
curl -s -o /dev/null -w "%{http_code}" https://deagent.ai/api/v1/chat
curl -s -o /dev/null -w "%{http_code}" https://demo.deagent.ai
```

---

## Critical Finding 7: Ella Chatbot Code Theft

Additional plagiarism from a project called Ella:

```python
# File: Agent3/chat.py line 31
bot_response = output["result"].replace("Ella: ", "")

# File: Agent3/chat.py line 39
print("Ella: Welcome! What are we exploring today?")
```

---

## Development Team: Chinese Origin Evidence

50+ files contain Chinese language comments:

```python
# File: agents/bubbleagent/assistant/chat.py
# 处理异步函数
# 处理异步生成器函数

# File: agents/bubbleagent/db/symbol_info.py
# 连接在池中可以保持空闲的最大秒数

# File: agents/bubbleagent/__main__.py
# 我想知道最新btc 的价格
```

```typescript
// Frontend files
// 请添加组件描述
// 将swap组件打开
// 缓存最近的一次uid
```

Production staking interface displays Chinese text:

```
stake.deagent.ai shows: "质押活动已结束"
```

### How To Verify

```bash
git clone https://github.com/DeAgentAI/deagent-alpha
grep -rP "[\x{4e00}-\x{9fff}]" .
```

---

## Risk Assessment

| Category | Score |
|----------|-------|
| Technical Claims vs Reality | 0/25 |
| Code Originality | 0/25 |
| Infrastructure Status | 0/25 |
| Smart Contract Usage | 0/25 |
| Security Practices | 9/25 |

**Total Score: 9/100**

---

## Conclusion

DeAgentAI is a project built on lies. The whitepaper describes zkML and MPC and Lobe Consensus and proprietary AI. None of these exist in the code. The codebase is stolen from BubbleAI with the original branding still visible. The ElizaOS repository is a pure fork with zero original contributions. The transaction module returns fake hashes. The claimed 17 million users and 192 million interactions are fabricated as the contracts show zero activity. 8 AI agents are advertised but none contain proprietary technology and all endpoints are offline. The development team appears to be based in China based on extensive Chinese comments in the code.

---

**Report Generated:** April 19 2026
**Auditor:** MEFAI Security Research
**Methodology:** Static code analysis and on chain verification
