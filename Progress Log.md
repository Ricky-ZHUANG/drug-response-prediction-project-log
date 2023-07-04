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

- we may have to parse the 52GB big file so that every rid/cid are in the returned list. Or we have to obtain the rid/cid and take the interception of them.

# Summary table of the data we may use for perturbation profile generation task.

| **Drug** | **Perturbation** | **Signature** | **Cell line** | **Gene** |
|----------|------------------|---------------|---------------|----------|
|   39319  |       83588      |    1201944    |      248      |    978   |
***
