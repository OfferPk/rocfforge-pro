\# ROCFForge Pro (v1.0.0)

\*\*CCSDS 911.5-B-2 / ISO 26145:2023 Space Link Extension (SLE) Return Operational Control Field (ROCF) Ground Gateway & Closed-Loop Telecommand Studio\*\*

\---

\#\# 1\. Executive Summary

\*\*ROCFForge Pro\*\* is an autonomous, zero-dependency developer studio and headless command-line interface implementing the \*\*CCSDS 911.5-B-2 (Space Link Extension — Return Operational Control Field Service)\*\* and \*\*ISO 26145:2023\*\* standards.

In space communications, closed-loop telecommanding relies on rapid, deterministic feedback from spacecraft to ground transmitters. While high-volume scientific instrument payload telemetry is delivered via services like Return All Frames (RAF) or Return Channel Frames (RCF), the telecommand desk requires immediate access only to the \*\*4-octet Operational Control Field (OCF)\*\* containing the \*\*Communications Link Control Word (CLCW)\*\*.

The CLCW reports the real-time status of the spacecraft Frame Acceptance and Reporting Mechanism (FARM-1), including:  
\- Next Expected Frame Sequence Number \$V(R)\$  
\- Lock-Out state flags requiring telecommand operator intervention  
\- Buffer Wait congestion flags  
\- Retransmit sequence gap flags for COP-1 Go-Back-N telecommand retransmissions  
\- Antenna RF Tracking carrier, subcarrier, and symbol sync lock flags

ROCFForge Pro provides:  
1\. Pure ES6+ modular JavaScript engine and Python 3 CLI.  
2\. Complete 3-state SLE Finite State Machine simulation (\`UNBOUND\`, \`READY\`, \`ACTIVE\`).  
3\. Earth Reception Time (ERT) stamping in strict compliance with the \*\*CCSDS 301.0-B-4 Day Segmented (CDS)\*\* time code format.  
4\. Global Virtual Channel Identifier (GVCID) filtering across Spacecraft ID and VCIDs (\$0 \\le \\text{VCID} \\le 63\$).  
5\. Delivery Mode arbitration (\`onlineTimely\`, \`onlineComplete\`, \`offline\`).  
6\. A 10-Point Normative Aerospace Standards Compliance Auditor (\`ROCF-001\` through \`ROCF-010\`).

\---

\#\# 2\. Standards Compliance

\- \*\*CCSDS 911.5-B-2\*\*: Space Link Extension — Return Operational Control Field Service Specification (Blue Book, Issue 2).  
\- \*\*ISO 26145:2023\*\*: Space data and information transfer systems — Space link extension (SLE) — Return operational control field service.  
\- \*\*CCSDS 232.1-B-2\*\*: Communications Operation Procedure-1 (COP-1) / Communications Link Control Word (CLCW) format.  
\- \*\*CCSDS 132.0-B-3\*\*: TM Space Data Link Protocol (OCF placement).  
\- \*\*CCSDS 301.0-B-4\*\*: Time Code Formats (CCSDS Day Segmented \- CDS Code).

\---

\#\# 3\. System Architecture & Lifecycle

\`\`\`  
\[Spacecraft Onboard FARM-1\] \---\> \[Generates 4-Octet CLCW\] \---\> \[Telemetry Frame OCF\]  
                                                                        \|  
                                                                        v  
                                                   \[Ground Antenna Receiving Station\]  
                                                   \- Carrier / Subcarrier Lock  
                                                   \- Earth Reception Time (ERT) CDS  
                                                                        \|  
                                                                        v  
                                                \+-------------------------------+  
                                                \|   ROCFForge Pro SLE Provider  \|  
                                                \|  \- State 1: UNBOUND           \|  
                                                \|  \- State 2: READY (ROCF-BIND) \|  
                                                \|  \- State 3: ACTIVE (START)    \|  
                                                \+-------------------------------+  
                                                                        \|  
                                                                        v  
                                                   \[GVCID Virtual Channel Filter\]  
                                                                        \|  
                                                                        v  
                                                   \[Telecommand Desk / FOP-1 Driver\]  
                                                   \- Evaluates V(R) Acknowledgement  
                                                   \- Regulates Sliding Uplink Window  
\`\`\`

\---

\#\# 4\. Quickstart & Usage

\#\#\# 4.1 Headless Python 3 CLI

Synthesize a 4-octet CLCW word:

\`\`\`bash  
\# Synthesize and inspect CLCW with VCID 2, report value 15, and Retransmit flag  
python3 source-code/rocf\_cli.py clcw \--vcid 2 \--report-val 15 \--retransmit  
\`\`\`

Execute telemetry stream distribution and automated compliance audit:

\`\`\`bash  
\# Run 20 OCF stream cycles in onlineTimely mode  
python3 source-code/rocf\_cli.py run \--mode onlineTimely \--count 20

\# Filter exclusively for VCID 0 with JSON output  
python3 source-code/rocf\_cli.py run \--mode onlineTimely \--filter-vcid 0 \--count 50 \--json  
\`\`\`

\#\#\# 4.2 Interactive Browser Mission Control Console

Open \`source-code/index.html\` directly in any standard modern web browser (zero build step or web server required):  
\- Real-time SLE State lifecycle controls (\`BIND\`, \`START\`, \`STOP\`, \`UNBIND\`).  
\- Interactive Antenna RF Lock toggle simulating atmospheric link loss.  
\- Live CLCW telemetry waterfall table with CDS UTC timestamps, VCID, \$V(R)\$, FARM-B counter, and Lockout/Wait/Retransmit flags.  
\- Real-time 10-point normative auditor scorecard.

\---

\#\# 5\. Automated Verification Test Suite

ROCFForge Pro includes a 10-point automated unit test runner (\`source-code/test\_runner.py\`):

\`\`\`bash  
python3 source-code/test\_runner.py  
\`\`\`

\*\*Results\*\*:  
\- Ran 10 tests in 0.003s.  
\- \*\*Pass Rate\*\*: 100% (10/10 PASS).  
\- \*\*Certification Status\*\*: \`CCSDS\_911\_5\_ROCF\_CERTIFIED\`.

\---

\#\# 6\. Commercial Model & Licensing

\- \*\*Community Open-Core\*\*: Free CLI inspection and standard CLCW parser for research, university satellite clubs, and open-source CubeSat teams.  
\- \*\*Professional Station Seat (\$349/year)\*\*: Ground station engineering workstation license with live UDP/TCP socket bindings, real-time waterfall monitors, and automated pass logging.  
\- \*\*Enterprise Ground Network License (\$2,499 one-time)\*\*: Full source code license, automated DSN/ESTRACK interface adapters, multi-antenna provider multiplexing, and commercial MOC integration support.

\---

\#\# 7\. Known Limitations

\- Simulates terrestrial SLE network protocol layers, OCF extraction, Earth Reception Time stamping, and antenna lock status.  
\- Physical parabolic antenna tracking servos, high-gain cryogenic amplifiers, and physical radio frequency demodulators require physical ground station hardware.  
