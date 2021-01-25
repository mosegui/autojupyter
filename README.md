# Portable Jupyter Environment for Data Science

This simple combination of a Docker image featuring a Jupyter service and a BASH script provides a one-command solution for firing a standaradized Jupyter development environment that will also automatically stop the container once the analyst is finished with his/her session. This way, analysts do not have not worry about manually managing Docker/Podman services or cleaning after themselves.

This construct easily allows for having Jupyter Notebooks automatically integrated in the CI/CD environment. Hereby, the Docker image providing the Jupyter service can have as base image an IDE Docker remote interpreter, and/or Docker images dedicated to testing, building or PR-merging, ensuring an homogeneous environment throughout the software/algorithm lifecycle.


## Getting Started

### Description

The service works via the combination of a bash script and a Docker container. The bash script `autojupyter` starts the Docker container serving Jupyter as well as it opens a new `chomium-browser` window. The bash script compares the existing `chromium-browser` process IDs (PIDs) before and after launching the service, in order to know which PIDs correspond to the newly opened browser window. The bash script then continously monitors whether these new PIDs continue to exist. As soon as one of the new PIDs ceases to exist (user closes broswer window) the script automatically finishes the Docker container.

The bash script automatically mounts to the Docker container the volume from which the `autojupyter` was called. This way, the analyst can fire `autojupyter` e. g. from where the data is located and have the data automatically available in the Jupyter environment.

See the demo:

![demo](video/demo.gif)


### Installation

- Clone the repo: `git clone git@github.com:mosegui/portable_jupyter_env.git`
- Move `autojupyter` bash script to `/usr/bin` so that it can be called from everywhere `cp autojupyter /usr/bin/autojupyter`
- Build the Docker (or Podman) image as `jupyter_env` `docker build -t jupyter_env .`
- Done


### Usage

- Navigate to the location where you want to open your Jupyter session.
- Open a terminal by either **Context Menu > Open in Terminal** or via the shortcut `Ctrl` + `Alt` + `T`
- Run `autojupyter`
- Use your Jupyter session normally. ***Remember to manually save*** everything you might need from your analysis (data, models, functions, plots) to the mounted volume. Once you close the browser window, any non-saved content in the container will be lost.

#### Available Python packages

- Algebra and data manipulation
	- NumPy
	- SciPy
	- pandas
- Statistics
	- statsmodels
- Machine Learning
	- Scikit-learn
- Deep Learning
	- TensorFlow
	- PyTorch
	- Keras
	- Theano
- Data Visualization
	- Matplotlib
	- seaborn
	- Bokeh
	- Plotly
- Data Mining and NLP
	- Beautiful Soup
	- SpaCy
	- Gensim
	- Natural Language Toolkit (NLTK)

### Observations/System requirements

- By default, the `autojupyter` shebang points to `#!/bin/bash`. While this is not mandatory and you could use other POSIX-compatible shells, it is necessary for the shell to be able to process arrays. Similarly, the shell must be able to process "here strings" to arrays via the redirection operator `<<<`. Possible alternatives to `bash` could be e. g. `zsh` or `ksh` or `yash`.
- If you want to use it with Podman instead of with Docker, either manually replace the `docker` command by `podman` in all events in the bash script, or you can just set `"alias docker='podman'"`.

- Depending on your Linux Desktop Environment, it is technically possible to launch `autojupyter` from your context menu (right click). While Ubuntu 20.04 support for `.desktop` files and custom launcher icons has worsened compared to previous versions, adding a `.desktop` file to `~/Templates` and accessing `autojupyter` via **Context Menu > New Document > Autojupyter** should be a solution. This, though, seems to work with KDE (Dolphin) but not so with GNOME.

### Troubleshooting

- If the app doesn't start:
	- You may need to make the BASH script runnable. You can do it by either right-clicking on the script and navigating to **Properties > Permissions** and checking the checkbox *Allow executing file as program* or by running `chmod +x autojupyter` on your terminal.
	- You might also need to set your file manager to automatically run executable files.
		- On Nautilus, navigate to **Settings > Preferences > Behavior** and in the section *Executable text files* select  *Run them*.
		- On Dolphin, navigate to **Preferences > General > Confirmations** and in the dropdown menu *When opening an executable file* select *Run script*

- If the connection with the Jupyter server is lost after short time (Docker container shuts down):
	- The most likely reason is that you have some Experimental Google Add-on turned on that is being catched as a PID by the BASH script and gets killed automatically, driving the container shutdown (finishing of ANY of the PIDs identified as new causes container to close).
		- You can investigate which one can it be by taking a look at the Chrome task manager (`Shift` + `Esc`), but one usual suspect is the *Chrome Media Router* extension. To disable it, open your browser and go to the [Flags](chrome://flags) settings, look for the feature **Load Media Router Component Extension**, and disable it.

- Docker container does not close automatically after closing the Chromium window.
	- You might be attempting to start the service in a folder for which you do not have permissions.

## Authors
* **Daniel Moseguí González** - [GitHub](https://github.com/mosegui) - [LinkedIn](https://www.linkedin.com/in/daniel-mosegu%C3%AD-gonz%C3%A1lez-5aa02849/)

## License

This project is licensed under the GNU GPL License - see the [LICENSE](LICENSE.txt) file for details.
