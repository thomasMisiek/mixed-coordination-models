# GEERTS-reproduction

The code in this repository runs the striatum-hippocampus model of Jesse Geerts, Fabian Chersi, Kimberly Stachenfeld and Neil Burgess.
The purpose of this repository is to allow the reproduction of Geerts et al (2020) results, on their simulation of Pearce's et al (1998) experiment.
Our reproduction work is described in details in the "A general model of hippocampal and dorsal striatal learning and decision making" paper.

We were not able to reproduce the authors' results without some modifications to the code and especially to the model's parameters. We thus provide here a version of the code with a new set of parameters, that is able to reproduce the results.
We tried to keep the code as close as possible to the original implementation, and brought modifications strictly when necessary.
First, we found that some parameters values were discrepant with what was specified in the authors' article. We corrected this discrepancy.
Then, we were still unable to reproduce the results. We had to hand-tune two parameters (inv_temp, gamma) in order to finally reproduce Geerts et al., (2020) results.

The necessary modifications for reproduction are:
  - Moving the pearce.py module from the "hippocampus/experiments" directory to the grandparent ../../ directory
  - Changing the hippocampus learning rate parameter from 0.1 to 0.07 (line 326, agents.py)
  - Changing the striatum learning rate parameter from 0.1 to 0.07 (line 434, agents.py)
  - Changing the arbitrator inverse temperature (inv_temp) parameter from 5 to 16 (line 30, pearce.py)
  - Changing the Successor-Representation discount factor (gamma) parameter from 0.99 to 0.95 (line 29, pearce.py)
  - Changing the Steepness of transition from MF to SR (A_alpha) parameter from 1 to 3.2 (line 22, agents.py)
  - Changing the Steepness of transition from SR to MF (A_beta) parameter from 1 to 1.1 (line 23, agents.py)

Additionally, we brought further modifications to the code to allow easier visualization of the results, lower memory consumption and faster processing.

These additional modifications were:
  - Transforming the pearce.py script into the launch_pearce() function (pearce.py)
  - Creation of the run.ipynb notebook to improve results display
  - Storing in a table the visual input that is experienced at each couple of position and head-direction of the agent, instead of computing it again at every new trial. This strongly reduced processing time (line 568 and line 581 to 591, agents.py)
  - Not recording some experimental variables that are useless for Pearce experiment, to reduce memory consumption (line 81 to 90 and line 144 to 159, agents.py)
  - Creating the 'perform_repro_pearce.py' script, for the user to launch a 100 agent experiment of both control and lesioned group, and to store the results figures and logs in a given folder.
  - Modified tqdm calls to stay in place (line 1, 115, 135, 136, pearce.py)
  - Added a display of the evolution of the escape time at the end of each data generation (fuse_plot_and_save(), pearce.py)
  - Added a environment.yml and requirements.txt files for easier setup of the environment.

## Installation

Using [GNU Guix](https://guix.gnu.org), set up the complete software
environment with:

```
$ guix time-machine -C channels.scm         \
       -- shell -m manifest.scm --container \
       --expose=../images=$HOME/path/to/local/clone/images
```

The `channels.scm` file instructs how to [replicate the exact Guix revision
used for
testing](https://guix.gnu.org/manual/en/html_node/Replicating-Guix.html),
while `manifest.scm` defines [the software
environment](https://guix.gnu.org/manual/en/html_node/Writing-Manifests.html)
of this computational experiment.

Alternatively, if you are not using Guix, install the required packages using the environment.yml file and the 'conda env create -f environment.yml', followed by the 'conda activate geerts_reproduction' command.
You can also install the required packages using the requirements.txt file and the 'pip install -r requirements.txt' command

## Usage

You can use the notebook run.ipynb, where previous calls' results are already displayed.

You can also run the simulations using the perform_repro_pearce.py script, using the 'python perform_repro_pearce.py' command.

The script can take two arguments:
 - A first optional parameter: the name of the directory where to store the results
 - A second optional parameter: how much agents to simulate for each group

Example calls:</br>
  'python perform_repro_pearce.py'
  'python perform_repro_pearce.py my_directory'</br>
  'python perform_repro_pearce.py' my_directory 100'</br>

A full simulation of Pearce et al.,(1998) experiment with 100 control agents and 100 HPC-lesioned agents takes 6 hours to complete on a laptop (i5 2.30ghz).</br>

For both the notebook and the script, the control group is simulated, then the HPC-lesioned group. The escape time of both group is then plotted on the same figure. The logs and figures are stored in the GEERTS-reproduction/results/path/ directory.

## Contributing
Please contact thomas.misiek@etu.univ-amu.fr to report any problem or if you want to add something to the code

## License
