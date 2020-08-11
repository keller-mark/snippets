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
