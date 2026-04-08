
1.What is asymmetric encryption (public/private key) and how does it work in the context of GPG? What is the difference between encrypting a message and digitally signing it?
To provide a protected and private way to send messages or files, asymmetric encryption is an approach utilizing two keys called a public key and private key. The public key may be sent openly, while the private key would remain secret. These two keys relate mathematically to each other with respect to the encryption/decryption operations performed with them (Kahn Academy, 2020).
An example of this would be how GPG (GNU Privacy Guard) implements the use of asymmetric encryption to provide confidentiality to protected data when sending through GPG. In this case, the public key will be used by the sender of the data to encrypt the data, while the recipient will use the private key to decrypt the message once received (Callas et al., 2007). 
Encrypting data with a given key ensures that none of the encryption keys will be accessible to the sending/receiving parties of the message after it has been sent. On the other hand, verifying the identity and verifying that the data from the sender of the encrypted message has not changed since being sent will be possible with digital signatures. Ultimately, encryption provides confidentiality for the message content and the use of digital signatures provides for authenticity and integrity of each respective message (Stallings, 2017).
2.Compare ProtonMail, Gmail and Outlook in terms of: (a) encryption at rest, (b) encryption in transit, (c) end-to-end encryption, (d) privacy policies and provider access to data, and (e) legal jurisdiction.
Criterion	ProtonMail	Gmail	Outlook
(a) Encryption at rest	Yes	Yes	Yes
(b) Encryption in transit	Yes (TLS)	Yes (TLS)	Yes (TLS)
(c) End-to-end encryption	Yes (between users)	Not by default	Not by default
(d) Privacy / data access	High privacy	Automated data processing	Limited access under policies
(e) Legal jurisdiction	Switzerland	USA	USA

In general, ProtonMail focuses more on user privacy, while Gmail and Outlook prioritize service integration and functionality. These differences are influenced by both technical design and legal frameworks (Green & Smith, 2016).
3.What is the PKI model based on Certificate Authorities (CA)? Mention 2 advantages and 2 disadvantages compared to the Web of Trust.
As noted by Adams and Lloyd, Public Key Infrastructure (PKI): Certificate Authorities (CA) are organizations responsible for verifying the identity of users and issuing the digital certificates to create trust in online communication.
The benefits of using a CA include:
* They simplify user identity management across large systems.
* They provide instant trust by virtue of an accepted authority.
The drawbacks of using a CA include:
* CAs rely on outside vendors for verification and access to the CA's trusted certificate.
* Security compromises involving a CA could potentially affect thousands of users.
4. Research the General Data Protection Regulation (GDPR) of the EU and the Organic Law on Personal Data Protection of Ecuador. What rights do they grant citizens regarding email and digital surveillance? (provide a comparative table)
Aspect	GDPR	Ecuador
Right of access	Allows users to know what data is held about them	Guaranteed
Rectification	Allows correction of inaccurate data	Allowed
Right to be forgotten	Allows deletion of personal data	Included
Consent	Must be explicit	Must be clear and informed
Use of email data	Strictly regulated	Regulated
Digital surveillance	Legally limited	Limited
Data portability	Allowed	Allowed

Both regulations aim to protect user privacy in digital environments and establish limits on how personal data is processed (European Union, 2016; Asamblea Nacional del Ecuador, 2021).
5. What is email metadata? Why does content encryption not protect metadata, and what are the privacy implications?
The term "email metadata" describes all of the non-content items associated with an e-mail message (e.g., sender, recipient, date, time, route, etc.). Unfortunately, encrypting the content of an e-mail does not protect metadata. Metadata is necessary for the successful transmission of an email message from sender to recipient through the network; thus, even if the email message's content is encrypted, the email message's metadata is still accessible.
This has significant privacy implications because analyzing email metadata may reveal patterns of communication, relationships between senders and recipients, and the frequency of sender-recipient interactions, all without having access to the actual content of the message (Solove, 2011).
