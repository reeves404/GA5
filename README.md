<!--
GA5 README template.
- Add your answers to each question's section below.
- For each question where we want you to add your commands,
  put the command(s) you ran in the terminal in the corresponding ```bash code block.
- Where we want to see a command's output, there is a plain ``` code block
  right below the command block: paste the output there.
- The code to run each step of the workflow goes in `protocol.md`, not here:
  you don't need to repeat it in this file.
- You don't need to paste the contents of your scripts into this file:
  we will look at the scripts in your `scripts` dir directly
  (so make sure you commit them!).
- Delete instructional comments like this one as you go.
-->

# GA5 answers - <reeves404>

------------------------------------------------------------------------

## Part A: Your project dir, Git repo, and protocol

### 1: Fill in the top of the protocol

<!-- No answer needed here -- we check `protocol.md` directly. -->

### 2: Markdown formatting

<!-- No answer needed here -- we check the formatting of both Markdown files. -->

### 3: Create subdirs, and copy the scripts

```bash
# Your mkdir command: mkdir -p scripts data/ref results
```

### 4: Initialize a Git repository, and commit your first files

<!-- No answer needed here -- we check the repo directly. -->

### 5: Download the reference transcriptome

<!-- Put the download command in `protocol.md`, not here. -->

Code to count the number of transcripts:

```bash
# Your command: gzip -cd data/ref/transcripts_rna.fna.gz | grep -c '^>'
```

The output of the command was:

```
```

*Answer*: The number of transcripts in the file: 28624

------------------------------------------------------------------------

## Part B: Read trimming

### 6: Run TrimGalore

<!-- Put the code to run TrimGalore in `protocol.md`, not here. -->

*Answer*: Whether all batch jobs succeeded, and how you checked that
(and any problems you encountered, and how you solved them): 4 of them initially failed. i looked at logs and resubmitted the ones that failed. After resubmission i rechecked that they worked this time

------------------------------------------------------------------------

## Part C: Transcript quantification with Salmon

### 7: Create the Salmon index

Code to check the size of the index dir:

```bash
# Your command: du -sh results/salmon_index
```

The output of the command was:

```
```

*Answer*: How long it took to create the index, and how you found out: 24 seconds

*Answer*: The size of the index dir: 1.1 GB

### 8: Run Salmon for all samples

<!-- No answer needed here -- we check `protocol.md` directly. -->

### 9: The transcripts with the most reads

*Answer*: What the `NumReads` column contains: salmons estimated number of reads for each transcript 

```bash
# Your command: tail -n +2 results/salmon_quant/S63/quant.sf | sort -k5,5nr | head -n 3 
```

The output of the command was:

```
XM_039591534.2  6566    6315.086        14518.656821    24004.889
XM_039573735.1  6445    6194.086        5342.073324     8663.254
XM_039573733.1  6591    6340.086        2876.480244     4774.749
```

### 10: The gene of the top transcript

```bash
# Your command: gzip -cd data/ref/transcripts_rna.fna.gz | grep '^>XM_039591534.2 '
```

The output of the command was:

```
>XM_039591534.2 PREDICTED: Culex pipiens pallens vitellogenin-A1-like (LOC120426760), mRNA
```

*Answer*: The gene that the top transcript belongs to: vitellogenin-A1-like (LOC120426760)

### 11: Mapping rates

```bash
# Your command: grep -i "mapping rate" results/salmon_quant/*/logs/salmon_quant.log
```

The output of the command was:

