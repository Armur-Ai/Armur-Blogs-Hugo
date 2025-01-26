---
title: "Massive Chrome Extension Breach Exposes Millions to Data Theft"
date: 2025-01-25T15:20:22+05:30
image: "https://i.ibb.co/k8tfQCH/Bzyvtd-P-famous-paintings-wallpaper.jpg"
tags: ["chrome", "extension", "breach"]
---

A widespread cyberattack has compromised at least 35 Chrome browser extensions, putting over 2.6 million users at risk of data theft and credential exposure. This targeted campaign exploited publisher accounts on the Chrome Web Store through phishing tactics, embedding malicious code into trusted extensions to steal cookies and access tokens.


### The Attack Campaign
The breach came to light after cybersecurity firm Cyberhaven reported that one of its employees fell victim to a phishing attack on December 24. This led to the compromise of Cyberhaven’s own browser extension. The attackers leveraged access permissions to insert malicious code, enabling communication with an external command-and-control (C&C) server hosted on the domain `cyberhavenext[.]pro`. The malicious extension downloaded additional configuration files and exfiltrated sensitive user data.

The phishing emails posed as communications from Google Chrome Web Store Developer Support, urging developers to act quickly to avoid extension removal due to alleged policy violations. Recipients were redirected to a page granting permissions to a malicious OAuth application named "Privacy Policy Extension," which facilitated the attack.

Cyberhaven disclosed that this malicious version of their extension passed Google’s Chrome Web Store security review and was approved for publication.

### Impact of the Attack

“Browser extensions are the soft underbelly of web security,” stated Or Eshed, CEO of LayerX Security. Many extensions, often overlooked in endpoint security protocols, possess extensive permissions to access sensitive user information such as cookies, access tokens, and identity data.

Jamie Blasco, CTO of Nudge Security, identified additional domains linked to the C&C server used in the Cyberhaven attack. Investigations uncovered more compromised extensions, including:
- AI Assistant Extensions: ChatGPT and Gemini for Chrome, Bard AI Chat Extension, TinaMind AI Assistant
- VPNs: VPNCity, Internxt VPN
- Utility Tools: Reader Mode, Web Mirror, Proxy SwitchyOmega (V3)
- Ecommerce-Related Extensions: Earny - Up to 20% Cash Back, Rewards Search Automator

Secure Annex and Extension Total, browser extension security platforms, revealed that the campaign might date back to April 2023 or even earlier, based on domain registration data.


### Technical Insights
Analysis revealed that malicious code in the Cyberhaven extension targeted identity data and Facebook account access tokens, with a specific focus on Facebook Ads users. The code monitored user activity on Facebook.com, searching for QR codes potentially used for bypassing two-factor authentication.

John Tuckner of Secure Annex linked the code in Cyberhaven’s extension to similar scripts found in others, such as “Reader Mode” and “Rewards Search Automator,” which exfiltrated data under the guise of safe-browsing or ecommerce features.

### Developer Monetization and Potential Complicity
In some cases, the malicious data-gathering code was reportedly introduced by developers themselves via monetization SDKs. Security researcher Wladimir Palant highlighted that the developer of “Visual Effects for Google Meet” included an ad-blocking library from Urban VPN, which stealthily exfiltrated browsing data.

### Mitigation and Response
Google has removed some of the compromised extensions, including Cyberhaven’s, but experts warn that the danger persists as long as the malicious versions remain installed on user devices. Or Eshed emphasized, “Hackers can still access and exploit live compromised extensions.”
Users are advised to:
- Regularly audit installed browser extensions.
- Remove unnecessary or suspicious extensions.
- Update all extensions to their latest versions.
- Enable two-factor authentication for sensitive accounts.

Alternatively, developers who are creating these extensions can use [Armur](https://armur.ai) to secure the extensions by checking vulnerabilities as well as ensuring their extensions are safe to use.

### Conclusion
While the origin and scale of this attack remain unclear, the breach underscores the vulnerability of browser extensions and the critical need for stronger security measures. Organizations must proactively monitor extensions on their endpoints to minimize exposure.
Google has been contacted for further comments on this incident, and updates will follow as the story develops.
