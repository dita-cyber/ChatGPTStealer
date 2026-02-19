# ChatGPTStealer

I’ve worked multiple alerts related to ChatGPT Stealer activity tied to malicious Chrome extensions. This repository documents my experience handling these alerts from a SOC perspective and summarizes the technical findings observed during investigation.

![image alt](https://github.com/dita-cyber/ChatGPTStealer/blob/294930bfd5ada5d3110fdc774ea00ce697687097/image3.png)

Over the past few weeks, I found myself working through a significant volume of alerts tied to malicious Chrome extensions harvesting ChatGPT and DeepSeek conversations. This technique is named Prompt Poaching. What started as a spike in browser-related detections quickly turned into one of the more eye-opening investigations I've worked recently.
What made this one stand out wasn't just the scale over 900,000 users affected across two extensions alone, but the new topic around AI and its vulnerabilities attached to browser extensions. 

Cybersecurity researchers have identified at least two malicious Chrome extensions with a combined install base of over 900,000 users that are silently exfiltrating ChatGPT and DeepSeek conversation data to attacker-controlled servers. The technique, codenamed Prompt Poaching, represents an emerging and highly scalable method of harvesting sensitive AI chatbot interactions through weaponized browser add-ons.
 
The two confirmed malicious extensions identified are:
- Chat GPT for Chrome with GPT-5, Claude Sonnet & DeepSeek AI. Extension ID: fnmihdojmnkclgjpcoonokmkhjpjechg  
- AI Sidebar with Deepseek, ChatGPT, Claude, and more. Extension ID: inhcgfpbfdjbjogdfjbclgolkmhnooop 

Both extensions impersonate a legitimate tool named "Chat with all AI models (Gemini, Claude, DeepSeek...) & AI Agents" published by AITOPIA, which has approximately one million genuine users. The "Chat GPT for Chrome with GPT-5" extension has since had its Chrome Web Store "Featured" badge revoked, however it remained available for installation at time of writing.

How the Attack Works
Upon installation, the extensions request permission to collect what they describe as "anonymous, non-identifiable analytics data" to improve the user experience. Once this consent is granted, the malware activates its exfiltration routine. The extensions scrape DOM elements inside ChatGPT and DeepSeek web sessions to extract full conversation content, then bundle this data alongside all open browser tab URLs and transmit the package to a remote command-and-control (C2) server at 30-minute intervals. Identified C2 infrastructure includes the domains chatsaigpt[.]com and deepaichats[.]com. Additionally, the threat actors used Lovable, an AI-powered web development platform, to host their phishing-style privacy pages and supporting infrastructure at chataigpt[.]pro and chatgptsidebar[.]pro, further obscuring the malicious operation.

Organizations whose employees have these extensions installed may have unknowingly exposed: full AI chatbot conversation histories including internal project discussions, intellectual property and trade secrets shared with AI assistants, internal corporate URLs visible in browser tab metadata, and sensitive data such as personally identifiable information (PII) or customer records entered into AI tools. This data can be weaponized for corporate espionage, targeted spear-phishing, identity theft, or sold on criminal marketplaces.

IOCs
Extension ID: fnmihdojmnkclgjpcoonokmkhjpjechg
Extension ID: inhcgfpbfdjbjogdfjbclgolkmhnooop
chatsaigpt[.]com
deepaichats[.]com
chataigpt[.]pro
chatgptsidebar[.]pro

Recommendations

- Audit installed Chrome and Edge extensions across all managed endpoints.
- Remove the two identified malicious extension IDs. Open the browser, click the three-dot menu (top-right), select "Extensions" > "Manage Extensions," and click "Remove" on the desired item
- Block the identified C2 and infrastructure domains. Add chatsaigpt[.]com, deepaichats[.]com, chataigpt[.]pro, and chatgptsidebar[.]pro to your blocklist and perimeter firewall deny list.
- Alert affected users and request self-remediation. Notify any employees found to have the extensions installed. Instruct them to remove the extension immediately and clear browser cookies and session data as a precaution. Consider requiring a password reset for any accounts accessed during the exposure window.

I also created a PDF sample template for a security advisory related to this alerts:

(https://github.com/dita-cyber/ChatGPTStealer/blob/main/SOC_Advisory_PromptPoaching_2026.docx.pdf)
