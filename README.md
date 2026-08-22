# GSoC 2026 — Using AI to Improve Open-Source IP

**Contributor:** Muhammad Rayyan Khan  
**Organization:** FOSSi Foundation / Redwood EDA  
**Mentor:** Steve Hoover  
**Project:** Strengthening the LLM-driven Verilog → TL-Verilog conversion pipeline  

## Project Summary

Millions of lines of open-source Verilog exist but remain verbose and 
hard to maintain. TL-Verilog offers a cleaner abstraction, reducing 
logic code by ~50%. This project strengthens the LLM-driven conversion 
pipeline by improving error recovery, documenting failure patterns, 
and applying the flow to real open-source RISC-V modules — all with 
formal equivalence verification (EQY + SymbiYosys) at every step.

## Contributions

### Pull Requests to stevehoover/LLM_TLV

| PR | Description | Status |
|---|---|---|
| [#21](https://github.com/stevehoover/LLM_TLV/pull/21) | Generic `convert.sh` wrapper for non-interactive conversion | Open |
| [#22](https://github.com/stevehoover/LLM_TLV/pull/22) | Stage/transaction alignment and FEV failure/output-remapping guidance | Open |
| [#23](https://github.com/stevehoover/LLM_TLV/pull/23) | Clarify clock-bridge timing for registered TL-Verilog signals | Open |

### Module Conversions

| Repo | Modules Converted | Status |
|---|---|---|
| [stevehoover/SERV](https://github.com/KhanRayyan3622/stevehoover-serv) | serv_state_cleanroom (34 checkpoints, 6 FEV configs), serv_alu, serv_bufreg, serv_bufreg2 | FEV verified |
| [Cores-VeeR-EL2](https://github.com/KhanRayyan3622/Cores-VeeR-EL2) | dmi_mux (full), el2_prim_generic_buf (full), dmi_jtag_to_core_sync (partial) | FEV verified / documented |
| [Cores-VeeR-EH1](https://github.com/KhanRayyan3622/Cores-VeeR-EH1) | dmi_jtag_to_core_sync (partial), dec_gpr_ctl, dec_trigger | FEV verified / documented |
| [kronos](https://github.com/KhanRayyan3622/kronos) | input_debouncer (in progress) | In progress |
| [ibex](https://github.com/KhanRayyan3622/ibex) | ibex_register_file_ff (partial) | Documented |
| [scr1](https://github.com/KhanRayyan3622/scr1) | scr1_tapc_shift_reg (partial) | Documented |

### Other Deliverables

| Item | Link |
|---|---|
| Training data (failure/fix pairs) | [KhanRayyan3622/training-data](https://github.com/KhanRayyan3622/training-data) |
| MakerCode RTL Challenge (101 questions) | [KhanRayyan3622/MakerCode_RTLChallenge](https://github.com/KhanRayyan3622/MakerCode_RTLChallenge) |
| GSoC Midterm deliverables | [KhanRayyan3622/gsoc-midterm](https://github.com/KhanRayyan3622/gsoc-midterm) |
| Blog post | [Teaching AI to Convert Verilog to TL-Verilog](https://medium.com/@khan.rayyan3622/teaching-ai-to-speak-tl-verilog-a-gsoc-2026-field-report-d4f3dc5a8c87) |

## Key Findings

1. **Stage alignment** — EQY match lines for TLV pipesignals require explicit `<>0` suffix
2. **Registered output conflict** — Outputs that are also flip-flops create EQY partition conflicts; keep flop gold-named in `\SV_plus`
3. **FEV configuration validation** — Malformed `M5_configs` can cause configuration coverage problems; upstream `fev.sh` has since been updated to fail loudly rather than silently degrade.
4. **Async reset is a structural limit** — TLV is synchronous only; modules with async reset need `\SV_plus` for the flops
5. **Cost routing** — Per-task routing with escalation: ~60¢ vs $8.22 monolithic (Ha's finding)

## Team

- **Steve Hoover** — TL-Verilog creator, mentor, conversion recipe author
- **Ha Le Van Thien** — Conversion Console, model comparison experiments, oversight judge, prompt caching
- **Arya K. Sekhar Das** — VS Code extension, n8n workflow automation
