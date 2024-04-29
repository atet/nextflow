# [atet](https://github.com/atet) / [**_nextflow_**](https://github.com/atet/nextflow/blob/main/README.md#atet--nextflow)

[![.img/logo_nextflow.png](.img/logo_nextflow.png)](#nolink)

# The Most Straightforward Introduction to Nextflow for Compute Pipelines

These instructions will get you familiar with Nextflow by quickly creating a pipeline that leverages a couple different programming languages to perform a straightforward task; *use this as a starting template for your own projects*.

Excluding time to download and processing your animation, _**you will be able to complete this tutorial in ~10 minutes**_.

--------------------------------------------------------------------------------------------------

## Table of Contents

* [0. Requirements](#0-requirements)
* [0. Introduction](#0-introduction)

### Supplemental

* [Other Resources](#other-resources)
* [Troubleshooting](#troubleshooting)

--------------------------------------------------------------------------------------------------

## 0. Requirements

- This tutorial was developed on Ubuntu 22.04 on Windows Subsystem for Linux 2 (WSL2) within Microsoft Windows 10
- Instructions here should work on any standalone Debian-based operating systems, but if you would like to use Windows 10 or 11, you can follow my tutorial for WSL2 here: https://github.com/atet/wsl

[Back to Top](#table-of-contents)

--------------------------------------------------------------------------------------------------

## 1. Introduction

Nextflow is used to create easily shareable, reproducible, and scalable pipelines for processing anything from life sciences data (e.g., genome sequencing) to custom compute processing (e.g., machine learning).

Though Nextflow has been released as an open-source project since 2013, it hasn't been until almost 2020 when the community hit critical mass with improved documentation, training materials, and unified community to share knowledge.

Before we get started, a big caveat is that as of Nextflow version 22.12.0  in December 2022, their **domain specific language version 1 (DSL1) is no longer supported**. So years of scripts you may find on the internet made in DSL1 will not work in recent versions of Nextflow and **you must use version 2 (DSL2)**. The code used here will be in DSL2.

[Back to Top](#table-of-contents)

-------------------------------------------------------------------------------------------------

## Other Resources

**Description** | **URL Link**
--- | ---
Github for Nextflow | https://github.com/nextflow-io/nextflow
Installing Windows Subsystem for Linux 2 on Windows 10 | https://github.com/atet/wsl
Community-created Nextflow pipelines | https://nf-co.re
Converting scripts from DSL1 to DSL2 | https://www.nextflow.io/docs/latest/dsl1.html

[Back to Top](#table-of-contents)

--------------------------------------------------------------------------------------------------

## Troubleshooting

Issue | Solution
--- | ---
**"It's not working!"** | This concise tutorial has distilled hours of sweat, tears, and troubleshooting; _it can't not work_
**"Pipeline is crashing due to some '*DSL1*' nonsense!"** | If you have a more current version of Nextflow (after 2022), it cannot run DSL1 scripts and you must [convert your scripts to DSL2](https://www.nextflow.io/docs/latest/dsl1.html)

[Back to Top](#table-of-contents)

--------------------------------------------------------------------------------------------------

<p align="center">Copyright © 2024-∞ Athit Kao, <a href="http://www.athitkao.com/tos.html" target="_blank">Terms and Conditions</a></p>
