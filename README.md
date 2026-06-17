# 🛠️ Cisco IOS Host Hardening Playbook

A centralized engineering matrix containing hardened baselines, access control configurations, and terminal security blueprints for local lab deployments.

---

## 🔒 1. Securing the Baseline & Hardening Command Logs

Once the initial access layers were established at the `Switch>` prompt, privileges were elevated to execute baseline security hardening configurations to secure local physical consoles and virtual network terminals:

```cisco
Switch> enable
Switch# configure terminal
Switch(config)# hostname Shadow

# Hardening HTTP/HTTPS Web Management Access Layers
Shadow(config)# ip http authentication local
Shadow(config)# ip http secure-server

# Securing the Physical Console Line (Auxiliary Port)
Shadow(config)# line con 0
Shadow(config-line)# login local
Shadow(config-line)# stop bits 1
Shadow(config-line)# exit

# Securing Virtual Terminal (VTY) Lines for Encrypted Network Access
Shadow(config)# line vty 0 4
Shadow(config-line)# login local
Shadow(config-line)# transport input ssh
Shadow(config-line)# exit
```

## 📝 Analyst Operational Notes

*   **Human Factors Matrix:** Setting local authentication parameters (`login local`) without mapping a corresponding master username or encrypted secret string initially caused an administrative lockout loop. Ensure the user access control database is populated before closing out terminal execution loops.
*   **Forensic Session Recovery:** Command sequences were successfully recovered on 2026-05-22 via terminal scrollback buffer analysis on `SEC-NODE-TOWER-01`, verifying that the configuration was successfully committed to memory banks despite initial credential mismatches.

---

---

## 🔑 2. Cisco Catalyst 3850 ROMMON Recovery & Privilege 15 Execution

**Objective:** Execute a physical hardware interrupt to bypass an administrative configuration lockout, initialize a permanent Privilege Level 15 superuser account, and establish persistent device control.

### ⚙️ Physical Interrupt Procedure
1. **Chassis LED Synchronization:** Initial attempts failed due to tracking console cable signaling lines rather than physical switch chassis LEDs. Paused operations to analyze hardware states.
2. **Subsystem Break Sequence:** Executed a physical hold on the hardware `MODE` button during the early power-on self-test (POST) sequence. Successfully interrupted the standard boot matrix and dropped the system into the `rommon 1>` execution shell.
3. **Bypassing NVRAM Storage:** Configured the boot register settings to explicitly ignore the encrypted startup configuration file located within NVRAM, allowing a clean initialization boot into default memory space.

### 💻 Privileged Account Initialization Logs

Upon reaching the unlocked system prompt, terminal session privileges were immediately escalated to absolute structural control to commit the master administrative user:

```cisco
Switch> enable
Switch# configure terminal

# Initializing Master Account with Absolute Privileges (Level 15)
Switch(config)# username admin_operator privilege 15 secret 8 \$eCrEt_PaSs_StRiNg!
Switch(config)# end

# Writing the new access database configuration directly to persistent NVRAM
Switch# write memory
Switch# exit
```

### 📝 Analyst Quality Assurance Notes

*   **Boundary Authentication Test:** Forced an explicit terminal termination (`exit`) to systematically verify credential persistence and database indexing parameters before initiating network interface bindings.
*   **Incident Deflection:** Discovered an operational credential mismatch (typographical error during execution) that induced a local authentication lockout loop.
*   **Corrective Action Vector:** Initiated a physical reset and repeated the ROMMON hardware interrupt blueprint from the baseline layer. The secondary deployment loop yielded a 100% successful authentication state with verified Privilege 15 terminal execution.

---
*Next Operational Track: Hardware baseline secured and completely under root administrative control. Initializing Layer 2 network segmentation protocols.*

---

---

## 🔌 3. Physical Layer 1 Infrastructure Mapping & Structural Cable Routing

**Objective:** Design and deploy a dedicated, physical Category 6 (Cat 6) copper infrastructure array to eliminate wireless overhead vulnerabilities, minimize electromagnetic interference (EMI), and establish secure hardline pathways.

### 🏗️ Routing Layout & Termination Node Drops
Successfully pulled, structured, routed, and punched down a total of 9 independent copper drops through the attic framing to isolate localized endpoints:

*   **Endpoint Zone Alpha:** 1 dedicated drop (Primary Gaming Engine)
*   **Endpoint Zone Beta:** 2 dedicated drops (Gaming Engine, Local Media Server)
*   **Command Node Vault (Operator Workspace):** 4 dedicated drops (Workstation Engine, Tablet Device, Console Infrastructure, and local laptop testing node)
*   **Auxiliary Monitoring Node:** 1 dedicated drop (Remote Media Monitoring Display)
*   **Core Gateway Interface Interconnect:** 1 dedicated drop (Connecting the primary perimeter security router to the upstream service gateway modem)

### 💻 Initial Physical Interface Mapping (Redacted` Catalyst 3850)

```cisco
# Mapping the physical ingress lines directly into the core switch configuration
Shadow# show interfaces description

Interface             Status         Protocol      Description
----------------------------------------------------------------------------------
Gi1/0/2               up             up            Hardline Workstation Node (Redacted)
Gi1/0/24              up             up            Perimeter Edge Router Gateway
```

*Note: The remaining physical attic drops are currently patched into the distribution frame with port matrix assignments pending final logic segmentation.*

### 📝 Analyst Operational Notes

*   **Infrastructure Findings:** The physical copper topology is 100% complete and fully verified at Layer 1 using physical continuity testing tools. 
*   **Next Operational Vector:** Transitioning operations from physical routing to logical Layer 2 hardening. Next steps involve building out localized virtual local area networks (VLAN isolation zones or "Toddler Bubbles") and enforcing explicit MAC-address filtering rules (`port-security`) across all newly deployed ingress interfaces on the Catalyst switch.