```
results/salmon_quant/S63/logs/salmon_quant.log:mapping rate: 82.4560%
results/salmon_quant/S64/logs/salmon_quant.log:mapping rate: 81.9152%
results/salmon_quant/S65/logs/salmon_quant.log:mapping rate: 79.1055%
results/salmon_quant/S66/logs/salmon_quant.log:mapping rate: 81.9350%
results/salmon_quant/S67/logs/salmon_quant.log:mapping rate: 83.0459%
results/salmon_quant/S68/logs/salmon_quant.log:mapping rate: 78.8070%
results/salmon_quant/S69/logs/salmon_quant.log:mapping rate: 82.1824%
results/salmon_quant/S70/logs/salmon_quant.log:mapping rate: 82.3060%
results/salmon_quant/S71/logs/salmon_quant.log:mapping rate: 79.9047%
results/salmon_quant/S74/logs/salmon_quant.log:mapping rate: 78.2753%
results/salmon_quant/S75/logs/salmon_quant.log:mapping rate: 80.3915%
results/salmon_quant/S76/logs/salmon_quant.log:mapping rate: 82.3396%
results/salmon_quant/S77/logs/salmon_quant.log:mapping rate: 82.3281%
results/salmon_quant/S78/logs/salmon_quant.log:mapping rate: 84.2490%
results/salmon_quant/S79/logs/salmon_quant.log:mapping rate: 80.4061%
results/salmon_quant/S80/logs/salmon_quant.log:mapping rate: 82.1721%
results/salmon_quant/S81/logs/salmon_quant.log:mapping rate: 83.6795%
results/salmon_quant/S82/logs/salmon_quant.log:mapping rate: 78.7291%
results/salmon_quant/S83/logs/salmon_quant.log:mapping rate: 76.1504%
results/salmon_quant/S84/logs/salmon_quant.log:mapping rate: 83.2207%
results/salmon_quant/S85/logs/salmon_quant.log:mapping rate: 76.9540%
results/salmon_quant/S86/logs/salmon_quant.log:mapping rate: 83.0532%
```

*Answer*: The sample with the lowest mapping rate: S83 with 76%

------------------------------------------------------------------------

## Part D: Summarizing all steps with MultiQC

### 12: Update your MultiQC script

<!-- No need to paste the script: we'll look at scripts/multiqc.sh and its Git history directly. -->

### 13: Run MultiQC

*Answer*: For each program, the number of reports that MultiQC found,
and why it found this number: salmon 22 meta, fragment length and library format reports, one for each type of the 22 samples. Trim galore had 44 because each of the 22 had two read files and fastqc had 44 because it analyzed the trimmed r1&2 seperately for the 22

### 14: The General Statistics table

*Answer*: The sample with the lowest Salmon mapping rate,
and whether this matches your answer to question 11: S83 which does match my answer

### 15: Adapter lengths

*Answer*: The most common adapter lengths, whether this surprised you,
and why you think these lengths are the most common: 1-3 mainly 1. Sort of surprising as the full length is longer. Probably trims bases that resemnle the beginning of an adapter which makes sense for most common lengths

### 16: Ask Claude about the MultiQC report

*Answer*: What you asked, and what you learned from Claude's answer (in your own words): i asked why 1-3 bases is the most common adapter length trimmed. It basically said theyre almost all chance matches and not real adapter contamination. Also that it doest actually matter

------------------------------------------------------------------------

## Part E: Improving the workflow with genAI

### 17: Claude's review of your protocol and scripts

*Answer*: At least three of Claude's suggestions,
and whether you agree or disagree with each one, and why: 1. correct the setup code which i agree with for replicability. 2. describe the placeholders which i also agree with. so having someone know where everything came from and having an abosolute path. 3 document the job orders , so talk about the initial failed and corrections which i probably agree with. 

### 18: Address a suggestion

*Answer*: Which suggestion(s) you addressed, and what you changed: i addressed the first suggestion so i added into the protocol md and added mkdir -p data/ref so that creates a folder before wget tried to save the reference file there 

------------------------------------------------------------------------

## Part F: Publish your repo on GitHub

### 19: Create a GitHub repository and push

<!-- No answer needed here -- we check your GitHub repo directly. -->

### 20: Get a (test) DOI with Zenodo

*Answer*: Your Zenodo Sandbox DOI:
