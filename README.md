# Dash Callback Wrapper

Reference callback parameters in property chaining style and simplify callback function definition.

For example, to reference the `value` property of `dropdown-selection` component, you need to use `callback_manager.dropdown_selection.value` instead of `dash.callback_context.inputs['dropdown-selection.value']` or parameter defined in callback function.

In addition, you can omit specifying callback function parameters at all.

Consider the [minimal example](https://dash.plotly.com/minimal-app). With `dcw` it will look as follows:

```python
import pandas as pd
import plotly.express as px
from dash import Dash, Input, Output, dcc, html

from dcw import DCWDash, callback
from dcw import callback_manager as cm

df = pd.read_csv('https://raw.githubusercontent.com/plotly/datasets/master/gapminder_unfiltered.csv')

app = DCWDash(__name__)

app.layout = html.Div([
    html.H1(children='Title of Dash App', style={'textAlign':'center'}),
    dcc.Dropdown(df.country.unique(), 'Canada', id='dropdown-selection'),
    dcc.Graph(id='graph-content')
])

@callback(
    Output('graph-content', 'figure'),
    Input('dropdown-selection', 'value')
)
def update_graph():
    dff = df[df.country==cm.dropdown_selection.value]
    return px.line(dff, x='year', y='pop')

if __name__ == '__main__':
    app.run(debug=True)
```

Note that both `def update_graph()` and `def update_graph(value)` will work. The only limitation is that if you decide to specify parameters, you still need to use the exact number of parameters.
