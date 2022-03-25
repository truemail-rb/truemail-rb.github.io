# Architecture

Truemail is a multi-layered email validator/verifier with configurable behavior for specified domain names/mx server ip addresses. So you can validate only what you need.

```mermaid
flowchart LR
A(Whitelist/Blacklist) --> B(Regex validation)
B(Regex validation) --> C(MX validation)
C(MX validation) --> D(MX blacklist validation)
D(MX blacklist validation) --> E(SMTP validation)
```

## DNS validation flow

```mermaid
flowchart TD
  Y[Domain name from email address] --> A(MX records resolver)
  A(MX records resolver) -->|False| B(CNAME records resolver)
  B(CNAME records resolver) -->|False| D(A records resolver)
  D(A records resolver) -->|False| E[Validation fails]
  A(MX records resolver) -->|not RFC MX lookup flow| E[Validation fails]
  A(MX records resolver) -->|True| C[Validation successful]
  B(CNAME records resolver) -->|True| C[Validation successful]
  D(A records resolver) -->|True| C[Validation successful]
  
  style A stroke:blue;
  style B stroke:blue;
  style D stroke:blue;
  style C color:green;
  style E color:red;
```

## MX records resolver

```mermaid
flowchart TD
  B(MX records resolver) --> C{Doesn't Null MX exist? RFC 7505}
  C{Doesn't Null MX exist? RFC 7505} -->|False| D[Validation fails]
  C{Doesn't Null MX exist? RFC 7505} -->|True| E{MX records exist? RFC 5321}
  E{MX records exist? RFC 5321} -->|False| F(CNAME records resolver)
  E{MX records exist? RFC 5321} -->|True| Z[Validation successful]
  
  click C href "https://tools.ietf.org/html/rfc7505"
  click E href "https://tools.ietf.org/html/rfc5321#section-5"

  style B stroke:blue;
  style F stroke:blue;
  style D color:red;
  style Z color:green;
```

## CNAME records resolver

```mermaid
flowchart TD
  A(CNAME records resolver) --> B{Do CNAME records exist? RFC 5321}
  B{Do CNAME records exist? RFC 5321} -->|False| C[A record resolver]
  B{Do CNAME records exist? RFC 5321} -->|True| D[Last CNAME host from each CNAME record]
  D[Last CNAME host from each CNAME record] --> E(MX records resolver)
  E(MX records resolver) -->|False| C(A record resolver)
  E(MX records resolver) -->|True| Z[Validation successful]
  
  click B href "https://tools.ietf.org/html/rfc5321#section-5"

  style A stroke:blue;
  style C stroke:blue;
  style E stroke:blue;
  style Z color:green;
```

## A records resolver

```mermaid
flowchart TD
  A(A records resolver) --> B{Do A records exist?}
  B{Do A records exist?} -->|False| C[Validation fails]
  B{Do A records exist?} -->|True| D[Host from first A record RFC 5321]
  D[Host from first A record RFC 5321] -->|True| Z[Validation successful]
  
  click D href "https://tools.ietf.org/html/rfc5321#section-5"

  style A stroke:blue;
  style C color:red;
  style Z color:green;
```

## SMTP validation flow

```mermaid
flowchart TD  
  A[Mail server] --> B{Is the 25-port opened?}
  B{Is the 25-port opened?} -->|False| D{Next mail-server exists?}
  D{Next mail-server exists?} -->|False| Y[Validation fails]
  D{Next mail-server exists?} -->|True| A[Mail server]
  B{Is the 25-port opened?} -->|True| C(Run SMTP-session)
  C(Run SMTP-session) -->|False| D{Next mail-server exists?}
  C(Run SMTP-session) -->|True| Z[Validation successful]
  
  style Y color:red;
  style Z color:green;
```

## SMTP-session

```mermaid
flowchart LR  
  A(Open session) -->|True| B(HELO/EHLO)
  B(HELO/EHLO) -->|True| C(MAILFROM)
  C(MAILFROM) -->|True| D(RCPTTO)
  D(RCPTTO) -->|True| Z[Validation successful]
  A(Open session) -->|False| Y[Validation fails]
  B(HELO/EHLO) -->|False| Y[Validation fails]
  C(MAILFROM) -->|False| Y[Validation fails]
  D(RCPTTO) -->|False| Y[Validation fails]
  
  style Y color:red;
  style Z color:green;
```
