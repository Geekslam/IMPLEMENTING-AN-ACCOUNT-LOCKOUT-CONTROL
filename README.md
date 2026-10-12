# Active Directory Account Lockout Policy Lab

A hands-on Windows Server lab demonstrating how to configure and verify an Active Directory Group Policy that locks a user account after **five failed password attempts**.

> **Lab safety:** Perform testing only in an isolated environment and with an account you control. Repeated failed sign-ins can lock out accounts and interrupt access.

## Project overview

This project documents the setup and validation of an account lockout policy using Group Policy in an Active Directory domain. The policy is intended to reduce repeated password guessing by temporarily locking an account after five consecutive failed logon attempts.

## Objectives

- Configure a domain account lockout policy through Group Policy.
- Apply the policy to the intended domain users.
- Verify that repeated incorrect passwords trigger a lockout.
- Document how an administrator can identify and unlock a locked account.

## Lab environment

Replace these examples with your actual setup:

| Component | Example |
|---|---|
| Domain | `cybertech.com` |
| Server OS | `<X64 2022>` |
| Client VM | `<Windows 8>` |
| Policy | `<Account lock out policy>` |
| Lockout threshold | 5 invalid attempts |

Do not include real passwords, private IP details, or sensitive account information in screenshots or documentation.

## Policy settings

The policy uses these account lockout settings:

| Setting | Lab value | Purpose |
|---|---:|---|
| Account lockout threshold | 5 invalid logon attempts | Number of failed attempts before lockout |
| Account lockout duration | `<duration>` | How long the account remains locked |
| Reset account lockout counter after | `<duration>` | How long without another failure before the counter resets |

The threshold alone does not describe the full lockout behavior. Document the configured duration and counter reset interval from your GPO so readers can reproduce the result accurately.

## Configuration

1. Sign in to the domain controller with an account authorized to manage Group Policy.
2. Open **Group Policy Management** (`gpmc.msc`).
3. Create or edit the GPO used for the lab. Record its name as `<Account lockout policy>`.
4. Configure the account lockout policy at:

   **Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Account Lockout Policy**

5. Set **Account lockout threshold** to **5 invalid logon attempts**.
6. Configure **Account lockout duration** and **Reset account lockout counter after** to the values chosen for your lab.
7. Link the GPO to the domain or the appropriate organizational unit (OU). Confirm the target accounts and computers are in scope.
8. On a test client, refresh Group Policy from an elevated Command Prompt:

   ```cmd
   gpupdate /force
   ```

9. Check which policies applied:

   ```cmd
   gpresult /r
   ```

   For a more detailed report, save an HTML result:

   ```cmd
   gpresult /h gpresult.html
   ```

> Domain account policies are generally controlled by the domain-linked policy. If you linked the GPO to an OU, verify that its scope and precedence produce the policy you expect. Use Group Policy Results or `gpresult` to confirm the effective setting rather than relying only on the GPO editor.

## Verification procedure

1. Use a **test account** that you control. Confirm it is enabled and not already locked.
2. Confirm the effective account lockout policy and record the lockout duration and counter reset interval.
3. From a domain-connected test client, attempt to sign in with the test account and an intentionally incorrect password up to five times.
4. Stop when the account is locked. Avoid testing against administrator or production accounts.
5. Verify the lockout in **Active Directory Users and Computers** by inspecting the test account, or review the domain controller’s Security event log for relevant account lockout events.
6. Record the result, including the time, test client, effective policy, and the observed lockout behavior. Redact usernames or machine details if they identify people or systems you should not disclose.
7. Unlock the test account after the test, if needed, and confirm that a valid sign-in works.

## Troubleshooting

- **The account does not lock after five failures:** Check that the GPO is linked to the correct scope, that it has applied, and that another policy is not determining the effective setting. Review `gpresult` or Group Policy Results.
- **The wrong account or domain is being tested:** Confirm the client is signed in against the intended domain and that you are testing a domain account rather than a local account.
- **The account is already locked:** Have an authorized administrator unlock it in Active Directory Users and Computers, then review the lockout event details before repeating the test.
- **The policy values differ from the GPO editor:** Check the winning policy and its precedence. For domain accounts, verify the effective domain account policy.
- **Repeated lockouts occur unexpectedly:** Review domain controller security logs and investigate the source before resetting the password or unlocking the account.

## Security notes

Account lockout can slow password guessing, but attackers may also abuse it to deny users access by deliberately causing lockouts. Choose duration and reset values that fit the lab or organization, monitor lockout events, and use other protections such as strong passwords and multifactor authentication where available.

This lab is for authorized testing and learning. Do not use it to lock out accounts you do not own or administer.

## Evidence to include

For a useful GitHub project, include sanitized evidence such as:

- A screenshot of the GPO’s lockout settings.
- The GPO name and link location.
- `gpresult` output showing that the policy applied.
- A screenshot or log excerpt showing the test account lockout.
- A short results section describing what happened.

Before committing screenshots or logs, remove passwords, personal data, public IPs, and unrelated domain details.

## Results

Fill this section in after completing the lab:

- **Policy applied to:** `<Cybersecurity>`
- **Threshold:** 5 invalid attempts
- **Lockout duration:** `<30mins>`
  

## Disclaimer

This repository documents a controlled educational lab. Follow your organization’s policies and test only on systems and accounts you are authorized to manage.
