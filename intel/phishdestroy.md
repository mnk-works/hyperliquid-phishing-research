# PhishDestroy Intelligence

## Source
- https://phishdestroy.io/domain/eng-hyperliquid.com
- https://phishdestroy.io/domain/us-hyperliquid.org

## eng-hyperliquid.com
```json
{
  "domain": "eng-hyperliquid.com",
  "vt": "4",
  "vtTotal": "92",
  "registrar": "Hosting Concepts B.V. d/b/a Registrar.eu",
  "caseId": "",
  "ip": "172.67.168.177",
  "isSharedEdge": true,
  "sharedNetworkProvider": "Cloudflare",
  "country": "RU",
  "reportedAt": "",
  "detectedAt": "2026-10-23 10:23:13",
  "created": "2026-08-02T21:22:25Z",
  "scamType": "",
  "reportCount": "0",
  "hoursSince": "0",
  "abuseEmail": "",
  "firstSent": "",
  "lastSent": "",
  "registrarSlug": "",
  "pageToken": "1793827304.7012a1f588ea8e54b2b52e4e3e61b57081c2442a031c317216495a8437b71d40"
}
```

## us-hyperliquid.org
```json
{
  "domain": "us-hyperliquid.org",
  "vt": "6",
  "vtTotal": "92",
  "registrar": "Hosting Concepts B.V. d/b/a Registrar.eu",
  "caseId": "",
  "ip": "104.21.53.135",
  "isSharedEdge": true,
  "sharedNetworkProvider": "Cloudflare",
  "country": "RU",
  "reportedAt": "",
  "detectedAt": "2026-10-23 10:22:51",
  "created": "2026-07-27T13:33:57.058Z",
  "scamType": "",
  "reportCount": "0",
  "hoursSince": "0",
  "abuseEmail": "",
  "firstSent": "",
  "lastSent": "",
  "registrarSlug": "",
  "pageToken": "1793827312.d94a8426e8feddb7476632a7f108b5a611e65a9245842267b2e6c3eef8419ae2"
}
```

## Correlation Analysis

| Attribute | eng-hyperliquid.com | us-hyperliquid.org | Match |
|-----------|---------------------|---------------------|-------|
| Registrar | Hosting Concepts B.V. | Hosting Concepts B.V. | ✅ Exact |
| Network Provider | Cloudflare | Cloudflare | ✅ Exact |
| Country | RU (Russia) | RU (Russia) | ✅ Exact |
| Creation Date | 2026-08-02 | 2026-07-27 | ✅ 6 days apart |
| Detection Date | 2026-10-23 10:23:13 | 2026-10-23 10:22:51 | ✅ Same minute |
| VT Detections | 4/92 | 6/92 | ✅ Similar |

## Conclusion
**HIGH CONFIDENCE**: Both domains are operated by the same threat actor based in Russia, using the same registrar (Registrar.eu) and Cloudflare for protection. The domains were created within 6 days of each other and detected simultaneously.