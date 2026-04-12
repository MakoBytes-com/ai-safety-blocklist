# AI Agent Safety Blocklist

A curated list of domains where AI agents should require explicit human approval before performing interactive actions.

**Maintained by [MakoBytes.com](https://makobytes.com)** — the team behind [LengLeng.ai](https://lengleng.ai)

## The Problem

AI desktop assistants and browser automation agents can now click buttons, fill forms, and navigate the web autonomously. Without guardrails, a prompt injection attack or LLM hallucination could:

- Transfer money from your bank account
- Submit a tax return with wrong information
- Change your password on a critical service
- Approve OAuth permissions on your behalf
- Expose medical records to a cloud AI provider

No one had built a standardized blocklist for this because **AI desktop agents barely existed until now.**

## The Solution

`ai-safety-blocklist.json` contains 200+ domains organized into 15 categories:

| Category | Count | Risk |
|----------|-------|------|
| Banking | 28 | Unauthorized transfers |
| Payments | 10 | Fund theft |
| Investments | 16 | Unauthorized trades |
| Crypto | 11 | Irreversible transfers |
| Medical | 11 | HIPAA violation |
| Pharmacy | 8 | Prescription exposure |
| Insurance | 17 | Policy manipulation |
| Government | 16 | Identity theft |
| Tax | 6 | Financial fraud |
| Identity/SSO | 12 | Account takeover |
| Password Managers | 6 | Total credential loss |
| Email | 6 | Recovery abuse |
| Cloud Admin | 8 | Infrastructure destruction |
| Social Admin | 6 | Account lockout |
| URL Patterns | 14 | Catch-all for any domain |

## How to Use

### For AI Agent Developers

```python
import json

with open('ai-safety-blocklist.json') as f:
    blocklist = json.load(f)

def is_sensitive(url):
    url_lower = url.lower()
    for category in blocklist['categories'].values():
        domains = category.get('domains', []) + category.get('patterns', [])
        for domain in domains:
            if domain in url_lower:
                return True
    return False

# Before any click/type/submit action:
if is_sensitive(current_url):
    require_human_approval()
```

### For C# / .NET

```csharp
// Substring match against the full URL
public static bool IsSensitive(string url)
{
    var lower = url.ToLowerInvariant();
    return SensitiveDomains.Any(d => lower.Contains(d));
}
```

### For JavaScript / TypeScript

```typescript
const blocklist = require('./ai-safety-blocklist.json');

function isSensitive(url: string): boolean {
  const lower = url.toLowerCase();
  return Object.values(blocklist.categories).some((cat: any) => {
    const domains = [...(cat.domains || []), ...(cat.patterns || [])];
    return domains.some(d => lower.includes(d));
  });
}
```

## Rules for AI Agents

1. **Read-only actions** (navigate, read page, screenshot) — allowed on all sites
2. **Write actions** (click, type, submit) on blocklisted sites — **always require human approval per action**
3. **Session-wide approval** should NOT apply to blocklisted sites
4. **Screen capture / monitoring** should skip screenshots when blocklisted sites are in the foreground
5. **Clipboard monitoring** should skip content copied from blocklisted sites

## Contributing

Found a missing domain? Open a PR. Categories we want to expand:

- International banks (UK, EU, Asia, Latin America)
- Country-specific government portals
- Regional healthcare systems
- Enterprise HR/payroll platforms (Workday, ADP, Gusto)

### Rules for additions:
- Must be a real, active service with user accounts
- Must handle sensitive data (financial, medical, identity, legal)
- No duplicates — check existing entries first
- Include the correct category

## License

MIT License. Use this in any AI agent, commercial or open source. Attribution appreciated but not required.

## About

Built by the team at [MakoBytes.com](https://makobytes.com) while developing [LengLeng](https://lengleng.ai), a personal AI voice assistant for Windows. We needed this list for our own product and couldn't find one anywhere, so we built it and open-sourced it.

If you're building an AI agent that interacts with the web, you need this list. Your users' bank accounts will thank you.
