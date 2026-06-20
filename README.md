# Electronic Medical Record

**This project gives you a FHIR R4 API and a small web UI: register a synthetic patient, write a clinical note, and watch a tamper-evident audit chain grow in real time.**

## How to install

```bash
git clone https://github.com/your-username/healthcare-go-test.git
cd healthcare-go-test

cp .env.example .env
```
You can modify environment values here.

```bash
go mod tidy
go run cmd/emrd/main.go
```

Pick a synthetic clinician, register a patient (the NHI is generated in either the legacy `AAA111#` or post-July-2026 `AAA11A#` format — both validated per HISO 10046), write a note, and watch the audit panel: every read and write is an `AuditEvent`, BLAKE3-hash-chained to the previous one. Press **Verify chain**.

Prefer curl? The API is plain FHIR R4:

```bash
curl -s -X POST localhost:8080/fhir/r4/Patient \
  -H 'X-Actor-HPI: 99ZZZA' -H 'Content-Type: application/json' \
  -d '{"resourceType":"Patient",
       "identifier":[{"system":"https://standards.digital.health.nz/ns/nhi-id","value":"ZZZ0016"}],
       "name":[{"family":"Skeleton","given":["Walking"]}]}'
curl -s localhost:8080/audit/verify
```