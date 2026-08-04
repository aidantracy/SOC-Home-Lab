# SOC Home Lab — Handoff

Snapshot of where the lab stands so a new chat can pick up without re-deriving everything.

## Current status (Lab 00 — foundation)

| Component | State |
|-----------|-------|
| Host virtualization platform | **Hyper-V** (decided — see below) |
| Windows edition | **Windows 11 Education** (upgraded from Home for Hyper-V) |
| Hyper-V role + Hyper-V Manager | Enabled and working |
| `SOC-Lab` Internal virtual switch | Created; host `vEthernet (SOC-Lab)` = `192.168.56.1` |
| Kali attacker VM | **Installed and working**, on `SOC-Lab`, static IP `192.168.56.10` |
| Windows 11 victim VM | **Not built yet** (Step 4) |
| Ubuntu SIEM VM | **Not built yet** (Step 5) |

## Lab network scheme (Hyper-V Internal switch "SOC-Lab", 192.168.56.0/24, no DHCP, no internet)

- Host `vEthernet (SOC-Lab)` — `192.168.56.1`
- Kali attacker — `192.168.56.10`
- Windows 11 victim — `192.168.56.20`
- Ubuntu SIEM — `192.168.56.30`

## Next steps

1. **Build the Windows 11 victim VM** (README Step 4): Gen 2, TPM + Secure Boot ON, static `192.168.56.20`. Use the temporary-internet trick during install if needed.
2. **Build the Ubuntu Server SIEM VM** (README Step 5): Gen 2, Secure Boot off / MS UEFI CA, static `192.168.56.30`. This hosts Splunk in Lab 01.
3. **Verify connectivity + isolation** (Step 7): Kali can ping `.20`/`.30`; no VM can reach the internet (`ping 8.8.8.8` fails).
4. **Checkpoint every VM** as `clean-baseline` (Step 8).
5. Move on to **Lab 01 — Splunk SIEM**.

## Key decisions & gotchas (so we don't relitigate them)

- **Hyper-V, not VirtualBox.** Windows 11's Memory Integrity / VBS keeps the hypervisor resident, so VirtualBox could only run through the slow WHP backend (green turtle), which black-screens Windows 11 guests. Running the lab natively in Hyper-V avoids all of it and coexists with the Claude workspace (which also needs the hypervisor) — no toggling.
- **Windows Home can't run Hyper-V.** Fixed by upgrading to **Windows 11 Education** for free via the Azure Education Hub (Boise State student email) — in-place product-key change, no reinstall.
- **Linux guests on Gen 2 need Secure Boot OFF** (or the "Microsoft UEFI Certificate Authority" template), or they won't boot ("unable to load OS"). Applies to Kali and Ubuntu. Windows 11 keeps Secure Boot + TPM ON.
- **"Not enough memory to start"** = give the VM less Startup RAM (2048 MB) with Dynamic Memory (min 1024 / max 4096), and run VMs one at a time if host RAM is tight. *(Host total RAM still unconfirmed — check Task Manager → Performance → Memory; if 8 GB, run one VM at a time.)*
- **Installer network / DHCP fails on the isolated switch** — expected, there's no DHCP on `SOC-Lab`. Easiest path: temporarily switch the VM's adapter to the **Default Switch** (NAT + DHCP + internet) for the install, then move it back to `SOC-Lab` and set the static IP inside the OS with `nmcli`.
- Set static IP inside a Linux guest:
  ```
  sudo nmcli con mod "Wired connection 1" ipv4.method manual ipv4.addresses 192.168.56.XX/24 ipv4.gateway 192.168.56.1
  sudo nmcli con up "Wired connection 1"
  ```

## Housekeeping / open items

- The old VirtualBox screenshots in `00-lab-setup/images/` (`Step2_virtualbox_setup.png`, `Step3–Step6.png`) are stale — recapture Hyper-V equivalents and delete the old ones.
- One-off helper scripts from setup (Hyper-V/VirtualBox toggle, boot menu) are no longer needed now that we're all-in on Hyper-V.
- `git` had a stale `.git/index.lock`; if commits fail, delete that file first, then `git add` + commit. Lab 00 README edits are still uncommitted.
- Top-level `README.md` may still reference VirtualBox — worth a consistency pass.
