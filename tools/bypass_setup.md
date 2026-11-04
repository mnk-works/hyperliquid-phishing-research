# Technical Access Blockers & Recommendations

## Current Status: BLOCKED

### Cloudflare Protection Analysis
Both domains (`us-hyperliquid.org` and `eng-hyperliquid.com`) are protected by Cloudflare with:
- **WAF Rules**: Immediate 403 Forbidden on automated browser detection
- **Bot Management**: Fingerprinting WebKit/Playwright signatures
- **No JavaScript Challenge**: Hard block, not a challenge page
- **Ray IDs**: Rotating per request (e.g., a5567e9fa936d268, a5568b95f99b4d86, a55696168d3563a90)

### Playwright MCP Limitation
Current configuration: `--browser=webkit` (headless)
- WebKit has distinct fingerprint easily detected by Cloudflare Bot Management
- No stealth plugin injection capability in MCP
- No proxy support configured
- No header/user-agent customization in MCP

## Bypass Requirements

### Minimum Viable Setup
```bash
# Node.js with stealth
npm install playwright playwright-extra playwright-extra-plugin-stealth

# Python with stealth
pip install playwright playwright-stealth
playwright install chromium
```

### Recommended Configuration
```javascript
const { chromium } = require('playwright-extra');
const stealth = require('playwright-extra-plugin-stealth')();
chromium.use(stealth);

const browser = await chromium.launch({
  headless: true,
  args: [
    '--disable-blink-features=AutomationControlled',
    '--disable-dev-shm-usage',
    '--no-sandbox'
  ]
});

const context = await browser.newContext({
  userAgent: 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.0.0 Safari/537.36',
  viewport: { width: 1920, height: 1080 },
  locale: 'en-US',
  timezoneId: 'America/New_York',
  // Proxy: { server: 'http://user:pass@residential-proxy:8080' } // Recommended
});

const page = await context.newPage();
// Human-like behavior
await page.mouse.move(100, 100);
await page.waitForTimeout(1000);
await page.evaluate(() => window.scrollBy(0, 100));
await page.waitForTimeout(500);

await page.goto('https://eng-hyperliquid.com/', { 
  waitUntil: 'networkidle',
  timeout: 60000 
});
```

## Alternative Access Methods

### 1. Residential Proxies (Highly Recommended)
- Cloudflare uses IP reputation heavily
- Datacenter IPs (like current) are heavily flagged
- Providers: Bright Data, Oxylabs, Smartproxy, IPRoyal

### 2. Browserless / Browserbase / Scraping APIs
- Managed browsers with stealth built-in
- Handle Cloudflare challenges automatically
- Examples: Browserless.io, Browserbase, Scrapfly, ZenRows

### 3. Google Cache / Bing Cache / Wayback
- Already attempted: All return 403, 451, or bot detection JS
- Not viable for this target

### 4. Text Extraction Services
- textise.net: Failed to fetch (Cloudflare blocks their IPs too)
- txtify.it: 403 Cloudflare
- jina.ai: 451 Legal Reasons
- r.jina.ai: 451 Legal Reasons

## Expected Page Structure (from PCRisk)
Based on PCRisk analysis of similar Hyperliquid phishing sites:
- **Visual**: Pixel-perfect clone of app.hyperliquid.xyz
- **Functional**: Only "Connect Wallet" button works
- **Drainer Activation**: On wallet connection (MetaMask, WalletConnect, etc.)
- **Tech Stack**: Likely uses common drainer template (per Confiant)
- **Wallets Targeted**: MetaMask, Coinbase Wallet, WalletConnect, Trust Wallet, etc.

## Next Steps for Drainer Analysis

### Once Access Achieved:
1. **Capture Network Traffic** (HAR file)
   - Identify all JS files loaded
   - Find wallet connection handlers
   - Trace Web3 provider injection

2. **Extract & Deobfuscate JavaScript**
   - Main drainer logic likely in single bundled file
   - Look for: `ethers.js`, `web3.js`, or custom wallet interaction code
   - Identify: `eth_requestAccounts`, `wallet_addEthereumChain`, `eth_sendTransaction`

3. **Analyze Drainer Logic**
   - Permit/ERC2612 signatures (gasless approvals)
   - Direct `transfer`/`transferFrom` calls
   - `setApprovalForAll` for NFTs
   - Multicall batching for efficiency

4. **Extract IOCs**
   - Receiving wallet addresses (ETH, BSC, Polygon, etc.)
   - C2 endpoints (if any)
   - Telegram/Discord webhook URLs
   - Unique strings for YARA rules

5. **Attribution**
   - Compare with Confiant drainer template
   - Check for Angel Drainer / Inferno Drainer / MS Drainer signatures
   - Infrastructure overlap with other Hyperliquid phishing domains

## Repository Structure for Analysis
```
hyperliquid-phishing-research/
├── README.md
├── domains/
│   ├── us-hyperliquid.org.json
│   └── eng-hyperliquid.com.json
├── intel/
│   ├── phishdestroy.md
│   └── pcrisk.md
├── iocs/
│   ├── confiant_drainer_domains.txt
│   ├── confiant_drainer_eth_addresses.txt
│   └── hyperliquid_specific_iocs.md (to be created)
├── network/          # HAR files, PCAPs (to be captured)
├── drainer/          # Deobfuscated JS (to be extracted)
├── analysis/         # Technical notes (to be created)
└── tools/
    └── bypass_setup.md  # This file
```

## Contact for Collaboration
If you have residential proxy access or managed browser infrastructure, the drainer code can be extracted and analyzed. The infrastructure is confirmed to be Russian-operated (Registrar.eu, RU country code, simultaneous deployment).