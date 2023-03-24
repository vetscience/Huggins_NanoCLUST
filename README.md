# Huggins_NanoCLUST
NanoCLUST/NCBI databases for filarial worms and apicomplexans
Download filarial worm database from here
https://melbourne.figshare.com/ndownloader/files/39387152
Download apicomplexan database from here
https://melbourne.figshare.com/ndownloader/files/39387155

Construction of Filarial Worm and Apicomplexan Haemoparasite Databases for NanoCLUST:

Within NCBI nucleotide the filarial worm COI gene Db was constructed using the search terms:
(((((((((((cytochrome c oxidase subunit 1[Title]) OR cytochrome c oxidase subunit I) OR cytochrome oxidase subunit 1) OR cytochrome oxidase subunit I) OR COX1) OR CO1) OR COI)) AND txid6295[Organism:exp])) AND 100:100000[Sequence Length])
And the NCBI accession NR_029255.1 (Aliivibrio fischeri) required for identification of our positive control. 
Additionally a second filarial worm Db was constructed from the same sequences downloaded using the aforementioned search terms with the inclusion of the dog genome GCF_014441545.1 (Canis lupus familiaris). 
For construction of the apicomplexan 18S rRNA gene Db the search terms used were:
((((((18S ribosomal RNA[Title]) OR 18S rRNA[Title]) OR ribosomal RNA[Title]) OR SSU rRNA[Title]) OR SSU ribosomal RNA[Title]) AND txid5794[Organism]) AND 200:10000[Sequence Length] 
Plus the addition of NR_029255.1 (Aliivibrio fischeri) required for positive control identification. 
The specific fasta sequences were chosen and downloaded as a fasta file from NCBI.
Extracted accession numbers from the fasta headers and produce a single column text file.
Downloaded the large NCBI accession2taxid database - a text file:
ftp.ncbi.nlm.nih.gov/pub/taxonomy/accession2taxid/nucl_gb.accession2taxid.gz
Created a mapping table of each accession to its taxa id using nucl_gb.accession2taxid
The awk command:
awk -F"\t" 'BEGIN{while(getline<"accession_ids.txt") hash[$1]=1} {if ($2 in hash) print $2,$3}' nucl_gb.accession2taxid > [Db_name]_map.txt
This will take a list of accession numbers "accession_ids.txt" and the downloaded accession2taxid database to produce a two column mapping file called [Db_name]_map.txt
Then used the makeblastdb command, downloadable from https://blast.ncbi.nlm.nih.gov/Blast.cgi
makeblastdb -in Filaria_AllCOI_species.fasta -parse_seqids -blastdb_version 5 -taxid_map [Db_name]_map.txt -title "[Db_name] database" -out [Db_name] -dbtype nucl
This produces the blast database, consisting of 10 files required by NanoCLUST. 
