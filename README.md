# assayBLAST - for *in silico* analysis of PCR oligos

This tool provides in silico predictions of microarray hybridization results, calculating the expected binding interactions between query DNA sequences (primers and probes) and a genome database. Based on the BLAST hits and their mismatch numbers, the functionality of the primers and probes can be estimated. The tool also checks the strand specificity of the primers and probes.

The tool has been evaluated and was published at MDPI applied biosciences (https://doi.org/10.3390/applbiosci4020018)



## Key Features
- BLAST Database Creation: Automatically generates a custom BLAST database from user-provided genome files.
- BLAST Search Execution: Runs forward and reverse complement BLAST searches to evaluate how primers or probes interact with both DNA strands.
- Mismatch Analysis: Identifies and filters matches with acceptable levels of mismatches, simulating how primers perform in the presence of mutations or single nucleotide polymorphisms (SNPs).
- Melting Temperature (Tm) Calculation: Computes the melting temperature of sequences using SantaLucia's thermodynamic nearest-neighbor model.
- Off-Target Detection: Helps identify off-target binding sites to ensure primer/probe specificity.
- Support for Multiple Genomes: Capable of performing searches across multiple genome sequences, ideal for comparative genomics or multi-strain analyses.
- Results in Multiple Formats: Outputs detailed results in XML, TSV, and text formats for easy review and further analysis.

## Python Requirements
- Python 3.7+
- Biopython
- NCBI BLAST+

## Usage
To run the tool, download the assayBLAST.py file and run it with python as explained below.
### Command-Line Arguments

```bash
python assayBLAST.py -g <"genome_files_glob_pattern.fasta"> -q <query_file.fasta> [options]
```
#### Required Arguments
- `-g, --genome`: Glob pattern for the genome FASTA files in "".
- `-q, --queries`: Path to the query FASTA file containing primers or probes.

#### Optional Arguments
- `-d, --db_name`: Name of the BLAST database (default: `genome_db`).
- `-o, --output`: Output file name for BLAST results (default: `blast_results.xml`).
- `-c, --tsv_output`: Output file for mismatch matrix in TSV format (default: `blast_results.tsv`).
- `-a, --alignments_output`: Output file for alignments with mismatches (default: `alignments.txt`).
- `-mh, --multi_hits_output`: Output file for multi-hit BLAST results (default: `multi_hits.txt`).
- `-db, --db_dir`: Directory to store BLAST database files (default: `blast_db`).
- `-m, --max_mismatches`: Maximum number of allowed mismatches in BLAST alignments (default: `4`).
- `-cc, --concatenate`: Concatenate input sequences into one (default: `False`).
- `-k, --keep_blast_db`: Keep the previously created BLAST database (default: `False`).

#### Example

```bash
python assayBLAST.py --genome "example_database.fasta" --queries example_queries.fasta --max_mismatches 2
```
This command:
- Uses the FASTA files `example_database.fasta` to build the BLAST database.
- Runs the BLAST search using the primers/probes in `example_queries.fasta`.
- Allows a maximum of 2 mismatches in alignments.
- The --concatenate parameter is not used, thus every entry in the `example_database.fasta` is a possible target in the output files.

The following table displays the output of the blast_result.tsv file generated from the example data. The table's header contains the names of the provided query oligonucleotides, each listed twice: once for the forward BLAST search (_forward) and once for the reverse complementary sequence used in the reverse BLAST search (_revcomp).
Below the oligo names, the table includes the corresponding query sequences, followed by their melting temperatures (Tm) calculated using the Santa Lucia formula. Next, the table presents the total number of BLAST hits, categorized by mismatch count (e.g., mm=0, mm=1, ..., mm=--max_mismatches).
Further down, the results show hits against each entry in the provided target sequences (example_database.fasta). If a query sequence matches a target sequence, it is listed along with the mismatch count and the start and end positions within the target sequence. If no match is found, no entry appears for that query in the respective target sequence.
For example, the first oligo, primer_lukF_11b_forward, did not produce any hits in the _forward search but was detected as a perfect match (0 mismatches) in the _revcomp search at position 1942449-1942466. Meanwhile, the corresponding probe, probe_lukF_10_forward, was detected on the forward strand. Since the primer was designed for linear amplification without a reverse primer, it must be located on the opposite strand relative to the probe. This results in a characteristic table pattern where primers and probes either appear adjacent to each other or two columns apart, making it easy to identify errors in strand orientation for primer/probe pairs.
![grafik](https://github.com/user-attachments/assets/ace37924-becc-457c-b958-c08dbf42b288)



## Outputs
- **BLAST XML Results**: Detailed output of BLAST alignments including scores, E-values, mismatches, and alignments.
- **TSV Output**: Summary table containing mismatch counts and melting temperatures for each query-target alignment.
- **Alignments**: Text file containing all alignments with mismatches, useful for further review or filtering.
- **Multi-Hits**: Text file listing all targets that have multiple hits from the same query sequence.

## How It Works
1. **BLAST Database Creation**: The tool generates a BLAST database from the user-provided genome files.
2. **Forward and Reverse Complement BLAST**: Runs two separate BLAST searches: one for the forward strand and one for the reverse complement of the query sequences.
3. **Mismatch Filtering and Analysis**: Alignments are filtered based on user-defined mismatch thresholds. Results include mismatch counts, binding positions, and melting temperatures.
4. **Result Generation**: Outputs the results in various formats including XML (for detailed BLAST results), TSV (for easy parsing and analysis), and text (for alignments and multi-hits).

## Contributing
Feel free to contribute to this project by submitting pull requests, reporting issues, or suggesting improvements.

## License
This project is licensed under the MIT License. See the `LICENSE` file for more details.

## Contact
For any questions, issues, or suggestions, please reach out via [GitHub Issues](https://github.com/mcollatz/AssayBLAST/issues).



















