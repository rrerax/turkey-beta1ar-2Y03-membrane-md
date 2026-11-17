# Reference membrane dynamics of deposited isoprenaline in PDB 2Y03

Written before any production trajectory. This is a reference simulation of the crystal complex itself. It is not a continuation of docking. CHEMBL776 is not added. The 3SN6 docking pose is not used, and neither is the manually rotated serine frame.

## Starting point

- Structure: OPM-oriented 2Y03, chain A. Turkey β1 receptor, not human β2. The stabilising mutations are kept: R68S, M90V, C116L, Y227A, A282L, F327A, F338M, and C358A.
- Residues already deleted in the construct are not rebuilt: N-terminus 3–32 and ICL3 244–271. Residues the crystal did not locate are not rebuilt either: 240–243, 272–277, and everything after 359, including the His tag. NME is added after 239 and ACE is added before 278. The N-terminus of residue 33 and the C-terminus of residue 358 stay charged termini.
- Disulfides are only the pairs written in the crystal: Cys114–Cys199 and Cys192–Cys198.
- The ligand is 5FW from that crystal. Chirality, heavy-atom coordinates, and protonation come from the deposited structure: the secondary amine carries one positive charge, and the two phenolic hydroxyls and the benzylic hydroxyl stay neutral. Charges use OpenFF `openff-gnn-am1bcc-1.0.0`, because this machine has no AmberTools.
- Crystal waters are not kept. The nearest water is about 9 Å from the ligand. There is no bridging water.
- The membrane is POPC, TIP3P, NaCl 0.15 mol/L, neutralised. Protein Amber14 ff14SB. Lipid Lipid17. The membrane centre is at z = 0. The box edge is at least 1 nm from the solute.
- Hydrogen mass is repartitioned to 1.5 u. The step is 4 fs. Temperature is 300 K. The membrane barostat is isotropic in XY and free in Z, with surface tension 0.

## Contacts specified beforehand

A hydrogen bond on a frame has a hydrogen–acceptor distance of at most 2.5 Å and a donor–hydrogen–acceptor angle of at least 120°.

- An amine hydrogen to OD1 or OD2 of Asp121.
- The phenolic hydrogen named OAB in the crystal to OG of Ser211. Ser211 corresponds to Ser203 of human β2.
- The phenolic hydrogen named OAC in the crystal to OG of Ser215. Ser215 corresponds to Ser207.
- Water between these three pairs: the water oxygen is within 3.2 Å of both the ligand N or phenolic oxygen and the matching OD or OG. Water is recorded only. It cannot turn a hydrogen bond that did not form into a contact that is still present.
- The Ser212 distance and the benzylic oxygen OAA to Asp121 OD1 are recorded only. They do not decide whether a contact persists.

A contact “persists” when its occupancy in the production frames of a replicate is at least 0.20, and at least two of the three replicates meet that bar. Equilibration frames are not counted. A flat RMSD along one trajectory is not an affinity. A ligand that remains in the pocket is not an affinity.

## Simulation

Each replicate starts from the same energy-minimised coordinates, equilibrates with its own velocity seed, and then runs production. The seeds are 20261026, 20261027, and 20261028.

Equilibration is NPT: 125 ps (restraint 500 kJ/mol/nm²), 125 ps (500), 250 ps (100), 250 ps (25), and 500 ps (0). The restraint is on protein CA atoms and on the heavy atoms of the ligand.

At the end of equilibration, if the CA RMSD of the transmembrane core (residues 43–229 and 288–348) from the crystal exceeds 0.30 nm, that replicate stops. It does not enter production and it is not written as a contact result. This is a check that the system has broken. It is not the contact threshold.

Production length is set by speed and is written to `run_plan.json` before any contact is counted. After warm-up, 5,000 steps are timed. At or above 40 ns/day, each replicate is 50 ns. At or above 15 ns/day, 20 ns. Slower than that, 10 ns. After that file is written, the length is not changed because a contact breaks or holds.

## What this sampling time cannot show

Ten to fifty nanoseconds can only show whether these three crystal hydrogen bonds are still occupied during that thermal motion. They cannot give an affinity, a residence time, or ligand departure. They cannot compare 434 with 776, they cannot show what a mutation would do, and they cannot explain why a docking score rejects a geometry that is otherwise correct.
