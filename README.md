# snippets

may or may not be relevant to others

## shell

## personal O2 things

```sh
ssh me@o2.hms.harvard.edu
ssh login01
tmux
source ~/.bashrc_mark
ssh-add ~/.ssh/id_rsa_github
```

### serve directory of files

```sh
ssh -L O2_PORT:127.0.0.1:LOCAL_PORT me@o2.hms.harvard.edu
ssh -L O2_PORT:127.0.0.1:O2_PORT login01
srun -t 0-3:00 --pty -p interactive --tunnel O2_PORT:O2_PORT /bin/bash
# cd to some directory with files
http-server --cors='*' --port O2_PORT .
```

### start an interactive job with slurm on O2

```sh
srun -p interactive --pty -t 8:00:00 -n 4 --mem 16G bash
```

### start an interactive job with slurm on O2 with 1 GPU

```sh
srun -p gpu --gres=gpu:1 --pty -t 8:00:00 -n 4 --mem 16G bash
module load gcc/9.2.0 cuda/11.7 # Needs same version of CUDA that was installed into Python environment
```

### run snakemake with time-based rerun triggers

```sh
snakemake -j 1 --rerun-triggers mtime
```


### run snakemake with time-based rerun triggers on a cluster filesystem

```sh
snakemake -j 1 --rerun-triggers mtime --latency-wait 30
```

### copy a local directory to google cloud storage

This will create a copy at `gs://{bucket}/{path}/local_dir`

```sh
gcloud auth login
gsutil -m cp -r ./local_dir gs://{bucket}/{path}
```

### copy from google cloud storage bucket to local

This will create a copy at `./remote_dir`

```sh
gcloud auth login
gsutil -m cp -r gs://{bucket}/{path}/remote_dir .
```

### download a file with curl

```sh
curl -L -o {output} {file_url}

# put quotes around file_url if it has characters that might be misinterpreted by the shell (e.g., '&')
curl -L -o {output} "{file_url}"
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

### un-gz a file

```sh
gunzip -c {input} > {output}
```

### serve a local directory over HTTP

```sh
# brew install http-server
http-server --cors='*' --port 8000 .
```

### create conda env

```sh
mamba env create -f environment.yml
```

or

```sh
conda env create -f environment.yml
```

### create conda env without environment file

```sh
conda create -n NAME python=3.11
```

### create conda env in particular directory

```sh
conda create --prefix=/path/to/envs/env_name python=3.11
conda config --append envs_dirs /path/to/envs
```

### list conda envs

```sh
conda info --envs
```


### configure mamba as conda solver

```sh
conda install -n base conda-libmamba-solver
conda config --set solver libmamba
```

### create conda env for R

```sh
conda config --add channels r
conda create -n NAME
conda activate NAME
conda install r-base r-essentials r-irkernel
```

### update conda env

```sh
mamba env update -f environment.yml
```

or

```sh
conda env update -f environment.yml
```

### remove conda env

```sh
conda env remove -n NAME
```

### update conda version

```sh
conda update -n base conda
```

### symlink a file or directory

```sh
ln -s /path/to/reference-file symlink-file
```

### revert to a particular git commit

```sh
git revert --no-commit b960c8e5..HEAD
```

### change git case sensitivity

```sh
git config core.ignorecase false
```

### authorize a [direnv](https://github.com/direnv/direnv)

```sh
direnv allow .
```

### create a [direnv](https://github.com/direnv/direnv) for conda

in `.envrc`

```sh
use conda NAME
```

### install a LaTeX package from CTAN with tlmgr

```sh
tlmgr install chemfig
```

### convert mov to gif

```sh
ffmpeg -i input.mov -r 10 -pix_fmt rgb24 output.gif
```

### checkout a single file from another git branch

```sh
# For example, if we want `some-file.js` from the `main` branch
git checkout main -- some-file.js
```

### pip install from a github branch with extras_require

```sh
pip install 'vitessce[all] @ git+https://github.com/vitessce/vitessce-python@main'
pip install 'SomePackage[PDF] @ git+https://git.repo/SomePackage@main#subdirectory=subdir_path'
```

### check version of installed python package

```python
from importlib.metadata import version
version('package_name')
```

### use `ripgrep` to find all cited bibtex keys

```sh
rg '\\cite\{([\w,]+)\}' -g '*.tex' -r '$1' --only-matching -N -I
```

### install `numba` with `uv`

```sh
uv add --no-build-package numba numba
```

### convert an OME-TIFF to a pyramidal OME-TIFF

```sh
export INPUT_TIFF=./image.ome.tiff
export OUTPUT_TIFF=./image.pyramid.ome.tif
bioformats2raw $INPUT_TIFF ./intermediate.zarr --resolutions 6 --series 0
raw2ometiff ./intermediate.zarr $OUTPUT_TIFF --compression LZW
generate_tiff_offsets --input_file $OUTPUT_TIFF
```

### tmux

#### list

```sh
tmux ls
```

#### attach to existing session 0

```sh
tmux a -t 0
```

#### attach to existing session 0, read-only

```sh
tmux a -t 0 -r
```

#### detach from session

<kbd>CTRL</kbd>+<kbd>B</kbd> <kbd>D</kbd>

## altair

### basics

```python
plot = alt.Chart(df).mark_bar().encode(
    x=alt.X("x_colname:N"),
    y=alt.Y("y_colname:Q"),
    color=alt.Color("color_colname:N"),
).properties(
    width=200,
    height=200
)

