# API Security Risk Analysis - Task 3

## Task Information

| Field | Value |
|-------|-------|
| **Task** | Task 3 - API Security Risk Analysis |
| **Track** | Cyber Security (CS) |
| **Intern Name** | Ariya709 |
| **CIN ID** | FIT/MAY26/CS8319 |
| **Date** | May 23, 2026 |

---

## Target API

| Field | Value |
|-------|-------|
| **API Name** | JSONPlaceholder |
| **Base URL** | https://jsonplaceholder.typicode.com |
| **Type** | Fake REST API for testing |
| **Endpoints Tested** | /posts, /users |

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Postman | API request testing |
| Browser DevTools | Request/response inspection |
| Canva | Report design |

---

## Findings Summary

| # | Vulnerability | Endpoint | Risk Level |
|---|---------------|----------|------------|
| 1 | No Authentication | All endpoints | 🔴 HIGH |
| 2 | No Rate Limiting | All endpoints | 🟡 MEDIUM |
| 3 | IDOR Vulnerability | GET /posts/{id} | 🟡 MEDIUM |
| 4 | Data Exposure (PII) | GET /users/{id} | 🟡 MEDIUM |
| 5 | HTTP Allowed (No HTTPS Enforcement) | All endpoints | 🟡 MEDIUM |

**Total Vulnerabilities Found: 5**

---

## Detailed Findings

### Finding #1: No Authentication (HIGH)

| Field | Value |
|-------|-------|
| **Endpoint** | POST /posts |
| **Issue** | API accepts requests without API keys, tokens, or login |
| **Evidence** | POST request succeeded with 201 Created, no Authorization header |
| **Impact** | Anyone can create, update, or delete data |
| **Fix** | Implement OAuth 2.0 or API key authentication |

---

### Finding #2: No Rate Limiting (MEDIUM)

| Field | Value |
|-------|-------|
| **Endpoint** | GET /users |
| **Issue** | API accepts unlimited rapid requests |
| **Evidence** | 20+ rapid requests all returned 200 OK, no 429 error |
| **Impact** | Denial of Service (DoS) attacks possible |
| **Fix** | Implement rate limiting (e.g., 100 requests/minute) |

---

### Finding #3: IDOR Vulnerability (MEDIUM)

| Field | Value |
|-------|-------|
| **Endpoint** | GET /posts/{id} |
| **Issue** | Can access any post by changing ID in URL |
| **Evidence** | /posts/1, /posts/2, /posts/3 all returned different users' posts |
| **Impact** | Users can access other users' private data |
| **Fix** | Implement proper access controls per user |

---

### Finding #4: Data Exposure - PII (MEDIUM)

| Field | Value |
|-------|-------|
| **Endpoint** | GET /users/{id} |
| **Issue** | API returns sensitive personal information |
| **Evidence** | Response shows email, phone number, and full address |
| **Impact** | Privacy violation, phishing attacks, identity theft |
| **Fix** | Mask PII data, implement authentication, use data filtering |

---

### Finding #5: HTTP Allowed (MEDIUM)

| Field | Value |
|-------|-------|
| **Endpoint** | All endpoints |
| **Issue** | API accepts insecure HTTP connections |
| **Evidence** | http:// URL worked the same as https:// |
| **Impact** | Man-in-the-middle (MITM) attacks possible |
| **Fix** | Redirect all HTTP traffic to HTTPS, enable HSTS |

---

## Remediation Priority Table

| Priority | Vulnerability | Timeline | Responsible |
|----------|---------------|----------|-------------|
| 🔴 HIGH | No Authentication | 1 week | Dev Team |
| 🟡 MEDIUM | No Rate Limiting | 2 weeks | DevOps |
| 🟡 MEDIUM | IDOR Vulnerability | 2 weeks | Dev Team |
| 🟡 MEDIUM | PII Data Exposure | 2 weeks | Dev Team |
| 🟡 MEDIUM | HTTP Allowed | 1 week | DevOps |

---

## Screenshots

| # | Screenshot | Description |
|---|------------|-------------|
| 1 | [01-get-request.png](screenshots/01-get-request.png) | GET /posts - 200 OK response |
| 2 | [02-idor-test.png](screenshots/02-idor-test.png) | IDOR - accessing /posts/2, /posts/3 |
| 3 | [03-no-auth.png](screenshots/03-no-auth.png) | POST request without authentication - 201 Created |
| 4 | [04-pii-exposure.png](screenshots/04-pii-exposure.png) | PII exposure - email and phone visible |
| 5 | [05-rate-limit.png](screenshots/05-rate-limit.png) | No rate limiting - multiple rapid requests |
| 6 | [06-http-allowed.png](screenshots/06-http-allowed.png) | HTTP allowed - no HTTPS enforcement |

---

## Evidence Folder

All screenshots and analysis materials are available in the  
[/screenshots folder]((Evidence Folder 3))

---

## Final Report

[View API Security Risk Analysis Report (Canva)](https://canva.link/uhn4oywcekowmtl)

---

## Conclusion

The JSONPlaceholder API has multiple security vulnerabilities:

| Severity | Count |
|----------|-------|
| 🔴 HIGH | 1 |
| 🟡 MEDIUM | 4 |
| 🔵 LOW | 0 |

**Key Recommendations:**
1. Implement authentication immediately (HIGH priority)
2. Enable rate limiting to prevent DoS attacks
3. Add access controls to fix IDOR vulnerabilities
4. Mask PII data in API responses
5. Force HTTPS and enable HSTS

**This API should NOT be used in production without implementing these security controls.**
