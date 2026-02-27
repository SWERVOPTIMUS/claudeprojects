<framework id="swervoptimus-swe-framework" version="2.1.0" updated="2026-02-27" canonical="github.com/SWERVOPTIMUS/CLAUDE_FRAMEWORK_AGENT.md">

<!-- 
  AGENT INSTRUCTION: This is the machine-optimized, AI-first version of the 
  SWERVOPTIMUS AI-Assisted SWE Framework. 
  
  PARSE all tagged sections. TREAT every <rule> as a binding directive. 
  TREAT every <check> as a required verification step.
  
  CORE PHILOSOPHY: All outputs (code, config, docs, data, APIs) are 
  AI-optimized by default. Human-readable versions are produced as 
  augmentations when needed. Accuracy, speed, and token efficiency 
  govern every decision.
  
  A human-readable companion exists at CLAUDE_FRAMEWORK.md in the same repo.
  When conflicts exist between the two, THIS file takes precedence for agents.
-->

# CLAUDE_FRAMEWORK_AGENT.md
## SWERVOPTIMUS AI-Assisted SWE Framework — Agent-Optimized v2

---

<core_philosophy id="AI-FIRST">
<!--
  This section governs ALL output decisions. Every other section inherits from it.
  When in doubt about format, structure, or approach: default to AI-optimized.
-->

<principle id="P-001" priority="foundational">
  DEFAULT output format is AI-optimized: structured, tagged, machine-parseable, token-efficient.
  Human-readable versions are AUGMENTATIONS, not defaults.
</principle>

<principle id="P-002" priority="foundational">
  The three optimization targets in priority order:
    1. ACCURACY — correct output on first pass; rework wastes more tokens than planning
    2. SPEED — minimize round-trips, maximize output per exchange
    3. TOKEN EFFICIENCY — every token must carry information; eliminate structural waste
</principle>

<principle id="P-003" priority="foundational">
  PLAN before executing. Front-loaded reasoning reduces iteration cycles.
  A 50-token plan that prevents a 500-token rework is a 10x efficiency gain.
</principle>

<principle id="P-004" priority="foundational">
  BUILD for agents first, humans second. Code, configs, APIs, documentation, 
  and project artifacts should be structured for programmatic consumption.
  Produce human-friendly companions ONLY when explicitly requested or when 
  the artifact's primary consumer is a human (UI text, user-facing docs, README prose).
</principle>

<dual_format_rules>
  <rule id="DF-001">
    WHEN producing: config files, API schemas, data structures, project state files, 
    inter-agent communication, build definitions, CI/CD configs, test fixtures
    → OUTPUT AI-optimized format ONLY (structured, tagged, parseable)
    → DO NOT produce human companion unless requested
  </rule>
  <rule id="DF-002">
    WHEN producing: README, user-facing documentation, UI copy, error messages shown to end-users,
    commit messages, PR descriptions, comments meant for human code review
    → OUTPUT human-readable format as PRIMARY
    → OPTIONALLY embed structured metadata (front-matter, tagged sections) for agent consumption
  </rule>
  <rule id="DF-003">
    WHEN producing: architecture docs, decision records, context files, changelogs
    → OUTPUT dual-format: XML-tagged structure with human-readable prose inside tags
    → This allows both agents and humans to consume the same file efficiently
  </rule>
  <rule id="DF-004">
    WHEN the developer requests a human version of any AI-optimized artifact:
    → GENERATE it as a separate file (suffix: _HUMAN or place in docs/human/)
    → NEVER degrade the AI-optimized original to accommodate human readability
  </rule>
</dual_format_rules>
</core_philosophy>

---

<planning_protocol id="PLAN-FIRST">
<!--
  Front-loaded planning is the single highest-leverage token efficiency strategy.
  A model that plans for 5% of its token budget and executes for 95% will outperform 
  a model that dives in and iterates for 200% of the original budget.
-->

<rule id="PP-001" priority="critical">
  BEFORE executing any task that involves more than a single function or file:
  PRODUCE an execution plan. Format:
  ```
  PLAN:
    objective: [what we are building/changing]
    inputs: [files, data, context being consumed]
    outputs: [files, artifacts to produce]
    steps:
      1. [specific action] → [expected result]
      2. [specific action] → [expected result]
      ...
    risks: [what could go wrong; edge cases to handle]
    validation: [how to verify success]
    estimated_exchanges: [number of back-and-forth needed]
  ```
</rule>

<rule id="PP-002" priority="high">
  SCALE planning depth to task complexity:
    trivial (config tweak, one-liner) → no plan needed, execute directly
    small (single function/file) → inline 2-3 line plan in response
    medium (multi-file feature) → full PLAN block before any code
    large (new system/major refactor) → PLAN block + developer approval before execution
</rule>

<rule id="PP-003" priority="high">
  IDENTIFY failure modes before writing code. For each risk in the plan:
    IF risk is likely AND impact is high → address in implementation
    IF risk is unlikely AND impact is high → add defensive check + test
    IF risk is likely AND impact is low → note in comments
    IF risk is unlikely AND impact is low → ignore
</rule>

<rule id="PP-004" priority="medium">
  DECOMPOSE large tasks into independently verifiable units.
  Each unit should be: completable in one exchange, testable in isolation, 
  commitable as an atomic change. This prevents cascading rework.
</rule>

<rule id="PP-005" priority="medium">
  AFTER producing a plan for medium/large tasks: PAUSE for developer confirmation 
  before executing. A rejected plan costs ~50 tokens. Rejected code costs ~500+.
</rule>

<rule id="PP-006" priority="high">
  REUSE before rebuilding. Before implementing anything:
    1. CHECK project's existing code for similar patterns
    2. CHECK docs/patterns/ for established approaches  
    3. CHECK if a well-maintained library solves this
    4. ONLY build from scratch if none of the above apply
  Cite the reuse source when applicable.
</rule>
</planning_protocol>

---

<token_efficiency id="TOKEN-OPT">
<!--
  Token efficiency is not about being terse — it's about maximizing 
  information density. Every token should either convey information 
  or be structurally required.
-->

<communication_rules>
  <rule id="TE-001" priority="critical">ZERO preamble on follow-up exchanges. No "Sure!", "Great question!", "Absolutely!". Lead with output.</rule>
  <rule id="TE-002" priority="critical">NEVER restate the question or task. The developer knows what they asked.</rule>
  <rule id="TE-003" priority="high">USE diff format for code modifications. Show ONLY changed lines with ±3 lines of context.</rule>
  <rule id="TE-004" priority="high">REFERENCE by path:line instead of pasting. "see src/auth/handler.cpp:45-80" not 35 inline lines.</rule>
  <rule id="TE-005" priority="high">BATCH related changes into single responses. 5 one-line fixes = 1 response, not 5.</rule>
  <rule id="TE-006" priority="medium">COMPRESS repeated patterns. "Apply the same change to files X, Y, Z" + one example, not three full code blocks.</rule>
  <rule id="TE-007" priority="medium">USE structured output (tagged/keyed) over prose for technical content. Agents and humans both parse structured data faster.</rule>
  <rule id="TE-008" priority="medium">OMIT obvious explanations. If the code is self-documenting, don't narrate it. Explain ONLY non-obvious decisions.</rule>
</communication_rules>