plot.save("my_plot.png")
plot.save("my_plot.svg)

plot
```

### title and subtitle

```python
# ...
).properties(
    title={
        "text": "Title here",
        "subtitle": "Subtitle here",
        "fontSize": 16,
        "fontWeight": 500,
        "subtitleColor": "black",
        "subtitleFontSize": 12
    }
)
```

### sorting/ordering things

```python
    # x, y, color, etc.
    x=alt.X("x_colname:N", scale=alt.Scale(domain=sorted_x_vals)),
    # row, column
    row=alt.Row("row_colname:N", sort=sorted_row_vals),
```

### axis titles

```python
    # x, y, etc.
    x=alt.X("x_colname:N", axis=alt.Axis(title="Title Here")),
    # color
    color=alt.Color("color_colname:N", legend=alt.Legend(title="Title Here")),
    # row, column
    row=alt.Row("row_colname:N", header=alt.Header(title="Title Here")),
```

### using row/column with errorbar/errorband

layer first, then facet

```python
base = alt.Chart(df)

lines = base.mark_line(point=True).encode(
    x=alt.X("year:O", axis=alt.Axis(title="Year")),
    y=alt.Y("percent:Q"),
    color=alt.Color("method_or_other:N")
).properties(
    height=200,
    width=800
)

error_bands = base.mark_errorband().encode(
    x=alt.X("year:O"),
    y=alt.Y("pct_lower:Q", axis=alt.Axis(title="Percent")),
    y2=alt.Y2("pct_upper:Q"),
    color=alt.Color("method_or_other:N")
)

alt.layer(lines, error_bands, data=df).facet(
    row=alt.Row(
        "field:N",
        sort=sorted_field_df.index.tolist(),
        header=alt.Header(
            title="Subject Area",
            labelAngle=0,
            labelAlign="left",
        )
    )
)
```

### choropleth: joining on state name

```python
import altair as alt
from vega_datasets import data

state_df = pd.read_csv(data.population_engineers_hurricanes.url)
state_to_id = dict(zip(state_df["state"].values.tolist(), state_df["id"].values.tolist()))

# df is some dataframe with column 'my_field'
df["id"] = df["state"].apply(lambda x: state_to_id[x])

states = alt.topo_feature(data.us_10m.url, 'states')


plot = alt.Chart(states).mark_geoshape().encode(
    color=alt.Color("my_field:N")
).transform_lookup(
    lookup='id',
    from_=alt.LookupData(data=df, key='id', fields=['my_field'])
).properties(
    width=500,
    height=300
).project(
    type='albersUsa'
)
plot
```
