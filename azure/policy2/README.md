# Conditional and strict opt-in Azure hardening policies

These Azure Policy definitions have security value but aren't safe universal
defaults. Assign them only after validating their prerequisites.

## Conditional controls

- `prevent-public-blob-access.yaml`: verify that anonymous public blob access is
  not a business requirement.
- `require-storage-tls12.yaml`: verify that all Storage clients support TLS 1.2.
- `require-app-service-https.yaml`: verify clients, probes, callbacks, and custom
  domains; deployment slots need separate coverage.
- `disable-nic-ip-forwarding.yaml`: exclude approved network virtual appliances
  and forwarding workloads.
- `prevent-public-ip-on-network-interfaces.yaml`: confirm the customer's ingress
  and egress design and exclude approved public workloads.
- `require-key-vault-purge-protection.yaml`: confirm retention and recovery
  procedures because protected vault content can't be purged early.

## Strict opt-in control

- `prevent-key-vault-deletion.yaml`: blocks direct Key Vault deletion and deletion
  through resource-group cascade. Define exemption and retirement procedures
  before assignment.

Universal preventive and audit controls are maintained in
[`../policy`](../policy).
