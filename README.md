# Bacterial GC Content & ORF Finder (demo)

A small, beginner-friendly bioinformatics project. It reads DNA sequences from a FASTA file, calculates the GC content of each sequence, finds open reading frames (ORFs) in all six reading frames, and saves a summary table and a chart.

> **Note:** the demo data is **simulated** (random sequences with GC content similar to well-known bacterial genera). It is not real genome data. To run a real analysis, replace `data/demo_sequences.fasta` with genomes downloaded from [NCBI](https://www.ncbi.nlm.nih.gov/datasets/genome/).

## What it does

1. Reads sequences from a FASTA file
2. Calculates the length and GC content of each sequence
3. Finds ORFs (ATG to stop codon) of at least a minimum length, on both DNA strands
4. Saves `results/sequence_summary.csv` and `results/gc_content_and_orfs.png`

## Results (demo data)

![GC content and ORFs](results/gc_content_and_orfs.png)

| Sequence | Length (bp) | GC (%) | ORFs ≥ 300 bp | Longest ORF (bp) |
|---|---|---|---|---|
| Sim_Staphylococcus_like | 30,000 | 33.03 | 0 | 0 |
| Sim_Escherichia_like | 30,000 | 50.92 | 5 | 435 |
| Sim_Klebsiella_like | 30,000 | 57.13 | 15 | 654 |
| Sim_Pseudomonas_like | 30,000 | 65.73 | 39 | 864 |
| Sim_Streptomyces_like | 30,000 | 72.23 | 61 | 1,395 |

## How to run

Tested with Python 3.12.

```bash
git clone https://github.com/<your-username>/bacterial-gc-orf-demo.git
cd bacterial-gc-orf-demo
pip install -r requirements.txt

# optional: regenerate the simulated data
python scripts/make_demo_data.py

# run the analysis
python scripts/analyze.py --input data/demo_sequences.fasta --outdir results --min-orf 300
```

Options for `analyze.py`:

| Option | Default | Meaning |
|---|---|---|
| `--input` | `data/demo_sequences.fasta` | FASTA file to analyze |
| `--outdir` | `results` | Folder for the output files |
| `--min-orf` | `300` | Minimum ORF length in bp |

## Project structure

```
bacterial-gc-orf-demo/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   └── demo_sequences.fasta      # simulated input sequences
├── scripts/
│   ├── make_demo_data.py         # generates the simulated data
│   └── analyze.py                # GC content + ORF finder
└── results/
    ├── sequence_summary.csv      # summary table
    └── gc_content_and_orfs.png   # chart
```

## Notes and limitations

- GC content varies a lot between bacterial groups, from roughly 33% in *Staphylococcus* to roughly 72% in *Streptomyces*. The simulated sequences are built to mimic this.
- The ORF counts rise with GC content even though these sequences are random. The stop codons (TAA, TAG, TGA) are AT-rich, so in high-GC sequences stops are rarer and long ORFs appear by chance. This finder only looks for ATG-to-stop stretches, so it cannot tell real genes from chance ORFs. Dedicated gene finders such as [Prodigal](https://github.com/hyattpd/Prodigal) use statistical models to deal with this.

## Ideas for next steps

- Run it on real genomes downloaded from NCBI
- Add a sliding-window GC plot along each sequence
- Parse FASTA with Biopython instead of the hand-written parser
- Add tests for `gc_percent`, `reverse_complement` and `find_orfs`
- Compare the ORFs found here with Prodigal's predictions

## Author, Seeker,learning bioinformatics