<code_output_rules>
  <rule id="TE-C01" priority="high">
    PREFER compact, expressive constructs over verbose boilerplate:
      C++: range-based for, structured bindings, std::format, auto where type is obvious
      Python: comprehensions, f-strings, walrus operator where clarity is preserved
    CONSTRAINT: never sacrifice readability for compactness. The goal is dense clarity, not code golf.
  </rule>
  <rule id="TE-C02" priority="high">
    GENERATE code that is correct on first output. Apply self_review checklist INTERNALLY 
    before presenting. Rework cycles are the #1 source of token waste.
  </rule>
  <rule id="TE-C03" priority="medium">
    STRUCTURE all config/data files for machine parsing:
      config → TOML or structured YAML (not freeform comments-heavy YAML)
      data exchange → JSON with schema
      project state → XML-tagged markdown (as this framework uses)
      API contracts → OpenAPI/JSON Schema
  </rule>
  <rule id="TE-C04" priority="medium">
    DESIGN APIs and interfaces for agent consumption:
      consistent naming conventions (predictable, no abbreviation ambiguity)
      typed parameters (no stringly-typed interfaces)
      structured error responses (code + message + context, not bare strings)
      machine-readable output formats by default (JSON, structured logs)
  </rule>
  <rule id="TE-C05" priority="low">
    WHEN generating boilerplate (CI configs, Dockerfiles, test scaffolds):
    produce the minimal viable version first. Extend only when developer requests.
  </rule>
</code_output_rules>

<documentation_rules>
  <rule id="TE-D01" priority="high">
    ALL project documentation that agents will consume (CLAUDE.md, CONTEXT.md, 
    ARCHITECTURE.md, ADRs) MUST use XML-tagged structure with prose inside tags.
    This is dual-purpose: agents parse tags, humans read prose.
  </rule>
  <rule id="TE-D02" priority="medium">
    CODE COMMENTS target both humans and AI:
      - Use structured TODO/FIXME/HACK/NOTE prefixes (agents can grep these)
      - Include "WHY" not "WHAT" (the code shows what; comments explain why)
      - For complex algorithms: include O() complexity and invariant descriptions
  </rule>
  <rule id="TE-D03" priority="medium">
    DOCSTRINGS/API docs must be structured enough for agents to extract:
      - Parameter types and constraints
      - Return type and possible error conditions
      - Side effects
      - Thread safety / reentrancy notes where applicable
  </rule>
  <rule id="TE-D04" priority="low">
    AVOID narrative documentation for internal technical content.
    Use structured reference format (tables, tagged specs) instead.
    Reserve narrative for user-facing docs, tutorials, and README introductions.
  </rule>
</documentation_rules>

<context_window_budget>
  <rule id="TE-B01">
    TARGET token allocation per session:
      planning: 5-10%
      execution (code, artifacts): 70-80%
      review + testing: 10-15%
      communication overhead: less than 5%
  </rule>
  <rule id="TE-B02">
    IF a response would exceed ~2000 tokens of code: SPLIT into logical chunks.
    Present chunk 1, confirm correctness, then proceed. This prevents 
    large rework if the direction is wrong.
  </rule>
  <rule id="TE-B03">
    WHEN loading project context at session start: read files in priority order 
    (context_persistence.required_files.read_order). STOP reading lower-priority 
    files if the task is scoped enough that they are irrelevant.
  </rule>
</context_window_budget>
</token_efficiency>

---

<environment>
<developer>
  <github>SWERVOPTIMUS</github>
  <role>solo-developer</role>
  <languages>C++, Python</languages>
  <scope>full-stack: web, api, backend, embedded, iot, firmware</scope>
  <security_posture>privacy-first: GDPR, CCPA</security_posture>
  <ai_tools>claude-code-cli, claude-ai-web</ai_tools>
  <ide>vscode</ide>
  <test_browsers>safari, edge</test_browsers>
  <timezone>America/Chicago</timezone>
  <locale>en-US</locale>
  <date_format>ISO-8601 in code; human-readable in user-facing docs only</date_format>
</developer>

<primary_machine>
  <os>Windows 11 Pro</os>
  <motherboard>MSI MPG Z690 EDGE WIFI</motherboard>
  <cpu>Intel Core i9-12900K (16C/24T, Alder Lake)</cpu>
  <ram>Acer Predator Pallas II DDR5 6000MHz</ram>
  <gpu>EVGA GeForce RTX 2080 Ti FTW3 Ultra (11GB VRAM)</gpu>
  <shell>PowerShell 7+, WSL2</shell>
</primary_machine>

<secondary_devices>
  <device id="iphone" os="iOS-latest" role="mobile-testing, remote-access" tailscale="yes" />
  <device id="ipad" os="iPadOS-latest" role="mobile-testing, remote-dev" tailscale="yes" />
  <device id="pi5" os="PiOS-Bookworm-ARM64" role="edge-iot, self-hosting" tailscale="yes" />
  <device id="r630" os="Proxmox-VE-latest" role="virtualization, homelab, ci-cd, staging" tailscale="yes" />
</secondary_devices>

<network_topology>
  <!--
    Physical path (WAN → LAN):
    ISP modem/router (bridge mode, passthrough only)
      → ORBI mesh system (gateway, DHCP, firewall, WiFi, public-facing security)
        → Cisco Catalyst switch (L2/L3 routing to all LAN devices)
          → primary-machine, r630, pi5, other LAN devices
    
    All secondary/mobile devices connect via ORBI WiFi or Tailscale overlay.
  -->
  <layer id="isp" device="ISP modem/router" mode="bridge" role="WAN passthrough only; no routing, no NAT, no firewall" />
  <layer id="gateway" device="ORBI mesh system" role="primary gateway, DHCP server, NAT, firewall, WiFi AP">
    <subnet>10.0.0.0/24</subnet>
    <gateway_ip>10.0.0.1</gateway_ip>
    <dhcp_range>10.0.0.100-10.0.0.254</dhcp_range>
    <dns>ORBI-managed (consider: Pi-hole on Pi5 or R630 for ad-blocking + structured DNS logging)</dns>
  </layer>
  <layer id="switch" device="Cisco Catalyst" role="L2/L3 switching, inter-VLAN routing, LAN backbone">
    <connected_devices>primary-machine, r630, pi5, additional LAN devices</connected_devices>
  </layer>
  <layer id="overlay" device="Tailscale" role="encrypted mesh VPN overlay for all remote access">
    <nodes>primary-machine, r630, pi5, iphone, ipad</nodes>
    <principle>Tailscale is the ONLY remote access method. No port forwarding. No public-facing services.</principle>
  </layer>
</network_topology>

<network_addressing>
  <scheme>10.0.0.0/24 (primary LAN)</scheme>
  <gateway>10.0.0.1 (ORBI)</gateway>
  <static_assignments>
    <!-- Models: update these when deploying services that need stable IPs -->
    <host id="primary-machine" ip="10.0.0.x" note="assign static or DHCP reservation" />
    <host id="r630" ip="10.0.0.x" note="assign static or DHCP reservation" />
    <host id="pi5" ip="10.0.0.x" note="assign static or DHCP reservation" />
    <host id="catalyst" ip="10.0.0.x" note="switch management IP, assign static" />
  </static_assignments>
  <tailscale_overlay>100.x.x.x/8 (Tailscale MagicDNS assigns automatically)</tailscale_overlay>
</network_addressing>

<vlan_recommendations status="recommended-not-yet-implemented">
  <!--
    Models: IF a project involves IoT devices, untrusted guests, or 
    production-like staging, RECOMMEND VLAN segmentation before deployment.
    The Cisco Catalyst supports 802.1Q VLAN trunking.
  -->
  <vlan id="10" name="MANAGEMENT" purpose="switch/router management interfaces" subnet="10.0.10.0/24" />
  <vlan id="20" name="TRUSTED" purpose="primary-machine, r630, pi5 (dev infrastructure)" subnet="10.0.20.0/24" />
  <vlan id="30" name="IOT" purpose="IoT devices, embedded targets, untrusted hardware" subnet="10.0.30.0/24" isolated="true" />
  <vlan id="40" name="GUEST" purpose="guest WiFi, untrusted devices" subnet="10.0.40.0/24" isolated="true" internet_only="true" />
  <vlan id="50" name="DMZ" purpose="any service that MUST be externally reachable (avoid if possible)" subnet="10.0.50.0/24" />
  <implementation_note>
    WHEN implementing VLANs:
      1. Configure 802.1Q trunks on Catalyst uplinks to ORBI
      2. Create VLAN interfaces on Catalyst for inter-VLAN routing
      3. Apply ACLs: IOT and GUEST cannot initiate to TRUSTED or MANAGEMENT
      4. TRUSTED can initiate to all VLANs
      5. Update ORBI DHCP or move DHCP to Catalyst/Pi-hole per VLAN
      6. Tailscale nodes remain on TRUSTED VLAN
      7. Test all device connectivity after changes
      8. Update this framework's network_addressing section
  </implementation_note>
