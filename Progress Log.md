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
|   28576  |       34418      |    720216     |      248      |    978   |
***
720216個signature(i.e. 720216個實驗)，28576種藥物/perturbagens，978個landmark基因。（Perturbation數目比藥物多是因爲一個藥物兩個target會被記錄兩次，但是他們的pert_id是一樣的）

## About dose
  ```
  Concentration and duration of treatment. High-throughput, cell-based small-molecule screens are often performed at a single, relatively high concentration of 10 mM.
  For some compounds, we used concentrations reported to be effective in cell culture or to approximate the maximum attainable plasma concentrations after therapeutic dosing
  ```

## About perturbation duration
```
  The duration of perturbation is also important because it takes time for perturbagens to change the DNA expression, like go through liver and spread the the whole body)
```

## About signature

Cmap uses signature to encode the information of a single treatment (like drug, target, dose, treatment duration).

The whole process be like:

set1 is using drug treatment, set2 is no drug treatment. Obtain the 2 gene expression profile (i.e. one is before treatment another is post treatment) via L1000 method. The 2 gene expression profile are the signature.

People can use the signature to measure the difference/similarity between drugs.
***
## About how the gene expression profile is derived (CMap)
```
C. Validation of L1000 probes using shRNA knockdown. MCF7 and PC3 cells transduced with shRNAs targeting 955 landmark genes. Differential expression values (z-scores) were computed for each landmark and the percentile rank of expression z-scores in the experiment in which it was targeted relative to all other experiments was computed.
```
so it used `differential expression value`
```
为了测量 1,000 个标志性转录本，我们采用了一种涉及连接介导扩增 (LMA) 的方法，然后在荧光寻址微球上捕获扩增产物（Peck 等人，2006）。我们将此方法扩展到 1,000 重反应（图1A; 协议请参见clue.io/sop-L1000.pdf）。
```
這個是用來measure那987個landmark基因的方法
