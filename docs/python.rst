Embedded Python
===============================

Supported libraries for the standard distribution (ETA_LIB)
-----

- numpy
- scipy
- lmfit
- matplotlib
- dash
- plotly
- bokeh

Users can also use the site-packages from his own python installation, 
which is usually located at `C:\Users\***\AppData\Local\Programs\Python\Python36\Lib\site-packages`

Minimum example
-----

.. code:: python

    import numpy as np
    result =eta.run(filename, 4)
    histogram = result["h1"] #get the table from result
    np.savetxt("h1.txt",histogram) #save the txt file for the histogram
    eta.send("processing succeed.")


Full example with plotly (dash)
-----

.. code:: python

    import numpy as np
    import dash
    import dash_core_components as dcc
    import dash_html_components as html
    import plotly.graph_objs as go
    result =eta.run(filename, 4)
    histogram= result["h1"] #get the table from result

    app = dash.Dash()
    app.layout = html.Div(children=[
        html.H1(children='Result from ETA'),
        html.P(children='+inf_bin={}'.format(inf)),
        dcc.Graph(
            id='example-graph',
            figure={
                'data': [
                    {'x': np.arange(histogram.size), 'y': histogram, 'type': 'bar', 'name': 'SF'},
                ],
                'layout': {
                    'title': expname
                }
            }
        )
    ])

    eta.display(app)


More Examples
-----

For more examples please refer to the recipes.

