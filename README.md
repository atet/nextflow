# [atet](https://github.com/atet) / [**_nextflow_**](https://github.com/atet/nextflow/blob/main/README.md#atet--nextflow)

[![.img/logo_nextflow.png](.img/logo_nextflow.png)](#nolink)

# The Most Straightforward Introduction to Nextflow for Compute Pipelines

These instructions will get you familiar with Nextflow by quickly creating a pipeline that leverages a couple different programming languages to perform a straightforward task; *use this as a starting template for your own projects*.

Excluding time to download and processing your animation, _**you will be able to complete this tutorial in ~10 minutes**_.

----------------------------------------------------------------------------

## Table of Contents

* [0. Requirements](#0-requirements)
* [1. Introduction](#1-introduction)
* [2. Installation](#2-installation)
* [3. Basic Examples](#3-basic-examples)
* [4. Mixed Programming Language Examples](#4-mixed-programming-language-examples)

### Supplemental

* [Other Resources](#other-resources)
* [Troubleshooting](#troubleshooting)

----------------------------------------------------------------------------

## 0. Requirements

- This tutorial was developed on Ubuntu 22.04 on Windows Subsystem for Linux 2 (WSL2) within Microsoft Windows 10
- Instructions here should work on any standalone Debian-based operating systems, but if you would like to use Windows 10 or 11, you can follow my tutorial for WSL2 installation here: https://github.com/atet/wsl
- Your user must have `sudo` permissions for the various installations and configurations

[Back to Top](#table-of-contents)

----------------------------------------------------------------------------

## 1. Introduction

Nextflow is used to create easily shareable, reproducible, and scalable pipelines for processing anything from life sciences data (e.g., genome sequencing) to custom compute processing (e.g., machine learning).

Though Nextflow has been released as an open-source project since 2013, it hasn't been until almost 2020 when the community hit critical mass with improved documentation, training materials, and unified community to share knowledge.

Before we get started, a big caveat is that as of Nextflow version `22.12.0` in December 2022, their **domain specific language version 1 (DSL1) is no longer supported**. So years of scripts you may find on the internet made in DSL1 will not work in recent versions of Nextflow and **you must use version 2 (DSL2)**. The pipeline script used here will be in DSL2.

Though Nextflow can easily scale to containerized ecosystems that may be spread across large compute clusters in your local data center or on the cloud, this introductory pipeline will simply execute locally on your computer.

[Back to Top](#table-of-contents)

----------------------------------------------------------------------------

## 2. Installation

We will install the Java dependency, the Nextflow program, and run an example script to ensure the installation was successful:

2.1. Install latest Java version (as of 20240428) and confirm installation:

```bash
$ sudo apt update && sudo apt -y install openjdk-21-jdk-headless && java --version

.
.
.
openjdk 21.0.2 2024-01-16
OpenJDK Runtime Environment (build 21.0.2+13-Ubuntu-120.04.1)
OpenJDK 64-Bit Server VM (build 21.0.2+13-Ubuntu-120.04.1, mixed mode, sharing)
```

2.2. Add environmental variable for the new Java installation and logout for changes to take effect:

```bash
$ echo -e "\nexport JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64" | sudo tee -a ~/.bashrc && exit
```

2.3. **Log back in** and confirm environmental variable setting for `JAVA_HOME` took effect:

```bash
$ echo $JAVA_HOME

/usr/lib/jvm/java-21-openjdk-amd64
```

2.4. Install Nextflow:

```bash
$ curl -s https://get.nextflow.io | bash
```

2.5. Add environmental variable for where Nextflow was installed and logout for changes to take effect:

```bash
$ echo "export PATH=~/:\$PATH" | sudo tee -a ~/.bashrc && exit
```

2.6. **Log back in** and confirm environmental variable setting for Nextflow took effect:

```bash
$ nextflow -version

   N E X T F L O W
   version 23.10.1 build 5891
   created 12-01-2024 22:01 UTC (14:01 PDT)
   cite doi:10.1038/nbt.3820
   http://nextflow.io
```

2.7. Test installation with the "Hello World!" Nextflow pipeline (you must be connected to the internet as Nextflow will download this script from their GitHub):

```bash
$ nextflow run hello

N E X T F L O W  ~  version 23.10.1
Pulling nextflow-io/hello ...
 downloaded from https://github.com/nextflow-io/hello.git
Launching `https://github.com/nextflow-io/hello` [friendly_mayer] DSL2 - revision: 7588c46ffe [master]
executor >  local (4)
[07/00891b] process > sayHello (2) [100%] 4 of 4 ✔
Hello world!

Bonjour world!

Hola world!

Ciao world!
```

Congratulations! If you saw similar output on the last step (words may be in different order), Nextflow is installed and working correctly on your computer.

[Back to Top](#table-of-contents)

----------------------------------------------------------------------------

## 3. Basic Examples

Let's inspect the "Hello World!" pipeline that we ran in the previous step (you can find this code on Nextflow's GitHub at https://github.com/nextflow-io/hello/blob/master/main.nf):

```bash
#!/usr/bin/env nextflow

process sayHello {
  input: 
    val x
  output:
    stdout
  script:
    """
    echo '$x world!'
    """
}

workflow {
  Channel.of('Bonjour', 'Ciao', 'Hello', 'Hola') | sayHello | view
}
```

We have a single **process** called `sayHello` that will:
- take an input string `x`
- drops it into a shell command
- that prepends it to `world!`
- and outputs to the next step

We have a single **workflow** (unnamed) that:
- takes a data struture (i.e., `Channel.of`) with strings (`Bonjour`, etc.)
- uses those strings as input into the `sayHello` process
- and outputs all the appended text to the console

Now let us make some simple modifications to this script, save it locally on your computer, and run it:

3.1. Create a new file called `hello2.nf` in your home directory:

```bash
$ cd ~ && nano hello2.nf
```

3.2. Copy and paste the below to the newly created file and press `CTRL+o`, `<ENTER>`, and `CTRL+x` to save and exit:

```bash
#!/usr/bin/env nextflow

ch1 = Channel.of('Bonjour', 'Ciao', 'Hello', 'Hola')
ch2 = Channel.of('le monde!', 'mondo!', 'world!', 'mundo!')

process sayHello {
   fair true
   input: 
      val x
      val y
   output:
      stdout
   script:
      """
      echo -n '$x $y'
      """
}

workflow {
   sayHello(ch1, ch2).view()
}
```

Changes above include:
- moving **channel** data up to the top as a global variable
- adding another channel (i.e., `ch2`) of the word "world" in the different languages
- using "`fair true`" to indicate that the strings must be executed in order 
- including another input for `sayHello` process as `val y`
- changing the `echo` output script to concatenate `x` and `y` (n.b., the '`-n`' flag is necessary to ensure the '`!`' is properly escaped)
- removing the pipe workflow (i.e., not using '`|`' format)

3.3. Run your new local `hello2.nf` script:

```bash
$ nextflow run hello2.nf

N E X T F L O W  ~  version 23.10.1
Launching `hello2.nf` [berserk_archimedes] DSL2 - revision: 44203c7d6e
executor >  local (4)
[db/5f5f97] process > sayHello (1) [100%] 4 of 4 ✔
Bonjour le monde!
Ciao mondo!
Hello world!
Hola mundo!
```

Congratulations! You learned about how to create workflow scripts locally, make changes (data, input, and output), and run the pipeline locally on your computer.

[Back to Top](#table-of-contents)

----------------------------------------------------------------------------

## 4. Mixed Programming Language Examples

We will now make some further edits to `hello2.nf` to leverage external scripts that will be in different programming languages. Don't worry though, no need to know these other programming languages, just **copy and paste**!

If you are using WSL2 like me, you should already have Python programming language installed:

```bash
$ python3 --version

Python 3.10.12
```

4.1. Create a new file called `hello3.nf` in your home directory:

```bash
$ cd ~ && nano hello3.nf
```

4.2. Copy and paste the below to the newly created file and press `CTRL+o`, `<ENTER>`, and `CTRL+x` to save and exit:

```bash
#!/usr/bin/env nextflow

ch1 = Channel.of('Bonjour', 'Ciao', 'Hello', 'Hola')
ch2 = Channel.of('le monde', 'mondo', 'world', 'mundo')

process perlTask {
   fair true
   input:
      val x
      val y
   output:
      stdout
   script:
      """
      #!/usr/bin/perl

      print '$x' . ' ' . '$y';
      """
}

process pyTask {
   fair true
   input:
      val z
   output:
      stdout
   script:
      """
      #!/usr/bin/env python3

      print("$z" + "!")
      """
}

workflow {
   strings = perlTask(ch1, ch2)
   sentences = pyTask(strings)
   sentences.view()
}
```

Changes above include:
- using two processes that are dependent on each other
- starts with `perlTask` (in Perl language) combining words from `ch1` and `ch2`
- ends with `pyTask` (in Python language) taking `perlTask` output and adding an exclamation point
- crafting the workflow like traditional programming instead of piping and/or nesting processes

4.3. Run your new local `hello3.nf` script:

```bash
$ nextflow run hello3.nf

N E X T F L O W  ~  version 23.10.1
Launching `hello3.nf` [big_shaw] DSL2 - revision: 2ce38ab8a5
executor >  local (8)
[f6/7d0c2c] process > perlTask (1) [100%] 4 of 4 ✔
[6e/39d326] process > pyTask (4)   [100%] 4 of 4 ✔
Bonjour le monde!

Ciao mondo!

Hello world!

Hola mundo!

```

Congratulations! You got to use two entirely different programming languages in your Nextflow pipeline. Though we just used Perl and Python, the sky is the limit and virtually any programming language could be leveraged here.

[Back to Top](#table-of-contents)

----------------------------------------------------------------------------

## Other Resources

**Description** | **URL Link**
--- | ---
Github for Nextflow | https://github.com/nextflow-io/nextflow
Installing Windows Subsystem for Linux 2 on Windows 10 | https://github.com/atet/wsl
Community-created Nextflow pipelines | https://nf-co.re
Converting scripts from DSL1 to DSL2 | https://www.nextflow.io/docs/latest/dsl1.html

[Back to Top](#table-of-contents)

----------------------------------------------------------------------------

## Troubleshooting

Issue | Solution
--- | ---
**"It's not working!"** | This concise tutorial has distilled hours of sweat, tears, and troubleshooting; _it can't not work_
**"Pipeline is crashing due to some '*DSL1*' nonsense!"** | If you have a more current version of Nextflow (after 2022), it cannot run DSL1 scripts and you must [convert your scripts to DSL2](https://www.nextflow.io/docs/latest/dsl1.html)

[Back to Top](#table-of-contents)

----------------------------------------------------------------------------

<p align="center">Copyright © 2024-∞ Athit Kao, <a href="http://www.athitkao.com/tos.html" target="_blank">Terms and Conditions</a></p>
