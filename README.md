# mixed-coordination-models

mixed-coordination-models allows to simulate agents which implements the coordination models of either Geerts, Chersi, Stachenfeld and Burgess (2020), or Dollé, Sheynikhovich, Girard, Chavarriaga and Guillot (2010).
The agents can be subjected to three different spatial navigation experiments:
 - The first experiment of Pearce, Roberts and Good, 1998
 - The first experiment of Rodrigo Rodrigo, Sansa, Baradad, and Chamizo, 2006
 - The third experiment of Pearce, Roberts and Good, 1998
Both coordination models arbitrate behavioral control over two different navigation experts, a goal-directed one modeling the hippocampus (HPC), and an associative one modeling the dorsolateral striatum (DLS).

For users interested in the replication and reproduction of Geerts results from the "A general model of hippocampal and dorsal striatal learning and decision making" paper, please read the "Additional Info" paragraph at the end of the README.

Numerous classes, functions and methods in this work are partially or fully taken from Geerts, Chersi, Stachenfeld and Burgess (2020) original code.

This work is intended to offer a partial replication of the results of Geerts et al, 2020 (only the first experiment (Pearce 1998) is being replicated), while offering a less computationally expensive (both processing time and memory consumption) approach.
This work is also intended to provide a comparison between the biological plausibility of the coordination models of Geerts and Dolle, on the three tasks enumerated above.

In Geerts's coordination model, the HPC is modeled by the Successor-Representation (Dayan 1993) which uses spatial input in an allocentric frame of reference, whereas the DLS is modeled by a classical TD-learning agent using visual input in an egocentric frame of reference. At each time step, the fusion of the Q-values of the DLS and of the HPC models is computed, with both models Q-values being weighted by an integer representing the proportion of the model influence on behavior. Navigation strategies' influence over behavior evolve with training as the coordination model maintain a push-pull relationship over them upon the basis of their relative reliability.

In Dolle's coordination model, the HPC is modeled by a model-based algorithm (in this implementation the RTDP from Barto, Bradtke, and Singh, 1995) which uses spatial input in an allocentric frame of reference. The DLS is also modeled by a TD learning agent, but contrarily to Geerts, is uses visual input in the allocentric frame of reference. At each time step, a single navigation module is selected to drive behavior, in a winner takes all fashion. The coordination model learn to select a given strategy in specific situations, using spatial and visual input and a second-order associative-learning module.  

Both Geerts' and Dolle's coordination models can be instanciated with switched frame of reference and goal-directed models.
For example, Geerts' model might be set with an allocentric frame of reference, or the RTDP algorithm as its goal-directed module instead of the SR.

To find the best set of parameters for each couple of model and experiment, we created a random grid-search algorithm that is implemented in the grid-search.py module. Example calls and results are stored in the Geerts' and Dolle's notebooks.
Take note that the grid-search process require thousands of datapoints (we used 2000) to be effective. In can take up to one week to be computed on a laptop (i5 2.30ghz), if the RTDP algorithm is used as the HPC model, and roughly 2 days if the SR is selected.

## Installation

