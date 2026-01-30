---

---
-------------------
CRON JOBS: 
1.  Google review fetch: every Hour fetch
2.  review auto reply: every day at 20:00:00 UTC time
3.  review delete script: every day at 15:00:00 UTC time
4. IOS review fetch: every hour ( Not running on live)
5. IOS review reply: every hour (Not running on live)
6. playstore review fetch: every hour (Not running on live)
7. playstore review auto-reply: every hour (Not running on live)
8. sentiment analysis of review: every hour (turned off on the live environment) right now it is not active discuss with utkarsh sir. 
---------------
Reply Model Used: 
1. for auto replys we use chatgpt's "gpt-4o" model  if our prompt is around 50-60 lines then it will cost $0.0175/INR ~1.45 per call
----
1. Refresh token and all the confidential things are stored encrypted in the database so make sure you decrypt them before you use. 