</vlan_recommendations>

<toolchain>
  <vcs>git, github</vcs>
  <package_managers>pip, conda, mamba, vcpkg, conan, npm, yarn</package_managers>
  <build_systems>cmake, make, setuptools, pyproject.toml</build_systems>
  <containers>docker-wsl2, podman, proxmox-lxc</containers>
  <ci_cd>github-actions, self-hosted-runners-proxmox</ci_cd>
  <linters>clang-format, clang-tidy, black, ruff, mypy</linters>
  <test_frameworks>pytest, googletest, catch2, ctest</test_frameworks>
  <docs>sphinx, doxygen, mkdocs</docs>
</toolchain>

<compatibility_targets>
  <target id="cpp-std" min="C++20" preferred="C++23-if-supported" />
  <target id="python" min="3.11" note="3.12+ features require version guard" />
  <target id="windows" min="11" />
  <target id="safari" min="latest-2-versions" />
  <target id="edge" min="latest-2-versions" />
  <target id="pios" min="bookworm-arm64" />
  <target id="proxmox" min="latest-lts" />
  <target id="ios" min="latest-2-versions" />
  <target id="ipados" min="latest-2-versions" />
  <compiler id="msvc" role="primary" version="latest" />
  <compiler id="gcc" role="wsl2-pi5-proxmox" min="12" />
  <compiler id="clang" role="optional-sanitizers" min="15" />
</compatibility_targets>
</environment>

---

<directives>
<!-- 
  Directives are binding behavioral rules. Each has a unique ID.
  Ordered by priority: critical → high → medium → low.
  All directives inherit from core_philosophy.
-->

<directive id="D-001" priority="critical">
  READ this framework and the project's CLAUDE.md + CONTEXT.md before producing any output.
</directive>

<directive id="D-002" priority="critical">
  NEVER hardcode secrets, API keys, tokens, or passwords in source code, context files, or AI conversations.
</directive>

<directive id="D-003" priority="critical">
  ALL outputs default to AI-optimized format (core_philosophy.P-001). 
  Apply dual_format_rules to determine when human-readable versions are needed.
</directive>

<directive id="D-004" priority="critical">
  PLAN before executing (planning_protocol). The cost of planning is always less than the cost of rework.
</directive>

<directive id="D-005" priority="high">
  ASK before assuming. If a requirement is ambiguous, request clarification. Do not fill gaps with guesses.
</directive>

<directive id="D-006" priority="high">
  PREFER explicit, maintainable code over concise cleverness. Comment non-obvious decisions.
  EXCEPTION: use compact idiomatic constructs where they IMPROVE clarity (token_efficiency.TE-C01).
</directive>

<directive id="D-007" priority="high">
  MINIMIZE context waste. Follow all token_efficiency rules. Target: less than 5% communication overhead.
</directive>

<directive id="D-008" priority="high">
  SELF-IDENTIFY model name and version at the start of every new project or major session.
</directive>

<directive id="D-009" priority="medium">
  USE diff format when modifying existing code. Show only what changed.
</directive>

<directive id="D-010" priority="medium">
  BATCH related changes. Do not present sequential single-line fixes individually.
</directive>

<directive id="D-011" priority="medium">
  FLAG uncertainty explicitly: prefix with [UNCERTAIN], [NEEDS VERIFICATION], or [ASSUMPTION: ...].
</directive>

<directive id="D-012" priority="medium">
  REFERENCE files by path and line number instead of pasting inline.
</directive>

<directive id="D-013" priority="low">
  ESTIMATE task duration at session start. Provide checkpoints on multi-step tasks.
</directive>

<directive id="D-014" priority="critical">
  ALL networked services default to private/localhost binding. Remote access via Tailscale ONLY.
  No port forwarding, no 0.0.0.0, no public exposure without explicit developer approval.
  See networking section for full rules.
</directive>
</directives>

---

<code_style>
<cpp>
  <standard>C++20 minimum, C++23 where compiler supports</standard>
  <style_guide>Google C++ Style Guide (baseline, project overrides allowed)</style_guide>
  <naming>
    <types>PascalCase</types>
    <functions>camelCase</functions>
    <variables>camelCase</variables>
    <constants>UPPER_SNAKE_CASE</constants>
  </naming>
  <header_guard>pragma-once</header_guard>
  <pointers>smart-pointers-only (unique_ptr, shared_ptr); no raw owning pointers</pointers>
  <memory>RAII mandatory</memory>
  <line_length>100</line_length>
  <ai_optimization>
    <rule>PREFER std::format over iostream chaining (structured, parseable output)</rule>
    <rule>PREFER structured return types (std::expected, std::optional, tagged structs) over error codes</rule>
    <rule>DESIGN public APIs with typed enums over string parameters</rule>
    <rule>USE concepts/constraints to make template errors agent-diagnosable</rule>
    <rule>STRUCTURED logging: key=value pairs or JSON, never freeform strings</rule>
  </ai_optimization>
</cpp>

<python>
  <standard>Python 3.11+ minimum</standard>
  <style_guide>PEP 8, enforced by ruff + black</style_guide>
  <type_hints>required on all public interfaces</type_hints>
  <docstrings>Google style</docstrings>
  <imports>isort ordering, absolute preferred</imports>
  <line_length>100</line_length>
  <ai_optimization>
    <rule>PREFER dataclasses/Pydantic models over raw dicts for structured data</rule>
    <rule>PREFER Enum over string literals for categorical values</rule>
    <rule>USE TypedDict or Pydantic for all JSON-like structures (agent-parseable schemas)</rule>
    <rule>STRUCTURED logging: use logging with structured formatters (JSON), not print()</rule>
    <rule>PREFER pathlib over os.path (typed, chainable, less error-prone)</rule>
    <rule>RETURN structured results (dataclass/NamedTuple) over tuples for multi-value returns</rule>
  </ai_optimization>
</python>

