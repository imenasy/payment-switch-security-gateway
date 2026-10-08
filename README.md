markdown
# Payment Switch & Interoperability Security Gateway

Zero-trust architecture baselines and secure API specifications connecting 30 consolidated District SACCOs with national payment systems (RIPPS, RNDPS, and Mobile Money switches).

## Core Controls
- Hardware Security Module (HSM) lifecycle governance (PIN blocks, ISO 8583 field encryption).
- Mutual Transport Layer Security (mTLS) enforcement with strict PKI x509 validation.
- API threat mitigation: Token replay defense, rate limiting, and OWASP API Top 10 controls.



*nginx/ripps_rndps_mtls.conf*:

nginx
# Production Reverse Proxy Gateway for Clearing Settlement (mTLS Enforcement)
server {
    listen 8443 ssl http2;
    server_name switch-gw.sacco-bank.rw;

    # Server Certificates
    ssl_certificate /etc/pki/tls/certs/sacco_switch_gw.crt;
    ssl_certificate_key /etc/pki/tls/private/sacco_switch_gw.key;

    # Client Certificate Verification (National Switch Root CA)
    ssl_client_certificate /etc/pki/tls/certs/national_switch_ca.crt;
    ssl_verify_client on;
    ssl_verify_depth 2;

    # Cipher Suite Hardening
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384';
    ssl_prefer_server_ciphers on;

    # Rate Limiting & Proxying to Core Interoperability API
    location /v1/clearing/ {
        limit_req zone=switch_zone burst=20 nodelay;
        proxy_pass https://backend_clearing_cluster;
        proxy_set_header X-Client-DN $ssl_client_s_dn;
        proxy_set_header X-Client-Verify $ssl_client_verify;
    }
}



---
