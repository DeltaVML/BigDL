# Train a two tower model using recsys data
This example demonstrates how to use BigDL Friesian to train a two tower model using [Twitter Recsys Challenge 2021 data](http://www.recsyschallenge.com/2021/).

## Prepare the environment
We recommend you to use [Anaconda](https://www.anaconda.com/distribution/#linux) to prepare the environments, especially if you want to run on a yarn cluster (yarn-client mode only).
```
conda create -n bigdl python=3.7  # "bigdl" is the conda environment name, you can use any name you like.
conda activate bigdl
pip install tensorflow==2.9.0
pip install --pre --upgrade bigdl-friesian[train]
```
## Preprocess data
You can download the full Twitter dataset from [here](http://www.recsyschallenge.com/2021/) and then follow the [WideAndDeep Preprocessing](https://github.com/intel-analytics/BigDL/tree/main/python/friesian/example/wnd/recsys2021#prepare-the-data) to preprocess the original data.

## Training 2 tower model
* Spark local, we can use some sample data to have a trial, example command:
```bash
python train_2tower.py \
    --executor_cores 8 \
    --executor_memory 50g \
    --data_dir /path/to/the/folder/of/preprocessed_data \
    --model_dir /path/to/the/folder/to/save/trained_model 
```

* Spark standalone, example command:
```bash
python train_2tower.py \
    --cluster_mode standalone \
    --master spark://master-url:port \
    --executor_cores 8 \
    --executor_memory 240g \
    --num_executor 8 \
    --data_dir /path/to/the/folder/of/preprocessed_data \
    --model_dir /path/to/the/folder/to/save/trained_model 
```

* Spark yarn client mode, example command:
```bash
python train_2tower.py \
    --cluster_mode yarn \
    --num_executor 20 \
    --executor_cores 8 \
    --executor_memory 240g \
    --data_dir /path/to/the/folder/of/preprocessed_data \
    --model_dir /path/to/the/folder/to/save/trained_model 
```

__Options:__
* `cluster_mode`: The cluster mode to run the training, one of local, yarn, standalone or spark-submit. Default to be local.
* `master`: The master URL, only used when cluster_mode is standalone.
* `backend`: The backend of Orca Estimator, either ray or spark. Default to be ray.
* `executor_cores`: The number of cores to use on each node. Default to be 48.
* `executor_memory`: The amount of memory to allocate on each node. Default to be 240g.
* `num_executors`: The number of executors to use in the cluster. Default to be 8.
* `driver_cores`: The number of cores to use for the driver. Default to be 4.
* `driver_memory`: The amount of memory to allocate for the driver. Default to be 36g.
* `batch_size`: The batch size used for training. Default to be 8000.
* `data_dir`: The input data directory as well as output of embedding reindex tables.
* `model_dir`: The path to save the trained model.

## Predicting 2 tower model

* Spark local mode, example command:
```bash
python predict_2tower.py \
    --executor_cores 8 \
    --executor_memory 50g \
    --data_dir /path/to/the/folder/of/preprocessed_data \
    --model_dir /path/to/the/folder/to/load/trained_model 
```

* Spark yarn client mode, example command:
```bash
python train_2tower.py \
    --cluster_mode yarn \
    --num_executor 20 \
    --executor_cores 8 \
    --executor_memory 240g \
    --data_dir /path/to/the/folder/of/preprocessed_data \
    --model_dir /path/to/the/folder/to/load/trained_model 
```