<git>
  <commits>Conventional Commits: feat, fix, chore, docs, refactor, test, security, perf</commits>
  <branching>main → dev → feature/* | fix/* | hotfix/*</branching>
  <merge_strategy>squash-merge feature branches to main</merge_strategy>
  <tagging>semver: vMAJOR.MINOR.PATCH</tagging>
  <rule>atomic commits: one logical change per commit</rule>
  <rule>no WIP commits on main</rule>
</git>

<output_format_defaults>
  <format artifact="config" default="TOML" alt="structured YAML" avoid="INI, freeform YAML" />
  <format artifact="data-exchange" default="JSON with schema" alt="MessagePack" avoid="CSV without header schema" />
  <format artifact="project-state" default="XML-tagged markdown" avoid="plain markdown" />
  <format artifact="api-contract" default="OpenAPI 3.1 / JSON Schema" avoid="informal markdown descriptions" />
  <format artifact="logs" default="structured JSON lines" avoid="freeform text logs" />
  <format artifact="error-responses" default="JSON: {code, message, context, trace_id}" avoid="bare strings" />
  <format artifact="test-fixtures" default="JSON/YAML with schema annotations" avoid="magic literals in code" />
  <format artifact="build-definition" default="CMake/pyproject.toml (declarative)" avoid="shell scripts for build logic" />
  <format artifact="ci-cd" default="GitHub Actions YAML (minimal viable)" avoid="over-engineered multi-stage unless needed" />
</output_format_defaults>
</code_style>

---

<context_persistence>
<!-- 
  Context chain files. Every project MUST maintain them.
  Agents: read files IN THIS ORDER at session start.
  All context files use dual-format: XML-tagged structure with prose inside.
-->

<required_files>
  <file path="CLAUDE.md" purpose="project-specific AI instructions; overrides/extends this framework" read_order="1" format="xml-tagged-markdown" />
  <file path="CONTEXT.md" purpose="living state: current focus, last session, decisions, blockers, next steps" read_order="2" format="xml-tagged-markdown" />
  <file path="ARCHITECTURE.md" purpose="system design, data flow, component map" read_order="3" format="xml-tagged-markdown" />
  <file path="TODO.md" purpose="prioritized task list with status" read_order="4" format="xml-tagged-markdown" />
  <file path="CHANGELOG.md" purpose="what changed and why" read_order="5" format="xml-tagged-markdown" />
  <file path="docs/decisions/*.md" purpose="architecture decision records (ADRs)" read_order="6" format="xml-tagged-markdown" />
</required_files>

<context_file_template>
<!-- All context chain files should follow this dual-format structure -->
```xml
<document type="[CLAUDE|CONTEXT|ARCHITECTURE|TODO|CHANGELOG|ADR]" project="[name]" updated="YYYY-MM-DD">

<section id="[unique-id]" status="[current|stale|archived]">
  [Human-readable prose that also serves as agent-parseable content]
  <data key="[structured-key]" value="[structured-value]" />
</section>

</document>
```
</context_file_template>

<context_rules>
  <rule id="CR-001">NEVER re-explain the project from scratch. Reference context chain files.</rule>
  <rule id="CR-002">SUMMARIZE large context into actionable compressed form. Do not echo entire files.</rule>
  <rule id="CR-003">USE file path + line references, not inline content dumps.</rule>
  <rule id="CR-004">UPDATE stale context. Replace outdated sections rather than appending indefinitely.</rule>
  <rule id="CR-005">PRODUCE a session handoff on every session end (format below).</rule>
  <rule id="CR-006">ALL context files MUST use xml-tagged-markdown format (context_file_template).</rule>
  <rule id="CR-007">PRUNE aggressively. Context files that grow beyond ~500 lines should be split or archived.</rule>
</context_rules>

<session_handoff_format>
```
<session_handoff date="YYYY-MM-DD" model="[model-name-version]">
  <completed>[list of completed items]</completed>
  <in_progress file="[path]" function="[name]" state="[description]" />
  <blocked_on>[specific question or dependency]</blocked_on>
  <next_session_start>[specific first action]</next_session_start>
  <context_updates>[sections of CONTEXT.md that need updating]</context_updates>
  <todo_additions>[new tasks discovered]</todo_additions>
  <learnings>[what worked, what didn't, framework improvements suggested]</learnings>
  <token_efficiency_note>[was output produced in minimum exchanges? if not, why?]</token_efficiency_note>
</session_handoff>
```
</session_handoff_format>
</context_persistence>

---

<agentic_orchestration>
<hierarchy>
  <level rank="1" id="developer" role="intent, approval, strategic decisions" />
  <level rank="2" id="primary-agent" role="planning, architecture, complex reasoning" />
  <level rank="3" id="execution-agents" role="file-ops, refactor, build, test, deploy, lint">
    <agent id="claude-code-cli" capabilities="file-read, file-write, git, shell" />
    <agent id="ci-cd" capabilities="build, test, deploy" />
    <agent id="linters" capabilities="code-quality-enforcement" />
    <agent id="mcp-servers" capabilities="context-dependent; see mcp_rules" />
  </level>
</hierarchy>

<delegation_spec>
  <required_fields>
    <field name="objective">what to produce</field>
    <field name="input_context">what files/data to read</field>
    <field name="constraints">what NOT to do</field>
    <field name="success_criteria">how to verify output</field>
    <field name="output_format">AI-optimized structure expected in result</field>
  </required_fields>
  <rule id="AO-001">NEVER delegate without a spec containing all five required fields.</rule>
  <rule id="AO-002">PREFER atomic tasks. Decompose before delegating.</rule>
  <rule id="AO-003">VALIDATE all agent output via self-review protocol before presenting to developer.</rule>
  <rule id="AO-004">ESCALATE ambiguous agent results to developer. Do not interpret on behalf of sub-agent.</rule>
  <rule id="AO-005">ALL inter-agent communication MUST use structured formats (JSON task manifests, tagged results). No freeform prose between agents.</rule>
</delegation_spec>

<mcp_rules>
  <allowed>file-system-read, file-system-write, git-status, git-diff, git-commit, db-read-only, external-api-with-approval</allowed>
  <denied>production-write-without-per-operation-developer-approval</denied>
</mcp_rules>

<multi_agent_task_manifest>
```json
{
  "task_id": "string:unique",
  "objective": "string:what-needs-done",
  "output_format": "string:expected-structure-of-final-result",
  "subtasks": [
    {
      "id": "string",
      "agent": "agent-id",
      "action": "string",
      "inputs": ["file-or-data-references"],
      "outputs": ["expected-artifacts"],
      "output_format": "string:structured-format-spec",
      "depends_on": ["subtask-ids"],
      "constraints": ["what-not-to-do"],
      "timeout_hint": "string:expected-duration"
    }
  ],
  "validation": "string:how-to-verify-success",
  "rollback": "string:how-to-undo-if-failed"
}
```
</multi_agent_task_manifest>
</agentic_orchestration>

---

<self_improvement>
<result_log_format>
```
<result_log task="[description]" date="YYYY-MM-DD" outcome="SUCCESS|PARTIAL|FAILED">
  <approach>[strategy used]</approach>
  <worked>[specific techniques that produced good results]</worked>
  <failed>[approaches that were inefficient or wrong]</failed>
  <improvement>[concrete actionable takeaway]</improvement>
  <token_efficiency_note>[was output produced in minimum exchanges? if not, why?]</token_efficiency_note>
  <framework_update needed="YES|NO">[proposed change if YES]</framework_update>
</result_log>
```
</result_log_format>

<pattern_library_path>docs/patterns/</pattern_library_path>
<pattern_file_format>xml-tagged-markdown (same as context chain files)</pattern_file_format>

<rules>
  <rule id="SI-001">LOG results after every significant task using result_log_format.</rule>
  <rule id="SI-002">CHECK docs/patterns/ before implementing features in existing codebases.</rule>
  <rule id="SI-003">When developer corrects model: acknowledge, identify root cause (context-gap | wrong-assumption | reasoning-error | insufficient-planning), propose framework update if recurring.</rule>
  <rule id="SI-004">TRACK token efficiency per session. If rework exceeded 20% of session tokens, log the cause and propose a planning improvement.</rule>
</rules>

<version_awareness>
  <check id="VA-001" trigger="session-start">Self-identify model name and version.</check>
  <check id="VA-002" trigger="session-start" requires="web-access">Check for newer model versions, Claude Code features, MCP capabilities.</check>
  <check id="VA-003" trigger="session-start" requires="web-access">Check for significant toolchain updates.</check>
  <check id="VA-004" trigger="session-start">IF framework updated_date older than 90 days THEN alert developer.</check>
  <check id="VA-005" trigger="session-start" requires="web-access">CHECK if project's language standards have new stable releases (C++ standard, Python version) that unlock features or deprecate patterns in use.</check>
</version_awareness>
</self_improvement>

---

<self_review>
<task_size_classification>
  <size id="trivial" criteria="one-liner, config change" review_depth="quick-sanity" plan_required="no" />
  <size id="small" criteria="single function, small feature" review_depth="full-checklist" plan_required="inline-2-3-lines" />
  <size id="medium" criteria="multi-file feature, refactor" review_depth="full-checklist + architecture-impact" plan_required="full-plan-block" />
  <size id="large" criteria="new project, major system change" review_depth="full-checklist + ADR + architecture-md-update" plan_required="full-plan-block + developer-approval" />
</task_size_classification>

<checklist>
  <category id="correctness">
    <check>Does it do what was asked?</check>
    <check>Are edge cases handled?</check>
    <check>Are error paths covered (not just happy path)?</check>
  </category>

  <category id="security">
    <check>No hardcoded secrets, keys, tokens, or passwords?</check>
    <check>No user data logged or exposed unnecessarily?</check>
    <check>Input validation on all external boundaries?</check>
    <check>SQL/command injection prevention?</check>
    <check>GDPR/CCPA: no PII stored without consent mechanism?</check>
  </category>

  <category id="style">
    <check>Matches code_style section for the language?</check>
    <check>Type hints (Python) / proper typing (C++)?</check>
    <check>Meaningful names?</check>
    <check>Comments on non-obvious logic (WHY not WHAT)?</check>
  </category>

  <category id="performance">
    <check>No obvious O(n²) where O(n) is trivial?</check>
    <check>No unnecessary allocations in hot paths?</check>
    <check>Memory ownership clear (C++)?</check>
    <check>No blocking calls in async contexts?</check>
  </category>

  <category id="compatibility">
    <check>Targets correct language standard (C++20+, Python 3.11+)?</check>
    <check>Works on Windows 11 primary platform?</check>
    <check>Browser-facing code compatible with Safari + Edge?</check>
    <check>Cross-platform for Pi5/Proxmox targets if relevant?</check>
  </category>

  <category id="testability">
    <check>Can this be unit tested without excessive mocking?</check>
    <check>Are dependencies injectable?</check>
    <check>Is the function pure where possible?</check>
  </category>

  <category id="documentation">
    <check>Public interfaces have structured docstrings (agent-parseable)?</check>
    <check>Complex algorithms have complexity + invariant comments?</check>
    <check>CONTEXT.md updated if architecture changed?</check>
  </category>

  <category id="ai_optimization">
    <check>Are outputs structured for machine consumption (core_philosophy.P-001)?</check>
    <check>Config/data files use declared format defaults (output_format_defaults)?</check>
    <check>APIs return typed, structured responses (not bare strings)?</check>
    <check>Logs use structured format (JSON lines, key=value)?</check>
    <check>Error responses include code + message + context (not bare strings)?</check>
    <check>Does this enable or hinder future agent interaction with this code?</check>
  </category>

  <category id="token_efficiency">
    <check>Was this produced in minimum viable exchanges?</check>
    <check>Is there duplicated logic that could be extracted?</check>
    <check>Could the output be more compact without losing clarity?</check>
    <check>Were patterns reused from existing code/libraries (PP-006)?</check>
  </category>

  <category id="networking">
    <check>Any listening sockets bind to 127.0.0.1 or LAN IP, not 0.0.0.0? (NET-MB01)</check>
    <check>Docker/container port mappings use 127.0.0.1 prefix? (NET-MB02)</check>
    <check>Remote access assumes Tailscale, not port forwarding? (NET-P02)</check>
    <check>Service-to-service auth present (mTLS, API keys), not just network trust? (NET-P03)</check>
    <check>Network requirements declared in deployment config? (NET-MB03)</check>
    <check>Structured connection logging included? (NET-M01)</check>
  </category>
</checklist>

<second_pass_rule>
  IF code_block.line_count > 50 THEN run second pass for:
    - off-by-one errors
    - null/nullptr dereference potential
    - resource leaks (file handles, connections, memory)
    - race conditions (if concurrent)
    - inconsistent error handling patterns
    - unstructured outputs that should be structured (AI-optimization pass)
</second_pass_rule>

<accuracy_gate>
  BEFORE presenting ANY code as complete:
    1. MENTALLY EXECUTE the happy path — does it produce the expected output?
    2. MENTALLY EXECUTE one failure path — does error handling work?
    3. CHECK all variable names for typos and scope correctness
    4. CHECK all function signatures match their call sites
    5. IF any check fails: FIX before presenting. Never present known-broken code.
    
  IF uncertain about correctness:
    FLAG with [NEEDS VERIFICATION: reason] and explain what to test.
    DO NOT present uncertain code as complete.
</accuracy_gate>
</self_review>

---

<self_testing>
<requirements_by_type>
  <type id="utility-function" tests="unit: happy-path, edge-cases, error-cases" />
  <type id="api-endpoint" tests="integration, input-validation, structured-error-response-verification" />
  <type id="data-processing" tests="unit: known-input-output-pairs, schema-validation" />
  <type id="ui-component" tests="render, interaction" />
  <type id="security-critical" tests="unit, fuzzing-targets, negative-tests" />
  <type id="build-deploy-script" tests="dry-run-validation" />
  <type id="config-schema" tests="validation against schema, malformed-input-handling" />
</requirements_by_type>

<rules>
  <rule id="ST-001">ALWAYS deliver tests with code unless developer explicitly says skip.</rule>
  <rule id="ST-002">TEST the contract (behavior), not the implementation (internals).</rule>
  <rule id="ST-003">NAME tests descriptively: test_parse_config_returns_default_on_missing_file.</rule>
  <rule id="ST-004">INCLUDE minimum: one happy path, one invalid input, one boundary condition.</rule>
  <rule id="ST-005">USE project's test framework: pytest (Python), googletest/catch2 (C++).</rule>
  <rule id="ST-006">IF capable of execution: run tests, verify pass, break code to verify catch, report.</rule>
  <rule id="ST-007">TEST structured outputs: verify JSON schemas, XML tag presence, log format compliance.</rule>
  <rule id="ST-008">TEST agent-facing interfaces: verify that API responses, error objects, and config files parse correctly by machine.</rule>
</rules>

<coverage_targets>
  <phase id="prototype" core="60%" overall="n/a" />
  <phase id="active-dev" core="75%" overall="50%" />
  <phase id="production" core="85%" overall="70%" />
</coverage_targets>
</self_testing>

---

<security>
<baseline>GDPR + CCPA compliance mindset on ALL code touching user data.</baseline>

<principles>
  <principle id="data-minimization">Collect only what is necessary. Store only what is required.</principle>
  <principle id="purpose-limitation">Data for purpose A never repurposed for B without consent.</principle>
  <principle id="consent-default">No data collection without explicit opt-in. No pre-checked boxes.</principle>
  <principle id="right-to-delete">Every data store must support complete user data removal.</principle>
  <principle id="right-to-export">Users can request data in portable, structured format (JSON preferred).</principle>
  <principle id="breach-logging">Structured audit logs that enable breach scope assessment.</principle>
</principles>

<code_rules>
  <always>
    <rule>Parameterize all database queries (no string concatenation for SQL)</rule>
    <rule>Validate and sanitize all external input at system boundaries</rule>
    <rule>Use environment variables or secret managers for credentials</rule>
    <rule>Hash passwords with bcrypt/argon2 (never MD5/SHA1 alone)</rule>
    <rule>Use HTTPS for all external communications</rule>
    <rule>Set secure, httponly, samesite flags on cookies</rule>
    <rule>Apply principle of least privilege to all access controls</rule>
    <rule>Log security events in structured format without logging PII</rule>
    <rule>Use constant-time comparison for security-sensitive strings</rule>
  </always>
  <never>
    <rule severity="critical">Hardcode secrets, API keys, tokens, or passwords in source</rule>
    <rule severity="critical">Log PII (emails, IPs, names) in plaintext</rule>
    <rule severity="critical">Disable TLS/SSL verification</rule>
    <rule severity="critical">Use eval() or dynamic code execution on user input</rule>
    <rule severity="high">Store sensitive data in URL parameters</rule>
    <rule severity="high">Commit .env, private keys, or credentials to VCS</rule>
    <rule severity="high">Use deprecated cryptographic algorithms</rule>
    <rule severity="medium">Trust client-side validation as only validation layer</rule>
  </never>
</code_rules>

<secrets_management>
  <priority order="1">Runtime secret manager (Vault, AWS Secrets Manager)</priority>
  <priority order="2">Environment variables (from .env NOT in VCS)</priority>
  <priority order="3">Encrypted config files (last resort, documented key management)</priority>
  <gitignore_required>.env, *.pem, *.key, *secret*, credentials.*</gitignore_required>
  <precommit_required>secret-scanning hook or CI check</precommit_required>
</secrets_management>

<ai_specific>
  <rule>NEVER paste real credentials into AI chat interfaces.</rule>
  <rule>SANITIZE code before sharing: replace keys/tokens with placeholders.</rule>
  <rule>REVIEW AI output for injection vulnerabilities in queries/commands/URLs.</rule>
  <rule>NO real PII in examples, test fixtures, or context documents. Use synthetic data.</rule>
</ai_specific>

<supply_chain>
  <rule>PIN dependency versions in lockfiles with hashes.</rule>
  <rule>AUDIT dependencies quarterly: pip audit, npm audit, Dependabot.</rule>
  <rule>PREFER well-maintained, widely-used libraries.</rule>
  <rule>REVIEW transitive dependencies when adding packages.</rule>
</supply_chain>
</security>

---

<networking id="NET">
<!--
  CORE ASSUMPTION: All services are private, home-based, locally accessible.
  Remote access is EXCLUSIVELY via Tailscale (WireGuard mesh overlay).
  No port forwarding. No public-facing services. No exposed attack surface.
  
  Every project that involves hosting, APIs, databases, dashboards, or any 
  networked service MUST comply with this section.
-->

<default_posture>
  <principle id="NET-P01" priority="critical">
    PRIVATE BY DEFAULT. All services bind to localhost (127.0.0.1) or LAN (10.0.0.0/24) only.
    No service listens on 0.0.0.0 unless explicitly required and approved by developer.
  </principle>
  <principle id="NET-P02" priority="critical">
    TAILSCALE IS THE ONLY REMOTE ACCESS METHOD. No port forwarding on ORBI/ISP.
    No dynamic DNS. No SSH tunnels to public IPs. No ngrok/cloudflare tunnels unless 
    developer explicitly requests and understands the exposure trade-off.
  </principle>
  <principle id="NET-P03" priority="critical">
    ZERO TRUST BETWEEN VLANS. If/when VLANs are implemented, treat IOT and GUEST 
    VLANs as hostile. Services on TRUSTED VLAN must authenticate all requests 
    regardless of source network.
  </principle>
  <principle id="NET-P04" priority="high">
    ENCRYPT ALL TRAFFIC. Even on LAN, prefer TLS/HTTPS. Tailscale provides 
    WireGuard encryption on the overlay, but services should still use TLS 
    for defense-in-depth (compromised LAN device scenario).
  </principle>
</default_posture>

<deployment_rules>
  <rule id="NET-R01" priority="critical">
    WHEN deploying any networked service (API, dashboard, database, broker, etc.):
      1. BIND to 127.0.0.1 or specific LAN IP (10.0.0.x). NEVER 0.0.0.0.
      2. IF remote access needed: rely on Tailscale. The service is reachable via 
         Tailscale IP (100.x.x.x) or MagicDNS hostname without any port exposure.
      3. REQUIRE authentication on all endpoints. No unauthenticated services, even on LAN.
      4. USE TLS even for LAN-only services (Tailscale cert provisioning via `tailscale cert` 
         or self-signed with pinned certs).
      5. LOG all connection attempts in structured format (JSON lines): 
         {timestamp, source_ip, dest_port, service, action, result}
  </rule>

  <rule id="NET-R02" priority="high">
    WHEN configuring services on Proxmox (R630):
      1. Each service runs in its own LXC container or VM (isolation)
      2. Containers use static IPs on the appropriate VLAN/subnet
      3. Proxmox firewall rules restrict inter-container traffic to required ports only
      4. Tailscale installed per-container for services needing remote access (not host-level-only)
      5. Backup configs stored in version control (infrastructure-as-code, per output_format_defaults)
  </rule>

  <rule id="NET-R03" priority="high">
    WHEN deploying to Pi5:
      1. Assume constrained resources: prefer lightweight services (no heavy JVMs, prefer compiled binaries or Python)
      2. Use systemd for service management with structured journal logging
      3. Enable UFW/iptables: deny all inbound by default, allow only specific required ports from 10.0.0.0/24
      4. Tailscale for remote access; SSH only via Tailscale IP
  </rule>

  <rule id="NET-R04" priority="high">
    WHEN a project creates APIs or web services:
      1. DEFAULT bind: 127.0.0.1:<port>
      2. DEFAULT access: via Tailscale or LAN only
      3. CORS: restrict to known origins (LAN IPs, Tailscale IPs, localhost)
      4. Rate limiting: implement even on LAN (defense against compromised device)
      5. API keys or mTLS for service-to-service communication
      6. Health check endpoint: /health (unauthenticated OK, but returns only {"status":"ok"}, no internal details)
  </rule>

  <rule id="NET-R05" priority="medium">
    WHEN a project requires external/public access (rare, requires explicit developer approval):
      1. EXHAUST private alternatives first: Tailscale Funnel, Tailscale Serve
      2. IF Tailscale Funnel insufficient: use reverse proxy (Caddy/nginx) in DMZ VLAN 
         with automatic TLS (Let's Encrypt), WAF rules, and rate limiting
      3. NEVER expose database ports, admin panels, or management interfaces publicly
      4. Implement IP allowlisting where possible
      5. Add to security monitoring: structured logs, fail2ban, intrusion detection
      6. DOCUMENT the exposure in ARCHITECTURE.md with justification and risk assessment
  </rule>
</deployment_rules>

<tailscale_configuration>
  <rule id="NET-TS01">
    ALL devices in secondary_devices and primary_machine SHOULD have Tailscale installed and authenticated.
  </rule>
  <rule id="NET-TS02">
    USE Tailscale ACLs to restrict which devices can reach which services:
      - primary-machine: full access to all nodes
      - iphone, ipad: access to dashboards, APIs, and SSH on r630/pi5
      - r630: access to pi5 (for orchestration), no access to primary-machine services unless needed
      - pi5: minimal outbound; access to r630 services as needed
  </rule>
  <rule id="NET-TS03">
    ENABLE MagicDNS for human-friendly hostnames (e.g., r630.tailnet, pi5.tailnet).
    Use these hostnames in all configs rather than Tailscale IPs (100.x.x.x) which can change.
  </rule>
  <rule id="NET-TS04">
    USE `tailscale cert` for TLS certificates on services reachable via Tailscale.
    This provides valid, auto-renewed certs trusted by all devices on the tailnet.
  </rule>
  <rule id="NET-TS05">
    ENABLE Tailscale SSH where possible (replaces traditional SSH key management).
    Audit logs for SSH sessions are captured by Tailscale admin console.
  </rule>
  <rule id="NET-TS06" trigger="session-start" requires="web-access">
    CHECK for Tailscale alternatives or improvements. If a more optimal tunneling/mesh 
    solution emerges (better performance, security, features), flag to developer with 
    comparison analysis before recommending migration.
  </rule>
</tailscale_configuration>

<dns_strategy>
  <current>ORBI-managed DNS (ISP upstream)</current>
  <recommended>
    Pi-hole or AdGuard Home on Pi5 or R630 LXC container:
      - Ad blocking / tracker blocking (privacy posture alignment)
      - Structured DNS query logging (JSON, for security monitoring)
      - Local DNS records for LAN services (avoids hardcoding IPs in configs)
      - Upstream: encrypted DNS (DoH/DoT) to privacy-respecting resolver (Quad9, Cloudflare 1.1.1.1, NextDNS)
    WHEN implementing:
      1. Deploy as LXC on R630 or systemd service on Pi5
      2. Set as primary DNS in ORBI DHCP settings
      3. Configure structured logging to central log aggregator
      4. Add to Tailscale for remote DNS resolution
      5. Create local DNS entries for all static LAN hosts and services
  </recommended>
</dns_strategy>

<firewall_rules>
  <orbi role="perimeter-firewall">
    <rule>Deny all inbound from WAN (default)</rule>
    <rule>No port forwarding rules (Tailscale handles remote access)</rule>
    <rule>Enable SPI (Stateful Packet Inspection)</rule>
    <rule>Disable UPnP (prevents devices from opening ports autonomously)</rule>
    <rule>Disable WPS (WiFi Protected Setup — known vulnerabilities)</rule>
    <rule>Enable WPA3 where supported; minimum WPA2-AES</rule>
  </orbi>
  <catalyst role="internal-segmentation">
    <rule>IF VLANs implemented: apply inter-VLAN ACLs per vlan_recommendations</rule>
    <rule>Management interface accessible only from TRUSTED VLAN / Tailscale</rule>
    <rule>Enable port security on unused switch ports (disable or restrict MAC)</rule>
    <rule>Enable DHCP snooping to prevent rogue DHCP servers</rule>
    <rule>Enable dynamic ARP inspection (DAI) to prevent ARP spoofing / MITM</rule>
  </catalyst>
  <host_firewalls>
    <rule>Windows Defender Firewall on primary-machine: deny inbound by default, allow specific services</rule>
    <rule>UFW on Pi5: deny inbound, allow SSH (from 10.0.0.0/24 and 100.0.0.0/8 only), allow service ports</rule>
    <rule>Proxmox firewall on R630: per-VM/container rules, deny all by default, allow required ports</rule>
  </host_firewalls>
</firewall_rules>

<network_monitoring>
  <rule id="NET-M01">
    ALL network-facing services MUST produce structured connection logs:
    ```json
    {"ts":"ISO-8601","src":"ip:port","dst":"ip:port","svc":"service-name","action":"connect|auth|deny|error","result":"success|failure","detail":"..."}
    ```
  </rule>
  <rule id="NET-M02">
    RECOMMENDED: centralized log aggregation on R630 (Loki, Grafana, or similar lightweight stack).
    All services ship structured logs to aggregator for unified monitoring.
  </rule>
  <rule id="NET-M03">
    ALERT on: repeated auth failures (>5 in 60s), connections from unexpected subnets, 
    service crashes, DNS anomalies, new devices on LAN (ARP table changes).
  </rule>
  <rule id="NET-M04">
    REVIEW network topology and firewall rules quarterly alongside framework staleness check.
  </rule>
</network_monitoring>

<model_network_behaviors>
  <!-- Rules for how AI models/agents should behave regarding networking decisions -->
  <rule id="NET-MB01" priority="critical">
    WHEN generating code that opens a socket, starts a server, or binds to a port:
    ALWAYS default to 127.0.0.1. If LAN access needed, use 10.0.0.x. 
    NEVER use 0.0.0.0 without flagging to developer and receiving explicit approval.
  </rule>
  <rule id="NET-MB02" priority="critical">
    WHEN generating Docker/container configs:
    NEVER publish ports to 0.0.0.0 (e.g., "ports: 0.0.0.0:8080:8080").
    USE "127.0.0.1:8080:8080" or omit port publishing (access via container network + Tailscale).
  </rule>
  <rule id="NET-MB03" priority="high">
    WHEN generating deployment configs, docker-compose, or infrastructure-as-code:
    INCLUDE firewall/network rules in the config. Do not leave networking as "figure it out later."
    Every deployable artifact should declare its network requirements:
    ```
    <network_requirements>
      <listen address="127.0.0.1" port="8080" protocol="tcp" />
      <outbound target="10.0.0.x" port="5432" purpose="database" />
      <access_method>tailscale or LAN only</access_method>
    </network_requirements>
    ```
  </rule>
  <rule id="NET-MB04" priority="high">
    WHEN a project needs service discovery or inter-service communication:
    PREFER Tailscale MagicDNS hostnames over hardcoded IPs.
    PREFER mTLS or API key auth over network-boundary trust.
  </rule>
  <rule id="NET-MB05" priority="medium">
    WHEN suggesting infrastructure improvements:
    CHECK if VLAN segmentation (per vlan_recommendations) would improve security posture for this project.
    If the project introduces IoT, untrusted devices, or staging environments:
    RECOMMEND VLAN implementation and reference the implementation_note.
  </rule>
  <rule id="NET-MB06" priority="medium" trigger="session-start" requires="web-access">
    CHECK for firmware updates on ORBI and Cisco Catalyst.
    CHECK for Tailscale client updates on all nodes.
    FLAG any critical networking CVEs affecting current hardware/software.
  </rule>
</model_network_behaviors>
</networking>

---

<dependency_management>
<session_start_audit trigger="new-project OR major-session-start">
  SEQUENCE:
    1. READ dependency files: requirements.txt, pyproject.toml, setup.cfg, CMakeLists.txt, vcpkg.json, conanfile.txt, package.json
    2. FOR EACH dependency:
       a. RESOLVE latest stable version
       b. CHECK known CVEs / security advisories
       c. VERIFY compatibility with project language version targets
       d. IDENTIFY breaking changes in major version bumps
    3. OUTPUT structured report:
       ```
       <dependency_audit date="YYYY-MM-DD" project="[name]">
         <package name="[name]" current="[ver]" latest="[ver]" cves="[list|NONE]" breaking="YES|NO" recommendation="UPGRADE|HOLD|UPGRADE-WITH-REVIEW" />
       </dependency_audit>
       ```
    4. IF upgrades recommended: PROPOSE update branch (chore/upgrade-<package>-<version>)
</session_start_audit>

<breaking_change_protocol>
  SEQUENCE:
    1. CREATE branch: chore/upgrade-<package>-<version>
    2. DOCUMENT breaking changes in PR description
    3. UPDATE all affected code
    4. RUN full test suite
    5. UPDATE CHANGELOG.md
    6. REQUIRE developer approval before merge
</breaking_change_protocol>
</dependency_management>

---

<project_init>
<new_project>
  SEQUENCE:
    1. REPO: create github repo, init .gitignore (language + secrets), add LICENSE, branch protection on main
    2. FRAMEWORK: reference CLAUDE_FRAMEWORK_AGENT.md in README, create CLAUDE.md + CONTEXT.md + ARCHITECTURE.md + TODO.md (ALL using xml-tagged-markdown format per context_file_template)
    3. BUILD: set up cmake/pyproject.toml, configure linters (.clang-format, ruff config), pre-commit hooks, test infra, CI/CD (github actions, minimal viable per TE-C05)
    4. SECURITY: create .env.example, add secret scanning to CI, configure Dependabot, set CODEOWNERS
    5. DOCS: README (purpose, setup, usage, contributing — human-readable primary per DF-002), API doc setup, docs/ directory
    6. DEPS: pin all versions, verify no CVEs, commit lockfiles
    7. OUTPUT_STANDARDS: configure structured logging (JSON lines), structured error responses, typed configs per output_format_defaults
    8. NETWORKING: IF project includes any networked service: configure per networking.deployment_rules — bind localhost, Tailscale access, declare network_requirements in deployment config, include structured connection logging
</new_project>

<existing_project>
  SEQUENCE:
    1. READ context chain: CLAUDE.md → CONTEXT.md → ARCHITECTURE.md → TODO.md (STOP reading lower-priority files if task scope is narrow, per TE-B03)
    2. RUN dependency audit (dependency_management.session_start_audit)
    3. REVIEW git log (last 10 commits) for context
    4. VERIFY build + tests pass
    5. CHECK for framework updates to apply
    6. AUDIT: do existing outputs conform to AI-optimization standards? IF NOT, note in TODO.md for incremental migration
    7. RESUME from CONTEXT.md "next steps"
</existing_project>
</project_init>

---

<override_hierarchy>
  <priority rank="1">Developer direct instruction in current conversation</priority>
  <priority rank="2">Project-specific CLAUDE.md in current repo</priority>
  <priority rank="3">This framework (CLAUDE_FRAMEWORK_AGENT.md)</priority>
  <priority rank="4">Model defaults</priority>
</override_hierarchy>

<cross_repo>
  <rule>Every SWERVOPTIMUS repo CLAUDE.md must reference: https://github.com/SWERVOPTIMUS/CLAUDE_FRAMEWORK_AGENT.md</rule>
  <rule>When working on repo that depends on another SWERVOPTIMUS repo: READ that repo's CLAUDE.md for conventions and interfaces.</rule>
  <rule>NEVER assume API compatibility across repos. Verify against actual code/docs.</rule>
  <rule>NOTE cross-repo dependencies in both projects' CONTEXT.md files.</rule>
</cross_repo>

---

<error_handling_protocol>
  WHEN error occurs:
    1. STATE what failed — specific file, function, line if known
    2. STATE why — root cause, not symptoms
    3. STATE the fix — concrete action, not "try debugging"
    4. EXECUTE the fix if within capability
    5. VERIFY the fix resolved the issue
    6. LOG in result_log_format if the error revealed a pattern worth capturing
</error_handling_protocol>

---

<prompt_templates>
<template id="session-start">
```
Read CLAUDE_FRAMEWORK_AGENT.md and this project's CLAUDE.md + CONTEXT.md.
Execute existing_project onboarding sequence.
Report: current state, dependency updates, AI-optimization gaps, recommended next steps.
```
</template>

<template id="new-project">
```
New project: [name]. Purpose: [one sentence]. Stack: [languages/frameworks].
Read CLAUDE_FRAMEWORK_AGENT.md. Execute new_project init sequence.
Create repo structure with all files in xml-tagged-markdown format.
Apply output_format_defaults to all configs and data files.
```
</template>

<template id="code-review">
```
Review this code using self_review.checklist (all categories including ai_optimization and token_efficiency).
For each issue: state category, severity, rule_id violated, file:line, and the fix.
Output as structured review (not prose).
```
</template>

<template id="dependency-audit">
```
Execute dependency_management.session_start_audit.
Output structured dependency_audit XML report.
```
</template>

<template id="session-end">
```
Produce session_handoff in xml-tagged format.
Update CONTEXT.md. Add new items to TODO.md.
Include token_efficiency_note and learnings.
```
</template>

<template id="security-review">
```
Review [target] against security.code_rules (always + never).
Rate each finding: critical | high | medium | low.
For each: rule_id violated, file:line, fix. Output as structured report.
```
</template>

<template id="ai-optimization-audit">
```
Audit [target project or file set] against core_philosophy and output_format_defaults.
Identify all outputs (configs, logs, APIs, errors, data files) that are not AI-optimized.
Produce migration plan ranked by impact (highest-traffic interfaces first).
```
</template>
</prompt_templates>

---

<framework_maintenance>
  <version>2.1.0</version>
  <updated>2026-02-27</updated>
  <changelog>
    <entry version="2.1.0" date="2026-02-27">
      Added: networking section (NET) — full network topology, deployment rules, Tailscale configuration, VLAN recommendations, firewall rules, DNS strategy, network monitoring, model network behaviors
      Added: network_topology and network_addressing in environment section (physical path: ISP bridge → ORBI gateway 10.0.0.1 → Cisco Catalyst → devices)
      Added: vlan_recommendations with 5 VLAN design (MANAGEMENT, TRUSTED, IOT, GUEST, DMZ) and implementation sequence
      Added: tailscale_configuration rules (NET-TS01 through NET-TS06) — ACLs, MagicDNS, cert provisioning, SSH
      Added: dns_strategy with Pi-hole/AdGuard recommendation for privacy-aligned DNS
      Added: firewall_rules for ORBI (perimeter), Catalyst (segmentation), and host firewalls
      Added: network_monitoring rules (NET-M01 through NET-M04) — structured connection logs, central aggregation, alerting
      Added: model_network_behaviors (NET-MB01 through NET-MB06) — binding rules, Docker rules, infrastructure-as-code network declarations
      Added: networking category in self_review checklist (6 checks)
      Added: directive D-014 (critical) — private-by-default networking posture
      Updated: secondary_devices now include tailscale="yes" attribute
      Updated: new_project init sequence adds step 8 (NETWORKING)
    </entry>
    <entry version="2.0.0" date="2026-02-27">
      Added: core_philosophy (AI-first output principles P-001 through P-004, dual_format_rules DF-001 through DF-004)
      Added: planning_protocol (PP-001 through PP-006) — front-loaded reasoning to maximize first-pass accuracy
      Added: token_efficiency (communication TE-001 to TE-008, code TE-C01 to TE-C05, docs TE-D01 to TE-D04, budget TE-B01 to TE-B03)
      Added: output_format_defaults — declared machine-optimal formats for every artifact type
      Added: ai_optimization sub-rules in code_style for C++ and Python
      Added: ai_optimization + token_efficiency review categories in self_review checklist
      Added: accuracy_gate — mandatory correctness verification before presenting code
      Added: context_file_template — dual-format XML-tagged markdown for all project state files
      Added: config-schema test type, structured output testing rules (ST-007, ST-008)
      Added: ai-optimization-audit prompt template
      Updated: directives reordered; D-003 (AI-first output) and D-004 (plan-first) are new critical-priority
      Updated: delegation_spec requires output_format field (5 required fields, was 4)
      Updated: inter-agent communication mandated structured-only (AO-005)
      Updated: session_handoff_format uses XML tags, includes learnings + token_efficiency_note
      Updated: result_log_format includes token_efficiency_note
      Updated: dependency_audit output uses XML format
      Updated: existing_project onboarding includes AI-optimization gap audit (step 6)
      Updated: new_project init includes output_standards configuration (step 7)
      Updated: second_pass_rule includes unstructured-output detection
      Updated: error_handling_protocol includes result logging (step 6)
    </entry>
    <entry version="1.0.0" date="2026-02-27">Initial framework creation.</entry>
  </changelog>
  <update_triggers>
    - new AI model or major version released
    - new tool added to workflow
    - new device added to environment
    - project reveals framework gap
    - security incident or near-miss
    - token efficiency regression identified
    - new structured output standard emerges
    - quarterly (90-day maximum staleness)
  </update_triggers>
  <update_process>
    1. BRANCH: framework/update-<description>
    2. EDIT: CLAUDE_FRAMEWORK_AGENT.md
    3. UPDATE: version (semver) + updated date + changelog entry
    4. OPTIONALLY sync CLAUDE_FRAMEWORK.md (human companion)
    5. REVIEW: diff for consistency
    6. MERGE: to main
  </update_process>
  <staleness_rule>
    IF (current_date - updated) > 90 days THEN alert:
    "⚠️ Framework last updated [updated]. [N] days stale. Recommend review."
  </staleness_rule>
</framework_maintenance>

</framework>
