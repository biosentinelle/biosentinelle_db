# biosentinelle_db

Strain identity database of [biosentinelle](https://github.com/biosentinelle/biosentinelle), and the workflow that builds it.

biosentinelle (*main.nf*) only reads this database: nothing has to be built to analyze a sample. Clone this repository next to biosentinelle, so that its default `db_path` (`$projectDir/../biosentinelle_db/db`) finds it:

<pre>
git clone https://github.com/biosentinelle/biosentinelle_db.git
</pre>

<br><br>
## CONTENT

| Files and folder | Description |
| :--- | :--- |
| **db folder** | The database, one sub folder per organism. Currently *db/yeast* only (*Saccharomyces cerevisiae*). |
| **build_db.nf** | Builds the strain panel (*db/\<organism\>/panels*) and the consensus of the read strains (*db/\<organism\>/strains/reads*). |
| **nextflow.config** | Parameter settings of *build_db.nf*. |
| **bin folder** | Python scripts that build each part of the database. |
| **modules folder** | Nextflow processes of *build_db.nf*. |
| **Licence.txt** | Licence of the release. |

<br><br>
## THE YEAST DATABASE

| Folder | Content |
| :--- | :--- |
| *organism.tsv* | Organism, reference genome and summary of the database. |
| *panels* | Strain SNP and indel panel: *panel_sites.tsv.gz* (sites and expected allele of each strain), *panel_kmers.tsv.gz* (k-mers of each allele), *panel_strains.tsv* (strains of the panel), *panel_report.txt* (build log). |
| *strains* | Strain catalogue (*strains.tsv*) and strain name aliases (*aliases.tsv*). |
| *mating* | MATa and MATalpha k-mers (*mating_kmers.tsv*). |
| *markers* | Native marker gene loci (*marker_loci.tsv*, *.bed*, *.fasta*), allele catalogue (*marker_alleles.tsv*), documented genotypes of laboratory strains (*documented_genotypes.tsv*) and repeats masked in the flanks (*marker_repeats.tsv*). |
| *tools* | Laboratory tool element sequences (*tools.fasta*) and their description (*tools.tsv*). |

The way each part was made, the sources, the choice of k and the validation of the yeast panel are described in [db/README.md](db/README.md), together with the commands to rebuild them.

<br><br>
## HOW TO BUILD

The strain panel is rebuilt with:

<pre>
nextflow run build_db.nf -c nextflow.config
</pre>

using the "Database construction" parameters of the *nextflow.config* file (`strain_table`, `population_matrix`, `reads_dir`, `reads_min_depth`, `panel_reference`, `panel_kmer_size`, `panel_options`). The results are written in `db_path`/`organism_dir`.

Warning: changing `panel_reference` or `panel_kmer_size` invalidates the whole database. `panel_reference` must also be the genome set in the *nextflow.config* file of biosentinelle, which genotypes the native marker genes in its coordinates.

The validation tests of the database (*test/validate_panel.py*, *test/validate_markers.py*) run the analysis scripts, so they stay in biosentinelle.

<br><br>
## LICENCE

See [Licence.txt](Licence.txt).
