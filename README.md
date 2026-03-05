# JB Service Documentation

## Accounts and Authentication
...
## Security
...

### Rate Limiting
...
### IP Blacklisting
...
### CAPTCHA

#### Overview
A CAPTCHA test is designed to distinguish whether a user is human or a bot. A CAPTCHA test can range from a tick the box to typing characters you see in the image. These tests are an effective yet simple prevention against malicious bot actions such as spamming and brute-forcing.

Example CAPTCHA illustration:
![handmade captcha test illustration](/cache illustration.png "handmade captcha test illustration")

Although its security benefits, CAPTCHA tests are not implemented at every point of interaction with the web to avoid interrupting user flow. Hence, these tests have trigger conditions which automatically start them. Our service has 5 trigger conditions which are explained in detail in the following section.

![handmade trigger conditions diagram](/trigger diagram.png "handmade trigger conditions diagram")

Note:
The numbering of conditions in the diagram reflects the order of explanations in this document. All conditions are evaluated independently and not in a sequential order.

#### Trigger Conditions
Our service shows a CAPTCHA if any of these 5 scenarios occur:


1. High request volume from the same IP address

Condition:
More than 500 requests from the same IP address in less than 20 minutes.

Purpose:
Prevents automated attacks from the same device.
Prevents Brute Force and automated attacks from the same device.

Example Scenarios:


Update thresholds:
The threshold for both the maximum amount of requests and the timespan can be updated.
See: config/capctha/rate-limit.yaml


2. Request from IP address in blacklist

Condition:
If the IP address from which the request is received is found in the blacklist.

Purpose:
Blocks requests from known and identified attackers, flagged sources, spam etc.

Example Scenarios:

Update Blacklist:
Blacklist is managed in: Admin Panel -> Security -> IP Blacklist where IP addresses can be added and removed.

3. Abnormally high traffic

Condition:
If within the current hour more than double the average amount of requests are made for that hour compared to the last 2 weeks.

Purpose:
Detects unusual traffic peaks caused by multiple devices.

Example Scenarios:

Update:

4. Repeated Payloads
Condition:
If the same payload has been sent more than 5 times in the last 30 seconds.

Purpose:

Example Scenarios:
// Example payload structure
{
"email": "...",
"comment": "...",
"date": "..."
}


Update thresholds:
The threshold for both the maximum amount of requests and the timespan can be updated.
See: config/capctha/rate-limit.yaml

5. Admin Enabled

Condition:
If the CAPTCHA has been enabled manually via the admin panel for certain requests.

Purpose:

Example Scenarios:

Update Selection of Request:
To manage which request should trigger the CAPTCHA: 
Security -> Admin Panel -> -> CAPTCHA Triggers

#### Troubleshooting and FAQ
* CAPTCHA not appearing for testing

CAPTCHA does not appear if the 5 conditions in the “Trigger Conditions” (add link) are not met. The easiest way to test CAPTCHA behaviour internally would be to enable it via Admin Panel.
See: ashjfskdfs

Other testing methods include:
Simulating high request from the same IP:  high_requests.py
Simulating repeated payloads: repeated_requests.py (link to readme.md)

* How to check which condition triggered the CAPTCHA

All triggering conditions are logged temporarily (24 hours) in service request logs.

Example log entry
Create one!!!

* Why are real users flagged as suspicious (not the best wording maybe)?

Some situations may trigger CAPTCHA for legitimate users. These situations include using shared IP networks, automated browser extensions, and repeated submissions in a short time. (fact check this)

#### Future Improvements
As technologies such as AI continue to improve, traditional CAPTCHA tests become less of a challenge for bots to solve. To maintain long term protection, the team is evaluating additional security mechanisms to accompany or replace CAPCTHA.



Machine Learning Based Behaviour Detection
Lightweight ML models that analyze request patterns and timing is being explored to differentiate between real users and bots
See: (placeholder link to project)

Optional 2 Factor Verification for Account Creation
The JetBrains service may introduce phone number verification or another form of 2FA during account creation to reduce account base abuse across the service.
See: (placeholder link to project)

Proof of Work Puzzle
A potential alternative to traditional CAPTCHA is a lightweight client side PoW challenge, where the browser performs a small computational task before a request proceeds. This increases the cost of automated attacks without impacting typical users.
See: (placeholder link to project)

## Admin Panel
...

