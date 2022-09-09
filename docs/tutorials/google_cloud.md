## Setting up rsync gsutils file transfer

* install a faster md5-check sum to increase overall upload via:

    ```bash
    sudo apt-get install gcc python-dev python-setuptools python-pip
    sudo pip uninstall crcmod
    sudo pip install --no-cache-dir -U crcmod
    # this should be true now
    sudo pip install --no-cache-dir -U crcmod | grep "compiled crcmod:"
    ```

## rsync to google drive

### simple example

* more simplistic command:
  * upload all .fastq files within `LOKAL-DIR/` to the bucket

```bash
gsutil -o "GSUtil:parallel_process_count=8" -o "GSUtil:parallel_thread_count=1" \
       -m rsync -j .fastq -c LOKAL-DIR/ gs://BUCKETNAME/DIR/
```

### More complex example

* recursively upload everything in `01-10_GERMANY` to `gs://nextflow-pipeline/metagenome-fastq/01-GER`
* with compression for fastq files `-j .fastq`, recursive `-r`, checksum `-c`, exclude certain pattern for upload `-x` 

```bash
gsutil -o "GSUtil:parallel_process_count=8" \
       -o "GSUtil:parallel_thread_count=1" \
       -m rsync -j .fastq -c -r -x ".*\.js$|.*\.txt$|.*\.log$|.*\.png$|.*\.html$|.*\.log$|.*\.pdf$" \
       01-10_GERMANY gs://nextflow-pipeline/metagenome-fastq/01-GER
```

`-n` is a dry mode -> tells you what it **would** do
