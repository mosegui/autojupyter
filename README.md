# Autojupyter

This simple Docker Compose-based application provides a one-command solution for firing a standaradized Jupyter development environment that will also automatically stop all containers once the analyst is finished with his/her session. This way, analysts do not have not worry about manually managing Docker/Podman services or cleaning after themselves.

This construct easily allows for having Jupyter Notebooks automatically integrated in the CI/CD environment. Hereby, the Docker image providing the Jupyter service can have as base image an IDE Docker remote interpreter, and/or Docker images dedicated to testing, building or PR-merging, ensuring an homogeneous environment throughout the software/algorithm lifecycle.


## Getting Started

### Description

The app consists of two services packed in their respective containers. One of them contians the Jupyter server, while the other provides a packaged installation of the Firefox web browser. Once the user is done with his/her session and the browser is closed, all containers are stopped.

 Providing a pre-packaged browser service and leveragind Docker Compose removes the need for monitoring host browser PIDs for triggering container stopping, as well as avoids configuration problems with the host browser web driver. The current stack only takes advantage of the host's X11 socket located at `/tmp/.X11-unix` for browser display.

Docker Compose automatically mounts to the Jupyter container the volume from which the `autojupyter` was called. This way, the analyst can fire `autojupyter` e. g. from where the data is located and have the data automatically available in the Jupyter environment.

See the demo:

![demo](video/demo.gif)


### Requirements

**Docker** and **Docker Compose** are required for Autojupyter to run. As of now, upon Autojupyter installation, these two programs will not be installed automatically by Autojupyter if missing. The user must install them manually.


### Installation

An installation script is provided for setting Autojupyter automatically up:

- Clone the repo: `git clone git@github.com:mosegui/autojupyter.git`
- From the repo folder, open a terminal and run `./installer $$`
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

- Depending on your Linux Desktop Environment, it is technically possible to launch `autojupyter` from your context menu (right click). While Ubuntu 20.04 support for `.desktop` files and custom launcher icons has worsened compared to previous versions, adding a `.desktop` file to `~/Templates` and accessing `autojupyter` via **Context Menu > New Document > Autojupyter** should be a solution. This, though, seems to work with KDE (Dolphin) but not so with GNOME.

### Troubleshooting

- If the app or hte installer don't run:
	- You may need to make the BASH scripts runnable. You can do it by either right-clicking on the script and navigating to **Properties > Permissions** and checking the checkbox *Allow executing file as program* or by running `chmod +x autojupyter` (or for `installer`)  on your terminal.
	- You might also need to set your file manager to automatically run executable files.
		- On Nautilus, navigate to **Settings > Preferences > Behavior** and in the section *Executable text files* select  *Run them*.
		- On Dolphin, navigate to **Preferences > General > Confirmations** and in the dropdown menu *When opening an executable file* select *Run script*


## Authors
* **Daniel Moseguí González** - [GitHub](https://github.com/mosegui) - [LinkedIn](https://www.linkedin.com/in/daniel-mosegu%C3%AD-gonz%C3%A1lez-5aa02849/)

## License

This project is licensed under the GNU GPL License - see the [LICENSE](LICENSE.txt) file for details.
