# Universal Azure hardening policies

This directory contains Azure Policy definitions that can be recommended without
collecting customer workload behavior or supplying customer-specific resource
names, locations, identities, networks, or exception values.

## Universal preventive controls

- `require-key-vault-soft-delete.yaml` prevents Key Vault creation or updates
  without soft delete while allowing recovery operations. It is based on the
  Microsoft built-in definition `1e66c121-a66a-4b1f-9b83-0fd99bf0fc2d`.
- `require-storage-secure-transfer.yaml` denies Azure Storage accounts that do
  not require secure transport.

## Universal audit controls

- `audit-public-blob-access.yaml`: reports Storage accounts that permit
  anonymous blob access.
- `audit-storage-tls12.yaml`: reports Storage accounts whose minimum TLS version
  is below TLS 1.2.
- `audit-app-service-https.yaml`: reports App Service applications without
  HTTPS-only access.
- `audit-nic-ip-forwarding.yaml`: reports network interfaces with IP forwarding.
- `audit-public-ip-on-network-interfaces.yaml`: reports network interfaces with
  directly associated public IP addresses.
- `audit-key-vault-purge-protection.yaml`: reports Key Vault resources without
  purge protection.

"Universal audit" means safe to assign broadly because the policy only records
noncompliance. It doesn't mean every finding is forbidden. Review findings for
legitimate public workloads, network appliances, legacy clients, and documented
recovery requirements before promoting an equivalent control to `deny`.

