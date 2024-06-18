# <img src="figures/dyna_logo.png" alt="DYNA Logo" width="50"/> DYNA: Disease-Specific Language Model for Variant Pathogenicity


Clinical variant classification of pathogenic versus benign genetic variants remains a challenge in clinical genetics. Recently, the proposition of genomic foundation models has improved the generic variant effect prediction (VEP) accuracy via weakly-supervised or unsupervised training. However, these VEPs are not disease-specific, limiting their adaptation at the point of care. To address this problem, we propose DYNA: Disease-specificity fine-tuning via a Siamese neural network broadly applicable to all genomic foundation models for more effective variant effect predictions in disease-specific contexts.

We evaluate DYNA in two distinct disease-relevant tasks. For coding VEPs, we focus on various cardiovascular diseases, where gene-disease relationships of loss-of-function vs. gain-of-function dictate disease-specific VEP. For non-coding VEPs, we apply DYNA to an essential post-transcriptional regulatory axis of RNA splicing, the most common non-coding pathogenic mechanism in established clinical VEP guidelines. In both cases, DYNA fine-tunes various pre-trained genomic foundation models on small, rare variant sets. The DYNA fine-tuned models show superior performance in the held-out rare variant testing set and are further replicated in large, clinically-relevant variant annotations in ClinVAR. Thus, DYNA offers a potent disease-specific variant effect prediction method, excelling in intra-gene generalization and generalization to unseen genetic variants, making it particularly valuable for disease associations and clinical applicability.

## Framework
<p align="center">
<img src="/figures/dyna_framework_v3.png" alt="The framework" style="width:20cm; height:auto;"/>
</p>

## Repository Organization

1. **dyna_data/**:
    - For coding variant effect predictions (VEPs), our approach centers on clinical variant sets specifically related to inherited cardiomyopathies (CM) and arrhythmias (ARM). We utilize a pre-compiled dataset comprised of rare missense pathogenic and benign variants, categorized using a cohort-based approach for diseases such as cardiomyopathy and arrhythmias, as detailed in the previous report by Zhang et al. ClinVar CM and ARM datasets include all missense variants in CM and ARM, respectively, are extracted from ClinVar (Landrum et al.). 
    - In the realm of non-coding VEPs, our focus shifts to splicing-related variants, utilizing a dataset from the multiplexed assay for exon recognition by Chong et al., which highlights the significant impact of rare genetic variants on splicing disruptions. Similarly, the ClinVar Splicing dataset, compiled from ClinVar, encompasses all benign sequences and pathogenic variants pertinent to splicing. 
    - For the ClinVar CM and ARM datasets, we translate the DNA sequences into protein sequences using the human genome assembly hg38 from [NCBI](https://www.ncbi.nlm.nih.gov/grc/human). We employed the GFF file, MANE.GRCh38.v1.1.ensembl_genomic.gff.gz from [NCBI](https://www.ncbi.nlm.nih.gov/refseq/MANE), to annotate coding versus non-coding regions for each gene, as only coding DNA sequences are translated into proteins. Additionally, protein domains, cataloged in the Pfam database (Finn et al.), are essential for the functional characterization of proteins. These domains are identified by aligning the translated sequences to known domain structures, thereby facilitating deeper insights into protein function. All the data is open source at [Zenodo](https://zenodo.org/records/12116074).

2. **dyna/**:
    - DYNA employs a Siamese neural network on embeddings generated by two language model branches that share weights. In the context of genomic foundation models, we analyze two distinct types of biological inputs: protein sequences, which represent the coding regions of the genome, and DNA sequences, corresponding to both coding and non-coding regions. This dual approach allows us to comprehensively assess the functional impacts of genetic variations on both the protein products and the regulatory elements.

3. **scripts/**:
    - SLURM batch script to run the `.py` files.

4. **dyna_model/**:
    - This link contains 2 DYNA fine-tuned checkpoints. See [Google Drive](https://drive.google.com/drive/folders/16N7WpiiSmP1TkGfaIC64vOvPQMv2siYy?usp=sharing). Replace `/path/to/your/local/model` with the actual file path to your saved model on your local system. 


## Setting up environment 
<pre>
conda env create -f dna_llm.yml
</pre>

## Fine-tuning the DYNA model
<pre>
sbatch scripts/test_gpu.sh
</pre>
