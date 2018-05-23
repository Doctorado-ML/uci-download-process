# UCI Machine Learning Repository downloading and conversion scripts

This is a script for converting [UCI Machine Learning Repository](http://archive.ics.uci.edu/ml/datasets.html) datasets (and some from other sources) into a common format. This Python repository is a kind of fork of Julia repository [JackDunnNZ/uci-data](https://github.com/JackDunnNZ/uci-data), from which configuration files are extracted. The UCI ML repository is a useful source for machine learning datasets for testing and benchmarking, but the format of datasets is not consistent. This means effort is required in order to make use of new datasets since they need to be read differently.

Instead, the aim is to convert the datasets into a format to be read
from [PyRidge](https://github.com/cperales/PyRidge), where each line is as follows:

    attribute_1 attribute_2 ... attribute_n class

This makes it easy to switch out datasets in ML problems, which is great when automating things!

## Converting to common format

The datasets are not checked in to git in order to minimise the size of the repository and to avoid rehosting the data. As such, the script downloads any missing datasets directly from UCI as it runs.

### Running the script

#### Command line

For downloading, use the following command from the root folder:

```bash
python download_data.py
```

And this sent the urls from data to `classification_db.txt`. If data
was downloaded before, it won't be added again.

For converting the downloaded files into the format described in
the previous section (attributes separated by spaces, integer target
at last position), run:

```bash
python convert_data.py
```

Eventually, processing data is finished with k folding, with `k=10`.

```bash
python k_folding.py
```


## Guide to config files

Due to the varying nature of the datasets in the repository, the script needs to behave differently for different datasets. This is achieved using the `config.ini` files present in each dataset folder. An example of this file is:

    [info]
    name = mammographic-mass.data
    info_url = https://archive.ics.uci.edu/ml/datasets/Mammographic+Mass
    data_url = https://archive.ics.uci.edu/ml/machine-learning-databases/mammographic-masses/mammographic_masses.data
    target_index = 6
    id_indices =
    value_indices = 1,2,3,4,5
    categoric_indices = 3,4
    separator = comma
    header_lines = 0

A guide to each of the attributes follows.

##### [info]

This is an arbitrary choice for describing the data in the config file. It must be included at the start of each file but otherwise doesn't matter.

##### name

The name of the dataset that will be produced by the script. The convention used is the name of the dataset on the UCI info page, converted to lower case with spaces replaced with hyphens. The suffix `.data` is then added. This name (before adding `.data`) is also used for the name of the containing folder.

##### info_url

Contains the link to the UCI information page for the dataset, allowing the dataset to be traced back to its source.

##### data_url

Contains the link to the dataset itself on UCI. To avoid checking in the datasets to Github, the script instead downloads any missing datafiles using these links when it runs.

##### target_index

A single integer indicating the index (1-based) of the variable in the dataset we want to predict.

##### id_indices

Any number of integers (separated by commas and no spaces i.e. 1,2,3) that indicate the indices (1-based) of any id values present in the dataset. These will be combined to form the final id value used in the output. If no id information is present in the dataset, leave this blank and the id value will be generated automatically.

##### value_indices

One or more integers (separated by commas and no spaces i.e. 1,2,3) that indicate the indices (1-based) of the data values in the dataset.

##### categoric_indices

A subset of the integers specified in `value_indices` that indicate those data values that are categorical/numeric in nature.

##### separator

The separator between values in the dataset.

##### header_lines

An integer number of header lines in the dataset before the values are reached.

## Acknowledgement

Config files comes from [JackDunnNZ/uci-data](https://github.com/JackDunnNZ/uci-data)
repository. Contributing by adding new datasets to original repository is recommended.
