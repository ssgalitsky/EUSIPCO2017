Music auto-tagging experiments for the paper entitled "Timbre Analysis of Music Audio Signals with Convolutional Neural Networks" by Jordi Pons, Olga Slizovskaia, Rong Gong, Emilia Gómez and Xavier Serra.

We provide the code for data preprocessing, training and evaluation of our approach.

## Installation
Python scripts based on Lasagne-Theano for deep learning and Librosa for feature extraction.

Requires installing Lasagne-Theano (http://lasagne.readthedocs.org/en/latest/user/installation.html) and Librosa (https://github.com/librosa/librosa). 

Lasagne is already in a /src folder, to install Theano do:
> sudo pip install --upgrade https://github.com/Theano/Theano/archive/master.zip

Dependencies: pandas, numpy and scipy.


## Steps for reproducing our results
1. Download the MagnaTagATune (MTT) dataset. See: http://mirg.city.ac.uk/codeapps/the-magnatagatune-dataset and https://github.com/keunwoochoi/magnatagatune-list 
2. Copy the MTT datset to `./data/audio/MagnaTagATune/` in its original format: 16 folders, '0' to '9' and then 'a' to 'f'. It will look like: `./data/audio/MagnaTagATune/a/`, `./data/audio/MagnaTagATune/2/`, etc. Note that you will have to create the folder: `./data/audio/MagnaTagATune/`.
3. Go to src folder: `cd EUSIPCO2017/src/`
4. Run `python spectrograms.py`. 
5. Run `python exp_setup.py`. 
6. Run `python patches.py`.
7. Configure `train.py`: 
    1. Set `config['patches']` to be the folder generated by `patches.py`, ie: `patches/patches_dieleman_setup_eusipco2017_187_logC_elementWise_memory_1489752607/`. 
    2. Choose the architecture by setting `config['type']` to: `smallSquared` or `proposed` or `proposed2`.
8. Run `python train.py`. If you want to use your GPU, you probably want to run the following: THEANO_FLAGS=mode=FAST_RUN,device=gpu,floatX=float32 python test_2.py
9. Configure `test.py`: set test_params['model_name'] to be the model created by `train.py`, ie: `dieleman_setup_eusipco2017_proposed2_v0_210270563616880246637098465086143809559` - without the extension. 
10. Run `python test.py`.

## Additional information
You might want to use this code for your own porpuses. If this is the case, the following additional information might be useful for you.

### Scipts pipeline
`spectrograms.py` > `exp_setup.py` > `patches.py` > `train.py` > `test.py`
- `spectrograms.py`: computes spectrograms.
- `exp_setup.py`: splits data in train, val, test. Requires: previous run of 'spectrograms.py'.
- `patches.py`: computes patches and normalizes the data. Requires: previous run of 'exp_setup.py'.
- `train.py`: trains a deep learning model defined in 'builid_architecture.py'. Requires: previous run of 'patches.py'.
- `test.py`: evaluates how the trained model performs. Requires: previous run of 'train.py'.

### Folders structure
Root folders:
- `./data`: with audio, groundtruth and all intermediate files (spectrograms, patches and train/test results).
- `./src`: with core and static scripts.

Default data folders:
- `./data/audio/`: with the raw audio files.
- `./data/index/`: with pre-processed *(i)* 'index_file' and *(ii)* 'gt_file'.

When running the scripts throughout the pipeline, the following folders will be created:
- `./data/spectrograms/`
- `./data/exp_setup/`
- `./data/patches/`
- `./data/train/`
- `./data/test/`
