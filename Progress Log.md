***
- The relation of the 3 files
- One sig_id correspond one perturbation experiment, one pert_id many perturation experiments (The same dose different perturbation time/duration)

- original matrix (sig_id * gene_id)
- use the idendity in signature matrix to obtain a list of sig_id:

e.g. this returns a list of sig_id whose perurbation type is chemical compound
```python
sig_info = pd.read_csv("siginfo_beta.txt",sep='\t')
compound_ids = sig_info["sig_id"][sig_info["pert_type"] == "trt_cp"] 
``` 
- use idendity of genes to obtain a list of gene_id:

e.g. this returns a list of gene_id whose feature space is landmark
```python
gene_info = pd.read_csv("geneinfo_beta.txt",sep='\t')
landmark_gene_ids = gene_info["gene_id"][gene_info["feature_space"] == "landmark"]
```

- we may have to parse the 52GB big file so hat every rid/cid are in the returned list. Or we have to obtain the rid/cid and take the interception of them.

- This is the summary table of the data we may use for perturbation profile generaton task.

| **Drug** | **Perturbation** | **Signature** | **Cell line** | **Gene** |
|----------|------------------|---------------|---------------|----------|
|   39319  |       83588      |    1201944    |      248      |    978   |
***
