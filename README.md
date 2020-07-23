# snippets

may or may not be relevant to others

## shell

### O2 slurm: start an interactive job

```sh
srun -p interactive --pty -t 8:00:00 -n 4 --mem 48G bash
```

### curl: download a file

```sh
curl -L -o {output} {file_url}
```

