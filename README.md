# code-anomaly-detection

Program for anomaly detection in Kotlin source code.

**[Go to code anomaly detection page](https://petukhovvictor.github.io/code-anomaly-detection/)**

## Description

Anomaly is Kotlin source code file, witch according to machine learning algorithms deviates from the norm.

Program consist of four top-level parts:
- parsing Kotlin source codes: Kotlin compiler run;
- CST (concrete syntax tree) extraction and factorization: CSTs transformation to a vectors by specified features configuration;
- autoencoding: run autoencoder neural network on the dataset obtained from the previous stage (vector set);
- anomaly selection based on the decoding losses obtained from the autoencoder (DBScan or 3/5-sigma deviation).

These parts correspond to three tools included in code-anomaly-detection as submodules: [kotlin-source2ast](https://github.com/PetukhovVictor/kotlin-source2ast), [ast-set2matrix](https://github.com/PetukhovVictor/ast-set2matrix) and [anomaly-detection](https://github.com/PetukhovVictor/anomaly-detection).

## Program use

You run the program on the project you are interested in by specifying the project folder. The program analyzes only files with the `kt` extension.

If the program finds anomalies, then it is written to the specified file.

Before run program, you need to initialize and update the git submodules:
```
git submodule update --init --recursive --remote
```

### Program arguments

* `-i` or `--input_folder`: path to your project on Kotlin (or just containing kotlin source code files);
* `-o` or `--output_file`: file in which paths to anomalistic files will be written, if they are found.

### Example of use

```
python3 main.py -i ~/IdeaProjects/kotlin-native -o ./anomalies.json
```

While working, the program creates a temporary folder `data` with intermediate analysis results in its own folder. Please do not remove it. The program will automatically remove it after the completion of its work.

### Copying anomalistic files to specified directory

You can use `anomalistic_files_extractor.sh` script to copy anomalistic files (by paths in `anomalies.json`) to specified directory.

#### Script arguments

* `-i` or `--anomalies_file`: path to file with found anomalies (generated by code-anomaly-detection);
* `-o` or `--anomalies_folder`: path to folder in which will be copied anomalistic files;
* `-c` or `--code_folder`: path to project which you specify for code-anomaly-detection.

#### Example of use

```
chmod +x ./anomalistic_files_extractor.sh
```
```
./anomalistic_files_extractor.sh -i ./anomalies.json -o ./anomalies -c ~/IdeaProjects/kotlin-native
```
The specified folder will be automatically created if not exist.

File names will be contain anomaly factor and index number.

## Kotlin CST factorization config

To factorize the Kotlin CST, a configuration file `features_config.json` is used that lists the extracting features.

You can change extracting features configuration. See more: [ast2vec readme](https://github.com/PetukhovVictor/ast2vec#feature-configuration)

## Configure algorithm parameters

To configure the parameters of the algorithms you can use the tools used in the `code-anomaly-decetion` yourself.
See:
- [kotlin-source2ast](https://github.com/PetukhovVictor/kotlin-source2ast),
- [ast-set2matrix](https://github.com/PetukhovVictor/ast-set2matrix),
- [anomaly-detectyion](https://github.com/PetukhovVictor/anomaly-detection).
