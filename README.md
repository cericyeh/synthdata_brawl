# synthdata_brawl

This is used to provide concrete examples of linkages between Sysmon and Netflow events.  

## Installation

Unzip the `src_data.zip` file in-place, which will create a directory `csvs/` populated with the examples.

This demo uses Python 3+ with an IPython notebook, and a minimal requirements file is provided.  To install, 

```
pip install -r requirements.txt
```

To start the notebook,

```
jupyter notebook --host localhost --port 9123
```

And browse to `https://localhost:9123` to load and view the demonstration notebook.  On most installations the default browser will be opened after starting the server.

## Demo

Please view the file `demo.ipynb` for a demonstration of how to load the examples.