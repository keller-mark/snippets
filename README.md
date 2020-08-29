# snippets

may or may not be relevant to others

## shell

### start an interactive job with slurm on O2

```sh
srun -p interactive --pty -t 8:00:00 -n 4 --mem 16G bash
```

### download a file with curl

```sh
curl -L -o {output} {file_url}
```

### tar.gz a directory

```sh
tar -cvzf dir.tar.gz dir/
```

### un-tar.gz a file

```sh
tar -xvzf dir.tar.gz
# or
tar -xvzf dir.tar.gz -C dest_dir
```

### serve a local directory over HTTP

```sh
# brew install http-server
http-server --cors='*' --port 8000 .
```

### create conda env

```sh
conda env create -f environment.yml
```

### create conda env

```sh
conda env update -f environment.yml
```

### remove conda env

```sh
conda env remove -n ENV_NAME
```

### revert to a particular git commit

```sh
git revert --no-commit b960c8e5..HEAD
```
