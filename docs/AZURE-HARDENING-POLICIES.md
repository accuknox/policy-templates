# Azure hardening policy classification

## Azure Policy compared with AWS SCPs

Azure Policy is the closest Azure governance mechanism to the resource-hardening
part of an AWS SCP catalog, but it isn't an identity permission boundary. Azure
Policy evaluates Azure Resource Manager resources and can deny noncompliant
resource creation, updates, or supported actions. Identity requirements such as
MFA and sign-in risk belong in Microsoft Entra Conditional Access rather than
Azure Policy.

For organization-wide governance, define policies at a management group and
assign them to that management group or its descendant subscriptions. An Azure
Policy definition has no effect until an assignment exists.

## Classification standard

- **Universal:** no customer-specific identifiers or architecture assumptions,
  low likelihood of disrupting legitimate operations, and no behavioral data
  required.
- **Universal audit:** safe to assign broadly without behavioral data because it
  only reports noncompliance. A finding can still represent an approved design
  and must be reviewed before the equivalent preventive policy is enabled.
- **Conditional:** broadly useful after a documented feature, compatibility, or
  architecture prerequisite is verified.
- **Strict opt-in:** imposes a strong lifecycle or recovery restriction and needs
  explicit customer acceptance plus an exemption or break-glass process.

## Current catalog

| Policy | Tier | Reason |
| --- | --- | --- |
| Require Key Vault soft delete | Universal | New vaults enable it by default, it cannot be disabled after enablement, recovery is exempted, and no customer-specific values are needed |
| Require Storage secure transfer | Universal | Secure transport is enabled by default for new Storage accounts and needs no customer-specific values |
| Audit anonymous public blob access | Universal audit | Discovery is non-blocking; public websites and datasets remain possible and appear as reviewable findings |
| Audit Storage TLS 1.2 | Universal audit | Safely identifies legacy protocol exposure without breaking older clients |
| Audit App Service HTTPS-only | Universal audit | Reports HTTP-enabled applications without disrupting clients, probes, callbacks, or deployment slots |
| Audit NIC IP forwarding | Universal audit | Reports forwarding while leaving legitimate virtual appliances and routers operational |
| Audit public IPs on NICs | Universal audit | Reports direct internet exposure without blocking approved public workloads |
| Audit Key Vault purge protection | Universal audit | Reports missing protection without changing retention or deletion behavior |
| Prevent anonymous public blob access | Conditional | Public websites or download workflows can legitimately require anonymous access |
| Require Storage TLS 1.2 | Conditional | Legacy clients using TLS 1.0 or 1.1 can fail |
| Require App Service HTTPS-only | Conditional | Existing clients, probes, callbacks, or deployment slots can require migration |
| Disable NIC IP forwarding | Conditional | Network virtual appliances and routers require forwarding |
| Prevent public IPs on NICs | Conditional | Some internet-facing systems and appliances require direct public IP addresses |
| Require Key Vault purge protection | Conditional | Retention prevents early permanent deletion and changes vault lifecycle operations |
| Prevent Key Vault deletion | Strict opt-in | Legitimate deletion and resource-group retirement are blocked until exemption or policy removal |

## Important operational behavior

- The `deny` effect blocks new or updated noncompliant resources but doesn't
  automatically repair existing resources.
- The `audit` effect creates compliance findings but doesn't block or modify the
  evaluated resource.
- The `denyAction` effect can block deletion and resource-group cascade deletion.
- Assignments can include exclusions and policy exemptions for approved cases.

## Catalog layout

- `azure/policy`: 2 universal preventive policies and 6 universal audit
  policies.
- `azure/policy2`: 6 conditional preventive policies and 1 strict opt-in policy.

## References

- [Azure Policy overview](https://learn.microsoft.com/azure/governance/policy/overview)
- [Azure Policy definition structure](https://learn.microsoft.com/azure/governance/policy/concepts/definition-structure)
- [Azure Policy deny effect](https://learn.microsoft.com/azure/governance/policy/concepts/effect-deny)
- [Azure Policy audit effect](https://learn.microsoft.com/azure/governance/policy/concepts/effect-audit)
- [Azure Policy denyAction effect](https://learn.microsoft.com/azure/governance/policy/concepts/effect-deny-action)
- [Azure Storage secure transfer](https://learn.microsoft.com/azure/storage/common/storage-require-secure-transfer)
- [Azure Storage anonymous-access prevention](https://learn.microsoft.com/azure/storage/blobs/anonymous-read-access-prevent)
- [Azure Key Vault recovery management](https://learn.microsoft.com/azure/key-vault/general/key-vault-recovery)
- [Microsoft built-in Key Vault soft-delete policy](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Key%20Vault/SoftDeleteMustBeEnabled_Audit.json)
