# Privacy & Air-Gapped Security Attestation — EdiPrism X12 EDIFACT

**Effective Date:** November 2026

## 1. Zero Data Collection & 100% Offline Execution
**EdiPrism X12 EDIFACT** operates **100% locally and offline** inside your JetBrains IDE process (`IntelliJ IDEA`, `Rider`, `PyCharm`, `DataGrip`, `GoLand`, `WebStorm`):

- **Zero Network Calls:** EdiPrism contains **no** HTTP clients (`java.net.http`, `HttpURLConnection`, `OkHttp`, `Ktor`), cloud SDKs, or LLM integrations. This guarantee is continuously enforced by automated bytecode inspection tests (`ArchitecturePurityTest`) on every build.
- **Zero Payload Transmission:** EDI interchanges, healthcare claims (`837`/`835`/`270`/`271`), purchase orders (`850`/`856`/`810`/`EDIFACT`), schemas (`BYOS`), and scrubbed test fixtures **never leave your workstation**.
- **Zero Telemetry or Analytics:** EdiPrism collects no usage telemetry, crash dumps, file names, identifiers, or analytics.

## 2. Licensing Verification
Commercial `[Pro]` subscriptions and 14-day trials are handled natively by **JetBrains Marketplace (`JetBrains s.r.o.`)** via the IDE's built-in `LicensingFacade` subsystem and verified locally using pinned JetBrains X.509 RSA root certificates.

## 3. Contact
For enterprise procurement, air-gapped compliance inquiries, or support, contact the vendor address listed on the official **EdiPrism X12 EDIFACT** JetBrains Marketplace listing.