Using [GNU Guix](https://guix.gnu.org), set up the complete software
environment with:

```
$ guix time-machine -C channels.scm \
       -- shell -m manifest.scm --pure
```

The `channels.scm` file instructs how to [replicate the exact Guix revision
used for
testing](https://guix.gnu.org/manual/en/html_node/Replicating-Guix.html),
while `manifest.scm` defines [the software
environment](https://guix.gnu.org/manual/en/html_node/Writing-Manifests.html)
of this computational experiment.

Alternatively, if you are not using Guix, install the required packages using the `environment.yml` file and the `conda env create -f environment.yml`, followed by the `conda activate mixedcoord` command.
You can also install the required packages using the `requirements.txt` file and the `pip install -r requirements.txt` command.

## Usage

You can use the notebooks, where example calls are already set up, with best parameters found using the grid-search.
One notebook allows to run all experiments with Geerts' coordination model. The results of our own simulations are plotted.
The other notebook allows to run all experiments with Dolle's coordination model.

You can also run the simulations using scripts. There are three:
 - perform_full_pearce.py
 - perform_full_rodrigo.py
 - perform_full_exp3_pearce.py

Which respectively run the complete first experiment Pearce (1998), first experiment
of Rodrigo (2006) and third experiment of Pearce (1998).

All scripts take two arguments:
 - A first mandatory parameter: the name of the directory where to store the results
 - A second optional parameter: which set of parameters to use, either 'best_geerts', 'best_dolle' or 'custom'.</br>'custom' is a random set of parameters that is intended to be modified by the user to explore new models behaviors on the task. This can be done by editing the corresponding lines in the script before execution</br>
'best_geerts' is the set of parameters that was found to produce the most biologically plausible behavior using geerts' coordination model and the same seed we used to produce our papers results, thus the results should be identical to ours</br>
'best_geerts_stochastic' is the set of parameters that was found to produce the most biologically plausible behavior using geerts' coordination model and no seed, thus the results might not be exactly the same as in our article, but should be similar</br>
'best_dolle' is the set of parameters that was found to produce the most biologically plausible behavior using dolle's' coordination model</br>
the best set of parameters for each models were found using a random grid-search (see grid_search module)

Example calls:</br>
  'python perform_full_rodrigo.py my_directory'</br>
  'python perform_full_rodrigo.py my_directory best_geerts'</br>
  'python perform_full_rodrigo.py test best_dolle'</br>
  'python perform_full_rodrigo.py my_directory custom'</br>
  'python perform_full_pearce.py my_directory'</br>
  'python perform_full_pearce.py my_directory best_geerts'</br>
  'python perform_full_pearce.py test best_dolle'</br>
  'python perform_full_pearce.py my_directory custom'</br>
  'python perform_full_pearce.py my_directory best_geerts_stochastic'</br>

The third experiment of Pearce 1998 need agents trained on the first experiment to be performed, thus it is mandatory
to execute the first experiment of Pearce before executing the third. Example below.</br>
  'python perform_full_pearce.py my_directory best_geerts'</br>
  'python perform_full_exp3_pearce.py my_directory best_geerts'</br>
If your are using the custom argument, make sure that both perform_full_pearce.py and perform_full_exp3_pearce.py
custom parameters are set to exactly the same values, decimals included, or the program will fail.

A full simulation of Pearce 1998 experiment, with 100 agents implementing Geerts' model takes
30 minutes to complete on a laptop (i5 2.30ghz), with Dolle's model, 2 hours.</br>
A full simulation of Rodrigo 2006 experiment, with 100 agents implementing Geerts' model takes
45 minutes to complete on a laptop (i5 2.30ghz), with Dolle's model, 3 hours.</br>
The third experiment of Pearce 1998 takes roughly 2 minutes to end with Geerts model, whereas it takes 8 minutes with Dolle's

Additional Info : for the replication of Geerts et al, 2020 results, please open the Geerts Replication.ipynb notebook in the sources/notebooks folder, it contains everything that was used to produce the data of the "A general model of hippocampal and dorsal striatal learning and decision making" paper. You can also run the perform_full_pearce.py script which will also produce the same data, but wont run the parameter optimisation algorithm. All the other notebooks are unrelated to this work.
The grid-search results can be computed in either a stochastic or deterministic way. The determinist method (using a seed) should return the exact results we used for our replication article, while the stochastic method should return similar but not identical
results. This can be set using the parameter "random" of the random_grid_search method. The same goes for the perform_group_pearce method which can be used with either the "seed" parameter set to 1 (control) or 2 (lesioned HPC) for deterministic behavior, and
with None value for stochastic computing.

Additional Info : for the reproduction of Geerts et al, (2020) results, please open the README.md document in the GEERTS-reproduction folder.

## Contributing
Please contact thomas.misiek@etu.univ-amu.fr to report any problem or if you want to add something to the code

## License
