**Quick links:** [compas docs](https://compas-dev.github.io/main/) | [compas_assembly docs](https://blockresearchgroup.github.io/compas_assembly/) | [compas_fab docs](https://gramaziokohler.github.io/compas_fab/latest/) | [slides](slides.pdf) | [system overview](#system-overview) | [troubleshooting](#troubleshooting)

**Progress:** [0. requirements](#requirements) | [1. installation](#getting-started) | [2. editor setup](#setting-up-your-development-environment) | [3. create assembly](#create-an-assembly---brick-wall) | [4. robotic planning](#planning-robotic-fabrication-of-assembly) | [5. robotic execution](#executing-robotic-fabrication)

# Workshop: Robotic Assembly with COMPAS framework

Materials for the Robotic Assembly workshop using COMPAS framework

> During this workshop, we will design an assembly of bricks, represented by a custom data structure, evaluate its stability and stability of the incomplete assembly at various stages of the assembly process, and plan the assembly process using a robotic arm.

## Requirements

* Operating System: **Windows 10** Pro or better <sup>(1)</sup>.
* [Rhinoceros 3D 6.0](https://www.rhino3d.com/): Focus on Rhino 6.0 only. [See here if you use Rhino 5.0](#rhino-50)
* [Anaconda Python Distribution](https://www.anaconda.com/download/): 2.7 or 3.x
* [Docker Community Edition](https://www.docker.com/get-started): Download it for [Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows) or [Mac](https://store.docker.com/editions/community/docker-ce-desktop-mac).
* X11 Server: On Windows use [XMing](https://sourceforge.net/projects/xming/), on Mac use [XQuartz](https://www.xquartz.org/) (see details [here](https://medium.com/@mreichelt/how-to-show-x11-windows-within-docker-on-mac-50759f4b65cb)).
* Git: [official command-line client](https://git-scm.com/) or visual GUI (e.g. [Github Desktop](https://desktop.github.com/) or [SourceTree](https://www.sourcetreeapp.com/))
* [ABB RobotStudio](https://new.abb.com/products/robotics/robotstudio/downloads): 6.08 (only available for Windows). After install, **make sure you add RobotWare 6.03.02** (`Add-Ins` -> `RobotApps` -> `RobotWare` and add `6.03.02`). After completing the setup, you can open the [ABB Linear Axis station](robot_station) in RobotStudio.

> Note: if you get an error, scroll down to the [Troubleshooting](#troubleshooting) section.

<sup>(1): Windows 10 Home does not support running Docker.</sup>


<details><summary>Rhino 5.0</summary>

The focus of the workshop will be on Rhino 6.0 only. While most things will work on Rhino 5.0, it is not recommended as there are several manual steps required to get the software to run.

However, if you do use Rhino 5.0, make sure to install the following:

* [Grasshopper](https://www.grasshopper3d.com/)
* [GHPython](https://www.food4rhino.com/app/ghpython)
* [IronPython 2.7.5](https://github.com/IronLanguages/main/releases/tag/ipy-2.7.5) ([see here for details about this manual update](https://compas-dev.github.io/main/environments/rhino.html#ironpython-1)).

</details>


## Getting started

![Progress](images/progress-1.png)

We will install all the required **COMPAS** packages using Anaconda. Anaconda uses **environments** to create isolated spaces for projects' depedencies, it is recommendable that you do all the exercises in a newly created environment.

First, clone this repository. You have two options:

<details><summary>1. Using a visual client <i>(e.g. SourceTree)</i></summary>
Open your GIT visual client (e.g. SourceTree), and clone the repository (on SourceTree, `File -> Clone / New`) and enter the following URL and the destination folder:

      https://github.com/gramaziokohler/robotic_assembly_workshop.git

</details>

<details><summary>2. Using git command line client</summary>
Start your Anaconda Prompt, go to the destination folder where you wish to place all the workshop material and run:

      git clone https://github.com/gramaziokohler/robotic_assembly_workshop.git

</details>

<br/>

Now, we can create the environment and install all packages. Start your Anaconda Prompt (**run as administrator**), go to the repository folder you just cloned, and run:

      conda env create -f workshop.yml -n workshop
      conda activate workshop

<details><summary>Not working?</summary>

Make sure you really changed into the repository folder. For example, if you cloned the repository into a folder called `Code` in your home directory, you should type:

**On Mac**

      cd ~/Code/robotic_assembly_workshop

**On Windows**

      cd %USERPROFILE%\Code\robotic_assembly_workshop

If the command fails because you already have an environment with the same name, choose a different one, or remove the old one before creating the new one:

      conda remove -n workshop --all

</details>

<br/>

Great! Now type `python` in your Anaconda Prompt, and test if the installation went well:

      >>> import compas
      >>> import compas_fab
      >>> import compas_assembly

If that doesn't fail, you're good to go! Exit the python interpreter (either typing `exit()` or pressing `CTRL+Z` followed by `Enter`).

Now let's make all the installed packages available inside Rhino. Still from the Anaconda Prompt, type the following:

      python -m compas_rhino.install -v 6.0 -p compas compas_ghpython compas_rhino compas_assembly compas_fab roslibpy

Congrats! 🎉 You are all set! Open Rhino and try to import `compas` to verify everything is working fine.

> **NOTE:**
> If the previous command throws an error, make sure you run the Anaconda Prompt as an **Administrator**.

## Setting up your development environment

![Progress](images/progress-2.png)

You can use any development environment that you're comfortable with, but for this workshop, we suggest using [VS Code](https://code.visualstudio.com/), since it provides very deep integration with Anaconda, Docker, Python debugging and many other niceties.

* Install [VS Code](https://code.visualstudio.com/) and open it
* Go to `Extensions` and install:
  * `Python` (official extension)
  * `EditorConfig for VS Code` (optional)
  * `Docker` (official extension, optional)
* Run your first example:
  * Open the folder where you cloned this repository with VS Code (right-click the folder from Explorer -> `Open with Code`)
  * Open the [`Hello COMPAS` example script](examples/ex00_hello_compas.py)
  * On the bottom left status bar, make sure you select the python interpreter to use. The list will contain Anaconda environments, select the one created above: `workshop`.
  * Press `F5` to run it.

---

## Exercises

By now, you should be up and running and ready to start playing with **compas**, **compas_assembly** and **compas_fab**!

### Create an Assembly - Brick wall

![Progress](images/progress-3.png)

*Basic setup*

* [x] Generate an assembly for a brick wall ([script](examples/ex06_wall_generate.py) | [plot](examples/ex06_wall_generate_plot.py))
* [x] Add assembly support plate ([script](examples/ex07_wall_support.py) | [plot](examples/ex07_wall_support_plot.py))
* [x] Identify interfaces of the assembly ([script](examples/ex08_wall_interfaces.py) | [plot](examples/ex08_wall_interfaces_plot.py))
* [x] Identify the courses of the assembly ([script](examples/ex090_wall_courses.py) | [plot](examples/ex090_wall_courses_plot.py))

*Examples of useful functions for robotic assembly*

* [x] Compute the building sequence for a selected brick ([plot](examples/ex092_wall_sequence.py) | [iplot](examples/ex092_wall_sequence_iplot.py) | [rhino](examples/ex092_wall_sequence_rhino.py))
* [x] Compute the equilibrium of the building sequence ([script](examples/ex093_wall_sequence_equilibrium.py) | [rhino](examples/ex093_wall_sequence_equilibrium_rhino.py))
* [x] Compute the hull of the building sequence ([rhino](examples/ex094_wall_sequence_hull_rhino.py))

### Planning robotic fabrication of assembly

![Progress](images/progress-4.png)

> **NOTE:** These examples need ROS with the ABB linear axis loaded:
> 1. Make sure your `X0.hosts` files is configured with your **current IP address** (<small>scroll down to [troubleshooting](#troubleshooting) for more details</small>).
> 1. Start your X11 server (`XMing` on Windows, `XQuartz` on Mac).
> 1. Run *docker-compose* to start the [`ROS ABB Linear Axis`](docker/ros-systems/ros-abb-linear-axis/docker-compose.yml) system [<small>(*need help?*)</small>](docker-help.md).

* [x] Load ABB linear axis model ([script](examples/ex50_abb_linear_axis_robot.py))
* [x] Transform assembly to work station ([script](examples/ex51_transform_assembly_to_workstation.py))
* [x] Search buildable bricks within robot's reach ([script](examples/ex52_buildable_bricks_within_reach.py) | [exercise](examples/ex52_buildable_bricks_within_reach_template.py))
* [x] Generate paths for brick building sequence ([script](examples/ex53_generate_paths_for_all_bricks.py) | [exercise](examples/ex53_generate_paths_for_all_bricks_template.py))

### Executing robotic fabrication

![Progress](images/progress-5.png)

> **NOTE:** These examples need ROS with an ABB linear axis loaded and a real or simulated (via RobotStudio) ABB controller:
> 1. Configure the `ROBOT_IP` environment variable to point to your controller.
> 1. Run the RobotStudio station and start all tasks.
> 1. Run *docker-compose* to start the [`ROS Real ABB Linear Axis`](docker/ros-systems/ros-abb-linear-axis-real/docker-compose.yml) system  [<small>(*need help?*)</small>](docker-help.md).

* [x] Execute paths for brick assembly ([script](examples/ex54_execute_paths_for_all_bricks.py))
* [x] Full example of planning + execution using Grasshopper ([grasshopper](examples/abb_linear_axis.ghx))

Did you finish up all exercises up to here? Awesome!! Have a cookie 🍪!

## Additional examples

The following is a list of additional example code to complement the exercises above:

* **Assembly - Stack**:
  * [x] Generate a stack ([script](examples/ex02_stack_generate.py))
  * [x] Identify interfaces of a stack ([script](examples/ex03_stack_interfaces.py))
  * [x] Compute contact forces required for static equilibrium of an assembly ([script](examples/ex04_stack_equilibrium.py))
  * [x] Draw stack in Rhino ([rhino](examples/ex05_stack_rhino.py))
* **Robotic fundamentals**:
  * [x] Frame orientation specs ([script](examples/ex10_frame_orientation_specs.py))
  * [x] Expressing frames in different coordinate systems ([script](examples/ex11_transformation_frame.py))
  * [x] Create a robot model programmatically ([script](examples/ex20_robot_model.py))
  * [x] Load robot model from URDF ([script](examples/ex21_robot_model_from_disk.py))
  * [x] Create a robot artist for rhino ([rhino](examples/ex22_robot_artist.py))
  * [x] Load complete robot ([script](examples/ex23_load_robot.py) | [exercise](examples/ex23_load_robot_template.py))
  * [x] Represent robot configurations / joint state ([script](examples/ex24_configuration.py))
* **Using ROS**:
  > **NOTE:** These examples need ROS:
  > 1. Run *docker-compose* to start the [`ROS Basic`](docker/ros-systems/ros-basic/docker-compose.yml) system [<small>(*need help?*)</small>](docker-help.md).
  * [x] Hello ROS ([script](examples/ex30_hello_ros.py))
  * [x] ROS Talker & Listener nodes ([talker](examples/ex31_talker.py) | [listener](examples/ex32_listener.py))
  * [x] Using ROS as mesh transport between CADs ([grasshopper mesh publisher](examples/ex34_mesh_publisher.py) | [grasshopper mesh subscriber](examples/ex34_mesh_subscriber.py))
* **Planning with robots**:
  > **NOTE:** These examples need ROS with a UR5 model loaded:
  > 1. Make sure your `X0.hosts` files is configured with your **current IP address** (<small>scroll down to [troubleshooting](#troubleshooting) for more details</small>).
  > 1. Start your X11 server (`XMing` on Windows, `XQuartz` on Mac).
  > 1. Run *docker-compose* to start the [`ROS UR5`](docker/ros-systems/ros-ur5/docker-compose.yml) system [<small>(*need help?*)</small>](docker-help.md).
  * [x] Calculate forward kinematics ([script](examples/ex40_calculate_fk.py))
  * [x] Calculate inverse kinematics ([script](examples/ex41_calculate_ik.py))
  * [x] Calculate cartesian path ([script](examples/ex42_calculate_cartesian.py))
  * [x] Calculate kinematic path ([script](examples/ex43_calculate_kinematic.py))
  * [x] Add collision object attached to the robot ([script](examples/ex44_add_attached_collision_object.py))
  * [x] Add collision object to a scene ([script](examples/ex45_add_collision_object.py))
  * [x] Calculate kinematic path with attached collision object ([script](examples/ex46_calculate_kinematic_with_aco.py))

---

## System overview

Environments? Containers? Processes? Confused? 😵 What is connected to what and how?

The following diagram shows how the different parts are interconnected and which one calls which other:

![System overview](images/overview.png)

---

## Troubleshooting

Sometimes things don't go as expected. Here are some of answers to the most common issues you might bump into:

> Q: Docker does not start. It complains virtualization not enabled in BIOS.

This is vendor specific, depending on the manufacturer of your computer, there are different ways to fix this, but usually, pressing a key (usually `F2` for Lenovo) before Windows even start will take you to the BIOS of your machine. In there, you will find a `Virtualization` tab where this feature can be enabled.

> Q: Cannot start containers, nor do anything with Docker. Error message indicates docker daemon not accessible or no response.

Make sure docker is running. Especially after a fresh install, docker does not start immediately. Go to the start menu, and start `Docker for Windows`.

> Q: `conda` commands don't work.

Try running them from the *Conda Prompt*. Depending on how you installed Anaconda, it might not be available by default on the normal Windows command prompt.

> Q: `conda activate workshop` fails

Make sure you have conda 4.6 or higher.

> Q: When trying to install the framework in Rhino, it fails indicating the lib folder of IronPython does not exist.

Make sure you have opened Rhino 6 and Grasshopper at least once, so that it finishes setting up all its internal folder structure.

> Q: It fails when trying to install on Rhino.

Try running the command prompt as administrator. Depending on the version of Python, it might be required or not.

> Q: error: Microsoft Visual C++ 14.0 is required

Follow the link to install Microsoft Visual C++ 14.0
https://www.scivision.co/python-windows-visual-c++-14-required/

> Q: When running `conda install compas_fab`, I get the error: `cannot find Frame`

You have already installed an older, pre-release version of COMPAS. Please remove it.

> Q: When installing `shapely`, I get the error: `HTTP 000 Connection Failed`

Your environment has an outdated version of OpenSSL. Go to your root environment (i.e. run `conda deactivate`) and then run the same command to install `shapely` on the workshop's environment.

> Q: Docker containers fail to open GUI tools

If you get errors like `cannot open display` or `cannot acquire screen ...` it means you have not configured the security of your X11 server correctly:

* On Windows using `XMing`, make sure you add your IP Address to the file `%ProgramFiles(x86)%\XMing\X0.hosts` (needs to be opened as administrator)
* On Mac using `XQuartz`, run `xhost +local:root` (and remember to disable later with `xhost -local:root`)

Remember to restart `XMing`/`XQuartz` after applying these changes.

> Q: How do I set environment variables?

Using [VS Code](https://code.visualstudio.com/) (also supported in some other editors), the easiest option is to place a file called `.env` in the root folder of your workspace (i.e. this repository) and enter the variables there, one variable per line, e.g. `ROBOT_IP=10.0.75.1`.

> Q: Python crashes when i run a *Plotter* example

This may be related to the version of Matplotlib.
For example, we have seen this problem on OSX with Matplotlib version 3+.
Try downgrading to `2.1.0`: `conda install matplotlib=2.1.0`. Make sure the correct environment is active...

