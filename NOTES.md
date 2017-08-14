# Project Name - MM-DD-YYYY

## Understanding

Document your understanding of the software's purpose, benefits, and concerns.

## Threat Modeling

Document a simple text DFD to get a sense of how components and sub-components
interaction with one another.

ComponentA - TLS -> ComponentB
  |_ TLS connection mututally authN'd
  |_ Long-lived TLS connection

ComponentB - TLS -> ComponentA
  |_ JWT used for sessions

ComponentB --> MySQL
  |_ MySQL connection authN'd by username/password

## Threat Enumeration

Enumerate threats that could affect the software.

* TH1: RefXSS, StoredXSS, DOMXSS, Content Injection

* TH2: CSRF, XSSI

* TH3: SSRF, Proxy Header tampering, RFI

* TH4: SQLi, 2nd order SQLi

* TH5: Server-Side Template Injection

* TH6: Dangerous File IO, LFI

* TH7: Authentication failures, lack of authentication, weak authentication, poor error handling

* TH8: Authorization failures, errors in access controls and role definitions, poor error handling

* TH9: Race conditions and inconsistent transactions

* TH10: LDAP injection

* TH11: User-supplied Serialization

## Test Case Enumeration and Prioritization

| Testcase ID | Status | Threat ID | Priority | Test Case      |
|-------------|--------|-----------|----------|----------------|
| TC1         | TODO   | TH1       | High     | SqlMap         |
| TC2         | TODO   | TH2       | Medium   | TplMap         |
| TC3         | TODO   | TH2       | Low      | Burp AS        |
| TC4         | TODO   | TH2       | Low      | Manual testing |

##  Test Case Execution

### TC1: Run SQLmap against ComponentB

I ran it for the following endpoints:

* `/a`

* `/b`

The command:
```
$ sqlmap -u compa.com?q=tst
```
