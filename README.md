# Anonymous supplemental materials

Trained models and datasets can be downloaded from GitHub releases.

## Requirements

- Python 3.7
- CUDA GPU (for Transformers)

## Installation

Create a new virtual environment for Python 3.7 with Conda:

```bash
conda create -n specialized-document-embeddings python=3.7
conda activate specialized-document-embeddings
```

Clone repository and install dependencies:

```bash
git clone https://... repo
cd repo
pip install -r requirements.txt
```

## Datasets

The datasets are compatible with [Huggingface datasets](https://github.com/huggingface/datasets) and are downloaded automatically.
To create the datasets directly from the [Papers With Code data](https://github.com/paperswithcode/paperswithcode-data), run the following commands:

```bash
# Download PWC files (for the paper with downloaded the files 2020-10-27)
wget https://paperswithcode.com/media/about/papers-with-abstracts.json.gz
wget https://paperswithcode.com/media/about/evaluation-tables.json.gz
wget https://paperswithcode.com/media/about/methods.json.gz

# Build dataset
python -m paperswithcode.dataset save_dataset <input_dir> <output_dir> 
```


## Experiments

To reproduce our experiments, follow these steps:

### Generic embeddings

Avg. FastText
```bash
# Train fastText word vectors
./data_cli.py train_fasttext paperswithcode_aspects ./output/pwc

# Build avg. fastText document vectors
./sbin/paperswithcode/avg_fasttext.sh
```
 
SciBERT
```bash
./sbin/paperswithcode/scibert_mean.sh
```

SPECTER
```bash
./sbin/paperswithcode/specter.sh
```

### Retrofitted embeddings

For retrofitting we utilize [Explicit Retroffing](https://github.com/codogogo/explirefit). 
Please follow their instruction to install it and update the `EXPLIREFIT_DIR` in the shell scripts accordingly.
Then, you can run these scripts:

```bash
# Create constraints from dataset 
./sbin/paperswithcode/explirefit_prepare.sh

# Train retrofitting models
./sbin/paperswithcode/explirefit_avg_fasttext.sh
./sbin/paperswithcode/explirefit_specter.sh
./sbin/paperswithcode/explirefit_scibert_mean.sh

# Generate and evaluate retrofitted embeddings 
./sbin/paperswithcode/explirefit_convert_and_evaluate.sh
```


### Transformers

```bash
# SciBERT
./sbin/paperswithcode/pairwise/scibert.sh

# SPECTER
./sbin/paperswithcode/specter_fine_tuned.sh

# Sentence-SciBERT
./sbin/paperswithcode/sentence_transformer_scibert.sh
```



## Evaluation

After generating the document representations for all aspects and systems, the results can be computed and viewed with a Jupyter notebook. 
Figures and tables from the paper are part of the notebook.

```bash
# Run evaluations for all systems
./eval_cli.py reevaluate

# Open notebook for Tables and Figures
jupyter notebook evaluation.ipynb

# Open notebook for sample recommendations
jupyter notebook samples.ipynb
```

## License

This repository is only for peer-review.