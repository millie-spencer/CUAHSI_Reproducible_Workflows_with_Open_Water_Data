# Setting up the course notebooks on your computer

*Reproducible Workflows with Open Water Data: NASA, NOAA, USGS River Data Access* (CUAHSI)

This guide gets you from a HydroShare download to a running notebook and, most of it waiting for the environment to build. You only do it once. All the course notebooks share a single conda environment, `open-water-data`.

You don't need Git or GitHub. Everything comes from this HydroShare resource.

## What you'll end up with

- A folder on your computer with the notebooks and `environment.yml`.
- A conda environment named `open-water-data` with every package the notebooks use.
- VS Code (or your choice of IDE) set up to run the notebooks with that environment.

## Before you start

- **Accounts.** 
1. HydroShare Account ([signup here](https://auth.cuahsi.org/realms/CUAHSI/protocol/openid-connect/registrations?response_type=code&scope=openid+email&client_id=hydroshare&redirect_uri=https%3A%2F%2Fwww.hydroshare.org%2Foidc%2Fcallback%2F&state=yXyadHFhWcL2ODyF3SQZHHofXLz3tQCX&nonce=xKJf4bpsBH3LAdlXQKBvHXq3O9AOp0wy)).
2. HydroShare cloud computing group  ([request to join here](https://www.hydroshare.org/group/156)).
3. The SWOT lessons need a free NASA Earthdata Login account ([urs.earthdata.nasa.gov](https://urs.earthdata.nasa.gov)).
4. A USGS Water Data API key is optional but raises your rate limit ([signup page](https://api.waterdata.usgs.gov/signup/)). You can create these any time before you reach the lesson that needs them.
- **Internet connection** for the build and for the data downloads.

## Step 1. Install the tools (once)

If you already have these, skip ahead. For this tutorial, I will be using VS Code as the example, but if you have a different IDE you prefer choose that.

1.  **VS Code.** Download it from [code.visualstudio.com](https://code.visualstudio.com/).
2.  **VS Code extensions.** Open VS Code, click the Extensions icon in the left sidebar, and install **Python** and **Jupyter** (both from Microsoft).
3.  **Conda.** Install [Miniforge](https://github.com/conda-forge/miniforge) (or use any conda you already have). After installing, **close and reopen VS Code** so it can find conda.

To check that conda works, open a terminal (see Step 3) and run:

``` bash
conda --version
```

It should print a version number. If it says `conda: command not found` (Mac/Linux) or `conda is not recognized` (Windows), see Troubleshooting.

## Step 2. Download and unzip the resource

1.  On the HydroShare resource page, download the content.
<div style="display: flex; flex-direction: row; justify-content: center; align-items: center;">
    <img src="https://www.hydroshare.org/resource/1e6700fb49ca4aa2ba372fe11b093ace/data/contents/Images/download_button.png" alt="The download button on the HydroShare resource page" width="700">
</div>
2.  **Unzip it and rename the folder something from understandable (when you download the resource, it takes the resource id as its name).**
3.  Inside the unzipped folder, find the folder that contains `environment.yml` and the notebooks folder. HydroShare downloads often nest the files inside `data/contents/`.

## Step 3. Open the course folder in VS Code

1.  In VS Code, choose **File → Open Folder** and select your course folder.
2.  Open a terminal inside VS Code with **Terminal → New Terminal**. It starts in your course folder.
3.  Check that you're in the right place:

``` bash
ls
```

(On Windows PowerShell, `dir` also works.) You should see `environment.yml` in the list. If you don't, you're in the wrong folder: go back to Step 2.

## Step 4. Create the environment

Copy the path of the environment file and then, 
<div style="display: flex; flex-direction: row; justify-content: center; align-items: center;">
    <img src="https://www.hydroshare.org/resource/1e6700fb49ca4aa2ba372fe11b093ace/data/contents/Images/copy_path.png" alt="Copying the path to a file or folder" width="700">
</div>
in the terminal, run:


``` bash
conda env create -f <insert path to environment.yaml file>```

This can take several minutes. Wait for the prompt to return. When it finishes, confirm the environment exists:

``` bash
conda env list
```

You should see `open-water-data` in the list.

A few things that trip people up:

- `-f` must point to the file `environment.yml`, not to a folder.
- If you see `No space left on device`, free up space and run the command again. A failed build can leave a half-built environment behind, so run `conda env remove -n open-water-data` first, then rebuild.

## Step 5. Select the kernel in VS Code

1.  Open the sample notebook (`.ipynb`) from the file list on the left.
2.  Click **Select Kernel** at the top right of the notebook.
3.  Choose **Python Environments**, then **open-water-data**.

The kernel name should now appear at the top right of the notebook.

If `open-water-data` isn't in the list, try these in order:

1.  Reload the VS Code window (**Developer: Reload Window** in the Command Palette, `Ctrl+Shift+P` / `Cmd+Shift+P`).
2.  Run **Python: Select Interpreter** from the Command Palette and choose the `open-water-data` environment.
3.  Register the environment as a Jupyter kernel by hand, then reload VS Code:

``` bash
conda run -n open-water-data python -m ipykernel install --user --name open-water-data --display-name "Python (open-water-data)"
```

## Step 6. Check that it works
The resource includes a short practice notebook, `00_practice_check-your-setup.ipynb`. It takes about 10 minutes, needs no accounts or keys, and tells you whether your setup is ready for the course notebooks. Do this before you open a lesson.
 
1. Open `00_practice_check-your-setup.ipynb` in VS Code and select the **Python (open-water-data)** kernel (Step 5).
2. Run the cells in order, one at a time, with **Shift+Enter**.

## Troubleshooting

**`conda` is not recognized / command not found.** Close and reopen VS Code after installing conda. If it still fails, run `conda init` for your shell (`conda init zsh` on Mac, `conda init powershell` on Windows), then open a new terminal. On Windows you can also use the "Miniforge Prompt" from the Start menu.

**`EnvironmentFileNotFound` or "file not found" for `environment.yml`.** Your terminal isn't in the course folder. Run `pwd` (Mac/Linux) or `cd` (Windows) to see where you are, and `ls` to check that `environment.yml` is listed.

**`The environment file(s) do not specify a name or prefix`.** You pointed `-f` at a folder instead of the `environment.yml` file.

**`zsh: parse error near` or similar on a Mac.** You typed the angle brackets from a placeholder. Remove them.

**`No module named ipykernel`.** The command ran in a Python that doesn't have `ipykernel`, usually the `base` environment, or the environment hasn't been built yet. Run `conda env list` to check that `open-water-data` exists, and use `conda run -n open-water-data ...` when registering the kernel.

**Kernel doesn't show up in VS Code.** See the options at the end of Step 5.

**`No space left on device` during the build.** Free up disk space. Then run `conda clean --all -y`, remove any half-built environment with `conda env remove -n open-water-data`, and rebuild.

**`FileNotFoundError: [WinError 3]` on Windows when downloading SWOT files.** Your folder path is too long. Move the course folder to something short like `C:\fwdc\` and run the notebook again from there.

**A result doesn't match the "what you should see" text.** Some values change from day to day: provisional USGS values can be revised, NWM forecasts keep only the newest runs, and SWOT data are reprocessed. Each notebook says which results can change.

## Getting help

If you run into any issues you cannot resolve, email me at kwong@cuahsi.org.
