***
## The relation of the 3 files
- One sig_id correspond one perturbation experiment, one pert_id many perturation experiments (The same dose different perturbation time/duration)

- original matrix (sig_id * gene_id)


## Methods of obtaining wanted data from beta data
Obtain drug information
1. Use the identity in signature matrix to obtain a list of sig_id:

e.g. this returns a list of sig_id whose perturbation type is `chemical compound`
```python
sig_info = pd.read_csv("siginfo_beta.txt",sep='\t')
drug_sig_ids = sig_info["sig_id"][sig_info["pert_type"] == "trt_cp"] 
```
2. Use `drug_sig_ids` to obtain pert_id so that we can locate the drug data
```python
drug_pert_id = sig_info.loc[sig_info["sig_id"].isin(drug_sig_ids),'pert_id']
```
3. Use `drug_pert_id` to obtain the SMILES data of drug in interest
```python
drug.loc[drug['pert_id'].isin(drug_pert_id),'canonical_smiles']
```

Use identity of genes to obtain a list of gene_id:

e.g. this returns a list of gene_id whose feature space is `landmark`
```python
gene_info = pd.read_csv("geneinfo_beta.txt",sep='\t')
landmark_gene_ids = gene_info["gene_id"][gene_info["feature_space"] == "landmark"]
```

## Extracting gene perturbation profile
take other treatment file as example
```python
other_treatment_row_metadata = parse('level5_beta_other_treatment.gctx',cid=landmark_gene_ids)
```
`parse_gctx check_id_validity some of the ids being used to subset the data are not present in the metadata for the file being parsed`

we may have to parse the 52GB big file so that every rid/cid are in the returned list. Or we have to obtain the rid/cid and take the interception of them.

(這個還要改，drug那裏它1個藥有2個target是儅2行來寫的)
# Summary table of the data we may use for perturbation profile generation task.

| **Drug** | **Perturbation** | **Signature** | **Cell line** | **Gene** |
|----------|------------------|---------------|---------------|----------|
|   28576(check in the big matrix)  |       34418      |    720216     |      248      |    978   |
***

About dose
  ```
  Concentration and duration of treatment. High-throughput, cell-based small-molecule screens are often performed at a single, relatively high concentration of 10 mM.
  For some compounds, we used concentrations reported to be effective in cell culture or to approximate the maximum attainable plasma concentrations after therapeutic dosing
  ```

About perturbation duration
```
  The duration of perturbation is also important because it takes time for perturbagens to change the DNA expression, like go through liver and spread the the whole body)
```

About signature
```
  signature is used to measure the difference between 2 perturbagens. (according 1st version)
  Cmap is a tool for mining small molecules that share similar gene expression
```

基因表達譜的意義：可能是說，這個藥物對於這個基因的影響能力從-1去到1
`After the query signature (mRNA expression level was significantly changed) was found from the drug or disease, the connectivity scores of each drug molecule in the database ranging from −1 to 1 (from completely negative correlation to completely positive correlation) were available by querying the Connectivity Map.`
***
