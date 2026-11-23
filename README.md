# Windows NTLM Hash Security Analysis

## Project Overview

This project documents a controlled lab exercise about Windows NTLM password hashes, why they matter, and how weak passwords can be exposed by offline password-auditing tools.

The goal is educational and defensive: understand the risk, recognize what an audit result means, and learn how to protect Windows accounts. Only hashes from systems you own or have explicit permission to test should ever be used. Do not extract or test credentials from someone else's device or account.

## Tools and Concepts

- **Kali Linux** — the lab environment shown in the terminal screenshot.
- **Hashcat** — a password-auditing tool that can test candidate passwords against supported hash formats.
- **NTLM** — a legacy Windows password-hash format.
- **Dictionary wordlist** — a collection of candidate strings used during an authorized password-strength audit.

This README describes the workflow at a high level and does not provide instructions for extracting real SAM credentials or cracking hashes taken from other people's devices.

## Lab Workflow (High-Level)

1. **Set up an authorized lab.** Use a virtual machine or test computer that you own, and use synthetic/demo credentials rather than real user passwords.
2. **Prepare a test hash.** The lab should use a sample NTLM hash created specifically for the exercise. Keep real account hashes and personal data out of screenshots and repositories.
3. **Run a password-strength audit.** Hashcat can compare the test hash against candidate entries in a wordlist. In the screenshot included below, the terminal shows Hashcat running in NTLM mode with a dictionary-based approach and the `rockyou` wordlist.
4. **Review the result.** The screenshot reports `Status: Cracked` and `Recovered: 1/1`, meaning Hashcat reports that it matched the one digest in that test input. This is evidence of the Hashcat audit stage only; the screenshot does not show how the hash was obtained.
5. **Record defensive lessons.** A weak or commonly used password can be vulnerable to offline guessing. Use long, unique passwords, enable multifactor authentication where available, keep systems updated, and protect local credential stores and backups.

## Screenshot Evidence

The following screenshot is the Hashcat terminal result supplied for this project.

![Hashcat NTLM audit result](screenshots/hashcat-result.webp)

The screenshot shows:

- Hashcat version 7.1.2 starting in a Kali terminal.
- Hash mode `1000`, identified by Hashcat as NTLM.
- A dictionary-based audit using the `rockyou` wordlist.
- One digest in the input and a final status of `Cracked` with `1/1` recovered.

**Important limitation:** this screenshot documents the Hashcat result. It does not show a `samdump2` command, the extraction of a Windows SAM database, or proof that the tested hash came from a particular Windows machine. Do not claim that it proves those steps.

## Security Notes

- Never publish real NTLM hashes, SAM/SYSTEM files, usernames, machine identifiers, or recovered passwords.
- Do not run password audits against accounts or devices without clear authorization.
- Treat a recovered password as compromised: change it immediately on the affected test account and anywhere it was reused.
- Prefer unique passphrases, a password manager, multifactor authentication, and least-privilege accounts.
- Organizations should restrict access to credential stores, monitor suspicious access, and use modern Windows security protections.

## Conclusion

This exercise demonstrates why NTLM hashes and weak passwords require careful protection. The supplied screenshot records a successful one-digest Hashcat audit, while the broader lesson is to use strong, unique credentials and conduct all testing only in an authorized lab.
