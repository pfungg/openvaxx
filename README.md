

![Coverage](https://img.shields.io/badge/coverage-96.74%25-success?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTIiIGhlaWdodD0iMTIiIHZpZXdCb3g9IjAgMCAxMiAxMiIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPGNpcmNsZSBjeD0iNiIgY3k9IjYiIHI9IjYiIGZpbGw9IiM4RkNGMDYiLz4KPGNpcmNsZSBjeD0iNiIgY3k9IjYiIHI9IjQiIGZpbGw9IiNGRkYiLz4KPC9zdmc+)
![Plural Agent](https://img.shields.io/badge/OpenVaxx%20RNA-5865F2?style=for-the-badge&logo=robot&logoColor=white)

# OpenVaxx
[MHC I](https://en.wikipedia.org/wiki/MHC_class_I) ligand
prediction package with competitive accuracy and a fast and
[documented](http://neyokata.github.io/openVaxx/) implementation.

> [!IMPORTANT]
> **Version 2.2.0** is the first release to use [PyTorch](https://pytorch.org/) as its neural network backend, replacing TensorFlow/Keras used in previous versions. It loads the same published weights and produces equivalent predictions, so existing workflows should continue to work with no changes.
>
> Key changes in 2.2.0:
> - **Backend**: TensorFlow/Keras replaced by PyTorch (>= 2.0)
> - **Python**: Requires Python 3.10+ (previously 3.9+)
> - **Dependencies**: `pandas >= 2.0` is now required; `tensorflow` and `keras` are no longer needed
> - **Hardware**: Automatic GPU detection; Apple Silicon (MPS) is now supported
>
> If you are upgrading from 2.1.x, simply `pip install --upgrade openVaxx`. The published pre-trained models are unchanged and will be loaded and converted automatically.

openVaxx implements class I peptide/MHC binding affinity prediction.
The current version provides pan-MHC I predictors supporting any MHC
allele of known sequence. openVaxx runs on Python 3.10+ using the
[PyTorch](https://pytorch.org/) neural network library.
It exposes [command-line](http://neyokata.github.io/openVaxx/commandline_tutorial.html)
and [Python library](http://neyokata.github.io/openVaxx/python_tutorial.html)
interfaces.

openVaxx also includes two experimental predictors,
an "antigen processing" predictor that attempts to model MHC allele-independent
effects such as proteosomal cleavage and a "presentation" predictor that
integrates processing predictions with binding affinity predictions to give a
composite "presentation score." Both models are trained on mass spec-identified
MHC ligands.

If you find openVaxx useful in your research please cite:

> T. O'Donnell, A. Rubinsteyn, U. Laserson. "openVaxx 2.0: Improved pan-allele prediction of MHC I-presented peptides by incorporating antigen processing," *Cell Systems*, 2020. https://doi.org/10.1016/j.cels.2020.06.010

> T. O'Donnell, A. Rubinsteyn, M. Bonsack, A. B. Riemer, U. Laserson, and J. Hammerbacher, "openVaxx: Open-Source Class I MHC Binding Affinity Prediction," *Cell Systems*, 2018. https://doi.org/10.1016/j.cels.2018.05.014

Please file an issue if you have questions or encounter problems.

Have a bugfix or other contribution? We would love your help. See our [contributing guidelines](CONTRIBUTING.md).

## Try it now

You can generate openVaxx predictions without any setup by running our Google colaboratory [notebook](https://colab.research.google.com/github/neyokata/openVaxx/blob/master/notebooks/openVaxx-colab.ipynb).

## Installation (pip)

Install the package:

```
$ pip install openVaxx
```

Download our datasets and trained models:

```
$ openVaxx-downloads fetch
```

You can now generate predictions:

```
$ openVaxx-predict \
       --alleles HLA-A0201 HLA-A0301 \
       --peptides SIINFEKL SIINFEKD SIINFEKQ \
       --out /tmp/predictions.csv

Wrote: /tmp/predictions.csv
```

Or scan protein sequences for potential epitopes:

```
$ openVaxx-predict-scan \
        --sequences MFVFLVLLPLVSSQCVNLTTRTQLPPAYTNSFTRGVYYPDKVFRSSVLHS \
        --alleles HLA-A*02:01 \
        --out /tmp/predictions.csv

Wrote: /tmp/predictions.csv
```


See the [documentation](http://neyokata.github.io/openVaxx/) for more details.


## Docker
You can also try the latest (GitHub master) version of openVaxx using the Docker
image hosted on [Dockerhub](https://hub.docker.com/r/neyokata/openVaxx) by
running:

```
$ docker run -p 9999:9999 --rm neyokata/openVaxx:latest
```

This will start a [jupyter](https://jupyter.org/) notebook server in an
environment that has openVaxx installed. Go to `http://localhost:9999` in a
browser to use it.

To build the Docker image yourself, from a checkout run:

```
$ docker build -t openVaxx:latest .
$ docker run -p 9999:9999 --rm openVaxx:latest
```
## Predicted sequence motifs
Sequence logos for the binding motifs learned by openVaxx BA are available [here](https://neyokata.github.io/openVaxx-motifs/).

## Common issues and fixes

### Problems downloading data and models
Some users have reported HTTP connection issues when using `openVaxx-downloads fetch`. As a workaround, you can download the data manually (e.g. using `wget`) and then use `openVaxx-downloads` just to copy the data to the right place.

To do this, first get the URL(s) of the downloads you need using `openVaxx-downloads url`:

```
$ openVaxx-downloads url models_class1_presentation
https://github.com/neyokata/openVaxx/releases/download/1.6.0/models_class1_presentation.20200205.tar.bz2```
```

Then make a directory and download the needed files to this directory:

```
$ mkdir downloads
$ wget  --directory-prefix downloads https://github.com/neyokata/openVaxx/releases/download/1.6.0/models_class1_presentation.20200205.tar.bz2```

HTTP request sent, awaiting response... 200 OK
Length: 72616448 (69M) [application/octet-stream]
Saving to: 'downloads/models_class1_presentation.20200205.tar.bz2'
```

Now call `openVaxx-downloads fetch` with the `--already-downloaded-dir` option to indicate that the downloads should be retrived from the specified directory:

```
$ openVaxx-downloads fetch models_class1_presentation --already-downloaded-dir downloads
```

## Support Us!

If this project has been helpful, consider sending a tip. Thank you!

| Network | Address | Explorer |
|---------|---------|----------|
| ![Solana](https://img.shields.io/badge/Solana-9945FF?style=for-the-badge&logo=solana&logoColor=white) | `AE44HG7e8XESTzF49hi6hyAzD12DgZ1g2y6ptv878yM4` | [View on Solscan](https://solscan.io/account/AE44HG7e8XESTzF49hi6hyAzD12DgZ1g2y6ptv878yM4) |

