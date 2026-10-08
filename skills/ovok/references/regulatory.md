# Signals and regulatory claims

Before describing Signals, MDR status, or an integrating app's medical-device status, check current [Signals](https://docs.ovok.com/signals/llms.txt) and [Data Ingestion](https://docs.ovok.com/data-ingestion/llms.txt) docs.

- Actimi Signals is standalone, cloud-based medical-device software and is MDR Class IIa. **Ovok itself is not a certified medical-device product.** Never transfer Signals' classification to Ovok or to an integrating app.
- Signals is optional. Enrollment is configurable per patient; only eligible vital-sign readings for patients enrolled in Signals are forwarded, according to the currently documented behavior. Do not guess which measurements are eligible or imply that all patient data is sent.
- An integrating app's classification depends on its own intended purpose and actual behavior. Do not give a blanket classification conclusion; recommend product-specific regulatory review when relevant.
- Distinguish Signals' analysis and alerts from the FHIR backend, the integrating app's UI, and the app team's workflow. Signals is not intended to present its results directly to end users.

Link the official [Signals docs](https://docs.ovok.com/signals) when making product claims. The public product reference lists UDI-DI `42700031203-API-XG`; verify current identifiers before presenting them as current. This guidance is not a legal determination.
