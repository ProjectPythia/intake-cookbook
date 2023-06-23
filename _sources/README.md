<img src="thumbnail.svg" alt="thumbnail" width="300"/>

# Intake Cookbook

[![nightly-build](https://github.com/ProjectPythia/intake-cookbook/actions/workflows/nightly-build.yaml/badge.svg)](https://github.com/ProjectPythia/intake-cookbook/actions/workflows/nightly-build.yaml)
[![Binder](https://binder.projectpythia.org/badge_logo.svg)](https://binder.projectpythia.org/v2/gh/ProjectPythia/intake-cookbook/main?labpath=notebooks)
[![DOI](https://zenodo.org/badge/512825541.svg)](https://zenodo.org/badge/latestdoi/512825541)

This Project Pythia Cookbook covers using and creating Intake catalogs to access data.

## Motivation

This cookbook will help simplify the way you access and share data in your research. You will learn to access data using Intake catalogs and create Intake catalogs to make your data available to others.

## Authors

[James Morley](https://github.com/jnmorley/)

### Contributors

<a href="https://github.com/ProjectPythia/intake-cookbook/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=ProjectPythia/intake-cookbook" />
</a>

## Structure
This cookbook is broken up into two main sections - "Introduction to Intake" and "Creating Intake Catalogs." 

### About HRRR
High-Resolution Rapid Refresh (HRRR) is a atmospheric model maintained by [NOAA](https://www.noaa.gov/). As stated on NOAA's [website](https://rapidrefresh.noaa.gov/hrrr/)

> The HRRR is a NOAA real-time 3-km resolution, hourly updated, cloud-resolving, convection-allowing atmospheric model, initialized by 3km grids with 3km radar assimilation. Radar data is
> assimilated in the HRRR every 15 min over a 1-h period adding further detail to that provided by the hourly data assimilation from the 13km radar-enhanced Rapid Refresh.

Throughout this cookbook we use a subset of HRRR data maintained by Mesowest on AWS S3 object storage. 

### Introduction to Intake
This section describes how to use intake catalogs to access data. It shows how to find information about catalog entries, how to set user parameters, and how to use intake with Dask.

### Creating Intake Catalogs
This section walks you through the process of creating your own Intake catalogs to access Mesowest's HRRR data.

## Running the Notebooks
You can either run the notebook using [Binder](https://binder.projectpythia.org) or on your local machine.

### Running on Binder

The simplest way to interact with a Jupyter Notebook is through
[Binder](https://binder.projectpythia.org), which enables the execution of a
[Jupyter Book](https://jupyterbook.org) in the cloud. The details of how this works are not
important for now. All you need to know is how to launch a Pythia
Cookbooks chapter via Binder. Simply navigate your mouse to
the top right corner of the book chapter you are viewing and click
on the rocket ship icon, (see figure below), and be sure to select
“launch Binder”. After a moment you should be presented with a
notebook that you can interact with. I.e. you’ll be able to execute
and even change the example programs. You’ll see that the code cells
have no output at first, until you execute them by pressing
{kbd}`Shift`\+{kbd}`Enter`. Complete details on how to interact with
a live Jupyter notebook are described in [Getting Started with
Jupyter](https://foundations.projectpythia.org/foundations/getting-started-jupyter.html).

### Running on Your Own Machine
If you are interested in running this material locally on your computer, you will need to follow this workflow:
  

1. Clone the `https://github.com/ProjectPythia/intake-cookbook` repository:

   ```bash
    git clone https://github.com/ProjectPythia/intake-cookbook.git
    ```  
1. Move into the `intake-cookbook` directory
    ```bash
    cd intake-cookbook
    ```  
1. Create and activate your conda environment from the `environment.yml` file
    ```bash
    conda env create -f environment.yml
    conda activate intake-cookbook-dev
    ```  
1.  Move into the `notebooks` directory and start up Jupyterlab
    ```bash
    cd notebooks/
    jupyter lab
    ```
