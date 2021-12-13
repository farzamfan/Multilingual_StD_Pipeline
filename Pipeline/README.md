## Dependencies

| Dependency                  | Version | Installation Command                                                |
| ----------                  | ------- | ------------------------------------------------------------------- |
| Python                      | 3.8.5   | `conda create --name stance python=3.8` and `conda activate stance` |
| PyTorch, cudatoolkit        | 1.5.0   | `conda install pytorch==1.5.0 cudatoolkit=10.1 -c pytorch`          |
| Transformers  (HuggingFace) | 3.5.0   | `pip install transformers==3.5.0`     |
| Scikit-learn                | 0.23.1  | `pip install scikit-learn==0.23.1`    |
| scipy                       | 1.5.0   | `pip install scipy==1.5.0`            |
| Ekphrasis                   | 0.5.1   | `pip install ekphrasis==0.5.1`        |
| emoji                       | 0.6.0   | `pip install emoji`                   |
| wandb                       | 0.9.4   | `pip install wandb==0.9.4`            |


## Instructions


### Directory Structure

Following is the structure of the codebase, in case you wish to play around with it.

- `train.py`: Model and training loop.
- `bertloader.py`: Common Dataloader for the 6 datasets.
- `params.py`: Argparsing to enable easy experiments.
- `README.md`: This file :slightly_smiling_face:
- `.gitignore`: N/A
- `data`: Directory to store all datasets
  - `data/16semeval`: Folder for the Semeval 2016 dataset
    - `data/16semeval/README.md`: README for setting up Semeval 2016 dataset
    - `data/16semeval/process.py`: Script to set up the Semeval 2016 dataset
    - `data/16semeval/scrapped_full`: train and test set
    - `data/16semeval/data.json`: json file for the pipeline (generated by `data/16semeval/process.py`)
  - `data/PImPo`: Folder for the Semeval 2016 dataset
    - `data/PImPo/create_PlmPo_with_Verbatim.r`: R file for reading the dataset from Project Manifesto API
    - `data/PImPo/process.py`: Script to set up the PImPo dataset (generating data.json)
    - `data/PImPo/[all OR countryName]`: train and test set for specific country
    - `data/PImPo/[all OR countryName]/data.json`: json file for the pipeline (generated by `data/16semeval/process.py`)  


### 1. Setting up the codebase and the dependencies.

- Clone this repository - `git clone https://github.com/farzamfan/Multilingual_StD_Pipeline`
- Follow the instructions from the [`Dependencies`](#dependencies) Section above to install the dependencies.
- If you are interested in logging your runs, Set up your wandb - `wandb login`.

### 2. Setting up the datasets.

This codebase supports the 2 datasets 16semeval and PImPo.
- SemEval 2016 Task 6 Dataset ([Mohammed et al., 2016](https://doi.org/10.18653/v1/S16-1003)) - `16semeval`

- The PImPo dataset can be generated by using the Project Manifesto API. (https://manifesto-project.wzb.eu/information/documents/pimpo)

For each `<dataset-name>` set up up inside its respective folder `data/<dataset-name>`. The instruction to set up each `<dataset-name>` can be found inside `data/<dataset-name>/README.md`. After following those steps, the final processed data will stored in a json format `data/<dataset-name>/data.json`, which will be input to our model.

### 3. Training the models.

We experimented with two models

- Target Oblivious Bert

<img src="https://github.com/Ayushk4/bias-stance/blob/master/images/target-oblivious-bert.png" alt="target-oblivious-bert" width="200"/>

- Target Aware Bert

<img src="https://github.com/Ayushk4/bias-stance/blob/master/images/target-aware-bert.png" alt="target-aware-bert" width="200"/>



After following the above steps, move to the basepath for this repository - `cd bias-stance` and recreate the experiments by executing `python3 train.py [ARGS]` where `[ARGS]` are the following:

Required Args:
- dataset_name: The name of dataset to run the experiment on. Possible values are ["16se", "PImPo"]; Example Usage: `--dataset_name=PImPo`; Type: `str`; This is a required argument.
- test_mode: Indicates whether to evaluate on the test in the run; Example Usage: `--test_mode=False`; Type: `str`
- bert_type: A required argument to specify the bert weights to be loaded. Refer [HuggingFace](https://huggingface.co/models). Example Usage: `--bert_type=bert-base-cased`; Type: `str`

Optional Args:
- seed: The seed for the current run. Example Usage: `--seed=1`; Type: `int`
- batch_size: The batch size. Example Usage: `--batch_size=16`; Type: `int`
- lr: Learning Rate. Example Usage: `--lr=1e-5`; Type: `float`
- n_epochs: Number of epochs. Example Usage: `--n_epochs=5`; Type: `int`
- dummy_run: Include `--dummy_run` flag to perform a dummy run with a single trainign and validation batch.
- device: CPU or CUDA. Example Usage: `--device=cpu`; Type: `str`
- wandb: Include `--wandb` flag if you want your runs to be logged to wandb.
- notarget: Include `--notarget` flag if you want the model to be target oblivious.


## Miscelaneous  

- The codebase has been written from scratch, but was inspired from many others, manily:

- Authors: Ayush Kaushal, Avirup Saha and Niloy Ganguly
- Code base written by Ayush Kaushal
- NAACL 2021 Proceedings

```
@inproceedings{kaushal2020stance,
          title={tWT–WT: A Dataset to Assert the Role of Target Entities for Detecting Stance},
          author={Kaushal, Ayush and Saha, Avirup and Ganguly, Niloy} 
          booktitle={Proceedings of the 2021 Conference of the North {A}merican Chapter of the Association for Computational Linguistics: Human Language Technologies (NAACL-HLT 2021)},
          year={2021}
        }
```

- License: MIT

