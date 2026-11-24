Overview

The challenge involved a Flask login application protected by IP-based rate limiting. The objective was to identify valid credentials from a provided credential list, bypass the rate restriction, and retrieve the flag.

Tools Used:
Burp Suite (Proxy & Intruder)
Source Code Analysis

<br><br>

# 1. Source Code Analysis
Reviewed app.py to understand the rate-limiting implementation.

Key configurations:
Maximum requests: 10
Request window: 30 seconds
Lockout duration: 120 seconds

The application resets its request counter after the 30-second window expires. This presented an opportunity to distribute authentication attempts over time without triggering a lockout.






<br><br>

# 2. Capturing the Login Request

Intercepted the login request using Burp Suite Proxy and sent the POST /login request to Intruder.
Screenshot: Login request intercepted in Burp Suite






<br><br>

# 3. Configuring Burp Suite Intruder

Selected Pitchfork to test corresponding username and password combinations simultaneously.

Configured two payload positions:
Position 1: Username list
Position 2: Password list

Both payload lists contained 100 entries, maintaining their original order.
Screenshot: Pitchfork attack and payload configuration





<br><br>

# 4. Bypassing the Rate Limit

Created a custom Burp Suite Resource Pool to control request timing.

Configuration:
Maximum concurrent requests: 1
Fixed delay: 3200 milliseconds
Automatic throttling: Disabled

This kept requests below the application's threshold while allowing the attack to progress.
Screenshot: Custom Resource Pool configuration




<br><br>

# 5. Identifying Valid Credentials

After launching the attack, Request 60 returned a different response from the failed login attempts.

Field

Result

Username: bethan
Password: barber

Status Code
302 Found

Response Length
491

Redirect

/

The redirect and session cookie indicated successful authentication.

Screenshot: Successful authentication in Intruder





<br><br>

# 6. Flag Retrieval

Authenticated using the discovered credentials and accessed the homepage.

Screenshot: Authenticated homepage displaying the flag



Flag:

academy{f00l_7h4t_l1m1t3r_01f813c6}





Key Takeaways

Source code review can reveal weaknesses in security controls.

Burp Suite Pitchfork is useful for testing paired credential lists.

Resource Pools allow precise control over request timing.

Fixed-window rate limiting can be susceptible to low-and-slow brute-force attacks.
