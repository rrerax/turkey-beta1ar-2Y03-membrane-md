# Turkey β1-AR membrane MD of deposited isoprenaline (PDB 2Y03)

Three 50 ns OpenMM membrane replicates of the deposited isoprenaline complex in turkey β1-adrenergic receptor. This is not the human β2-adrenergic receptor docking screen. That screen is a separate repository: [rrerax/Beta2AR_SupervisedEnOpt_Screening](https://github.com/rrerax/Beta2AR_SupervisedEnOpt_Screening).

The receptor is the OPM-oriented chain A of PDB 2Y03. It is turkey β1-AR, not human β2-AR. The ligand is the deposited 5FW (isoprenaline). No docked pose is the starting coordinate. CHEMBL776 is not in the system.

## What was run

Each replicate starts from the same minimised coordinates and uses its own velocity seed: 20261026, 20261027, and 20261028. The engine is OpenMM. Protein Amber14 ff14SB, lipid Lipid17, membrane POPC, TIP3P, 0.15 mol/L NaCl. Hydrogen mass is 1.5 u and the step is 4 fs.

`run_plan.json` was written before any contact was counted. The benchmark was about 69 ns/day, so each production run is 50 ns. Trajectories, checkpoints, and the solvated system are not in this repository.

The contact rule was written before production. A hydrogen bond on a frame is a hydrogen–acceptor distance of at most 2.5 Å and a donor–hydrogen–acceptor angle of at least 120°. A contact persists when its occupancy in the production frames is at least 0.20 in at least two of the three replicates. Equilibration frames are not counted. A flat RMSD is not an affinity, and a ligand that stays in the pocket is not an affinity.

## Result

From `contact_result.json`:

| Contact | Occupancy, three seeds | Persists |
|---|---|---|
| Amine hydrogen to Asp121 | 0.346, 0.327, 0.607 | yes |
| Phenolic OAB to Ser211 | 0.233, 0.243, 0.004 | yes |
| Phenolic OAC to Ser215 | 0.227, 0.327, 0.266 | yes |
| Bridging water at each of those three pairs | all at or below 0.058 | no |

The amine–aspartate hydrogen bond and both phenolic hydrogen bonds meet the pre-written rule. In the third replicate, OAB–Ser211 is essentially absent (occupancy 0.0036). Bridging waters do not persist. This describes contact variability across replicates. It does not rank ligands, it does not compare CHEMBL776 with CHEMBL434, and it does not explain a docking score.

## Files

- `PROTOCOL.md` — settings and the contact rule, written before production.
- `what_50ns_does_not_show.md` — what this length cannot support, and the numbers above.
- `prepare_system.py`, `run_replicates.py`, `analyze_contacts.py` — build, run, and count.
- `run_plan.json`, `contact_result.json`, `contacts_three_replicates.png` — the plan fixed before the count, and the count.
