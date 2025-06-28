### Types of sensitive data

1. PII (Personal Identification Information): 
2. Financial Data
3. Authentication Credentials
4. Regulated Data (GDPR-protected, PCI-DSS governed data)

### How to Protect Sensitive Data Across the Full Lifecycle
1. **Collection**: Use HTTPS, validate input, and tokenize sensitive data at the source (e.g., card info).
2. **In Transit**: Encrypt all communication using TLS and authenticate internal service calls with mTLS or signed tokens.
3. **At Rest**: Encrypt sensitive data using strong algorithms (AES-256) and manage keys securely via a vault or KMS.
4. **In Use**: Limit decryption scope, restrict memory exposure, and enforce access only to authorized processes.
5. **Logging**: Mask or redact sensitive fields, and avoid logging credentials or PII in any environment.
6. **Access Control**: Apply least-privilege access via RBAC/ABAC, and securely store secrets with fine-grained permissions.
7. **Deletion**: Comply with data retention policies by securely deleting or anonymizing sensitive data when no longer needed.

#### Other Concepts

1. **Tokenization**
Tokenization replaces sensitive data like card numbers with meaningless tokens stored securely in a vault. Unlike encryption, tokens can’t be reversed without access to the vault. It’s commonly used in FinTech to reduce PCI-DSS scope and prevent data breaches.
If the tokenization service is compromised, real sensitive data could be exposed. That’s why the vault must be isolated, encrypted, audited, and hardened with strict access controls, key separation, and zero-trust policies — treating it as a high-value target.
