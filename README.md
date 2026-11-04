# Hyperliquid Phishing Research

## Target Domains
- **us-hyperliquid.org** - Primary target, impersonates Hyperliquid
- **eng-hyperliquid.com** - Related domain from same threat actor, redirects to us-hyperliquid.org

## Threat Intelligence

### PCRisk Analysis (January 2026)
- **Threat Type**: Phishing, Scam, Social Engineering, Fraud
- **Disguise**: Hyperliquid trading platform
- **Mechanism**: Fake "Connect Wallet" button activates cryptocurrency drainer
- **Damage**: Monetary loss - cryptocurrency transactions cannot be reversed
- **Related Domains**: hyperliquidmetrics[.]xyz, hyperliquid-views[.]xyz, check.view-stats[.]xyz, app-hyperiliquild[.]org, hyperliquidbuy[.]org, hyperliquidcrypto[.]org, hyperliquides[.]org, hyperliquid[.]life
- **Serving IP** (for hyperliquid[.]life): 172.67.200.230
- **Detection**: Combo Cleaner (Malware), G-Data (Malware), VirusTotal detections

### DNS Information
```
us-hyperliquid.org:
  - 172.67.213.32 (Cloudflare)
  - 104.21.53.135 (Cloudflare)

eng-hyperliquid.com:
  - Behind Cloudflare (same IPs likely)
```

### Cloudflare Protection
Both domains are protected by Cloudflare with:
- **WAF Rules**: Blocking automated access (403 Forbidden)
- **Bot Detection**: Identifying Playwright/WebKit fingerprints
- **Ray IDs**: Rotating (e.g., a5567e9fa936d268, a5568b95f99b4d86, a55696168d3563e4)

## Technical Analysis Status

### Site Access Attempts
| Method | us-hyperliquid.org | eng-hyperliquid.com |
|--------|-------------------|---------------------|
| Direct curl | 403 Cloudflare | 403 Cloudflare |
| Playwright (WebKit) | 403 Cloudflare | 403 Cloudflare |
| textise.net | Failed to fetch | Failed to fetch |
| txtify.it | 403 Cloudflare | 403 Cloudflare |
| Google Cache | Returns Google bot detection JS | Returns Google bot detection JS |
| Bing Cache | 451 Legal Reasons | 451 Legal Reasons |
| Jina.ai | 451 Legal Reasons | 451 Legal Reasons |
| Wayback Machine | Not archived | Not archived |

### Cloudflare Bypass Research
Based on HasData/cloudflare-bypass and other sources, effective techniques include:
1. **Playwright + Stealth Plugin** (playwright-extra + playwright-extra-plugin-stealth)
2. **Residential Proxy Rotation** - Better IP reputation
3. **Header/User-Agent Spoofing** - Realistic browser headers
4. **Human-like Behavior** - Mouse movement, scrolling, timed delays
5. **Browser Choice** - Chromium/Firefox often better than WebKit for stealth

### Current Blocker
The Playwright MCP is configured with `--browser=webkit` which has distinct fingerprinting that Cloudflare easily detects. The MCP server would need to be restarted with:
- `--browser=chromium` or `--browser=firefox`
- Stealth plugin injection (not directly supported by MCP)

## Crypto Drainer Mechanism (from PCRisk)
1. Site perfectly mimics legitimate Hyperliquid design
2. Only functional element: "Connect Wallet" button
3. On wallet connection: Drainer activates
4. Some drainers estimate asset value and prioritize high-value tokens
5. Automated outgoing transactions (appear vague to lower suspicion)
6. **Irreversible**: Victims lose all/most assets; transactions cannot be reversed

## Next Steps for Research

### Immediate
1. **Reconfigure Playwright MCP** to use Chromium with stealth
2. **Try residential proxies** for better IP reputation
3. **Implement human-like interactions** (mouse, scroll, delays)
4. **Capture network traffic** to identify drainer JS files

### Analysis Phase
1. **Download and deobfuscate** the drainer JavaScript
2. **Identify wallet connection handlers** (Web3 provider injection)
3. **Trace transaction construction** (permit/permit2, approval, transfer)
4. **Map C2 infrastructure** (if any)
5. **Extract wallet addresses** receiving stolen funds

### Attribution
1. **Correlate** with known drainer kits (Angel Drainer, Inferno Drainer, etc.)
2. **Infrastructure overlap** with other Hyperliquid phishing domains
3. **Code similarity** analysis

## Repository Structure
```
hyperliquid-phishing-research/
├── README.md              # This file
├── domains/               # Domain intelligence
├── network/               # PCAPs, HAR files
├── drainer/               # Deobfuscated drainer code
├── analysis/              # Technical analysis notes
└── iocs/                  # Indicators of Compromise
```

## References
- [PCRisk: Fake Hyperliquid Trading Platform Scam](https://www.pcrisk.com/removal-guides/29580-fake-hyperliquid-trading-platform-scam)
- [HasData Cloudflare Bypass](https://github.com/HasData/cloudflare-bypass)
- [Playwright Stealth](https://github.com/berstend/playwright-extra/tree/master/packages/playwright-extra-plugin-stealth)