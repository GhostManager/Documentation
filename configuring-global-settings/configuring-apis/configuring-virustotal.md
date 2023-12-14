---
description: Enabling the VirusTotal API
---

# Configuring VirusTotal

Ghostwriter can use VirusTotal's API to look up domain names in your Domain Library. If any domain name is linked to positive malware downloads or assigned an undesirable category, Ghostwriter will mark that domain as "Burned" with an explanation.

The blocklist is maintained inside of the _review.py_ module:&#x20;

```
class DomainReview(object

    « snip »

    blocklist = [
        "phishing",
        "web ads/analytics",
        "suspicious",
        "placeholders",
        "pornography",
        "spam",
        "gambling",
        "scam/questionable/illegal",
        "malicious sources/malnets",
    ]
```

By default, Ghostwriter will block users from checking out and using domain names marked as burned. You can always review the status change and override it (change the status back to "Healthy") if you feel the domain is still safe to use.

If you do not have one, get [a free API key from VirusTotal](https://developers.virustotal.com/reference). The free version (VirusTotal Community / Public API) is limited to 500 requests per day and 4 requests per minute. By default, Ghostwriter sleeps for 20 seconds between requests for 3 requests per minute.

If your organization has a premium API key (aka Premium API or Private API), you can change the sleep time to match the key's configured request rate.

Some older API keys may have more restrictive quotas. You can check your key's quotas by visiting _https://virustotal.com/_, logging in with the account linked to the key, clicking your profile, and clicking "API Key" from the menu.

![Quotas for a Free VirusTotal API Key](<../../.gitbook/assets/image (51).png>)

Under the VirusTotal Configuration section, check the _Enable_ checkbox and provide your API key. Only change the _Sleep Time_ value if your rate limit allows for faster requests.

![VirusTotal Configuration](<../../.gitbook/assets/image (30).png>)
