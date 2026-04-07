1. Asymmetric Encryption and GPG

Asymmetric encryption uses a pair of keys: a public key (shared openly) and a private key (kept secret). In GPG (GNU Privacy Guard), a sender encrypts a message using the recipient’s public key, and only the recipient can decrypt it using their private key (Garfinkel, 1995).

Encrypting a message ensures confidentiality, meaning only the intended recipient can read it. In contrast, a digital signature verifies the sender’s identity and ensures message integrity by using the sender’s private key; others can verify it with the sender’s public key (Green & Smith, 2016).



2. Comparison: ProtonMail, Gmail, Outlook
Feature	ProtonMail	Gmail	Outlook
(a) Encryption at rest	Yes	Yes	Yes
(b) Encryption in transit	Yes (TLS)	Yes (TLS)	Yes (TLS)
(c) End-to-end encryption	Yes (default)	No (optional tools only)	No (limited/enterprise)
(d) Privacy policies	Strong, zero-access	Data used for services/ads	Data used for services
(e) Jurisdiction	Switzerland	United States	United States

ProtonMail offers end-to-end encryption by default, meaning even the provider cannot read emails. Gmail and Outlook encrypt data in transit and at rest but may still access user data under their policies (Proton AG, 2023; Google, 2024; Microsoft, 2024).


3. PKI vs Web of Trust

A Public Key Infrastructure (PKI) relies on trusted Certificate Authorities (CAs) to verify identities and issue digital certificates.

Advantages over Web of Trust:
Centralized trust model, easier to manage
Widely adopted in web security (SSL/TLS)
Disadvantages:
Single point of failure (if CA is compromised)
Requires trust in third parties

In contrast, the Web of Trust (used by GPG) is decentralized and based on user trust relationships (Kahn Academy, 2020).



4. GDPR vs Ecuador Data Protection Law
Aspect	GDPR (EU)	Ecuador Law
Right to access	Yes	Yes
Right to rectification	Yes	Yes
Right to erasure (“right to be forgotten”)	Yes	Yes
Right to data portability	Yes	Yes
Protection from surveillance	Strong regulation	Regulated but less strict
Email privacy	Explicitly protected	Protected

Both laws grant citizens rights over their personal data, including emails. The GDPR provides stricter enforcement and broader protections, especially regarding digital surveillance (European Union, 2016; Asamblea Nacional del Ecuador, 2021).


5. Email Metadata and Privacy

Email metadata includes information such as sender, recipient, date, time, IP address, and subject line. Unlike the message content, metadata is often not encrypted in standard email systems.

Encryption protects only the body of the message, not metadata, because email routing requires this information to deliver messages. This creates privacy risks, as metadata can reveal communication patterns and relationships even without accessing content (Electronic Frontier Foundation, 2022).

