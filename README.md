# AD Learning Path 27 — Configure the Domain Password Policy

## Objective
Configure and validate the domain-wide password policy at the domain root and understand how the resultant policy is calculated.

## Prerequisites
- Approved lab policy
- Authorized domain-wide change account
- Test users
- Existing values recorded for rollback

## Setup
1. Record the current default domain password policy.
2. Set a lab baseline such as minimum length 14, history 24, complexity enabled, reversible encryption disabled, and a deliberate age policy.
3. Confirm the account-policy GPO is linked at the domain root.
4. Validate the effective policy and test one compliant and one noncompliant password reset on a disposable account.
5. Document service-account and application compatibility.

```powershell
Get-ADDefaultDomainPasswordPolicy | Format-List *

Set-ADDefaultDomainPasswordPolicy -Identity corp.lab `
    -MinPasswordLength 14 `
    -PasswordHistoryCount 24 `
    -ComplexityEnabled $true `
    -ReversibleEncryptionEnabled $false `
    -MinPasswordAge '1.00:00:00' `
    -MaxPasswordAge '60.00:00:00'
```

## Validation
```powershell
Get-ADDefaultDomainPasswordPolicy | Format-List *
net.exe accounts /domain
Get-ADUserResultantPasswordPolicy ajohnson
```

## Evidence
Store before/after policy, GPO source/link, accepted and rejected test results, compatibility notes, troubleshooting, and final pass/fail status under `evidence/`.

## Troubleshooting
- Policy unchanged: confirm domain-root processing and GPO precedence.
- User result differs: check for a fine-grained password policy.
- Immediate password change fails: review minimum password age.

## Security notes
Use stronger passwords together with MFA and credential protection. Do not enable reversible encryption without a documented legacy requirement.

## Cleanup
Restore recorded values only through a controlled domain-wide change.

## References
- Microsoft Learn: Password policy
- Microsoft Learn: `Set-ADDefaultDomainPasswordPolicy`

## Next activity
`AD-Learning-Path-28-Configure-the-Account-Lockout-Policy`
