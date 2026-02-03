# Chapter 1 - Setting up the environment

## Prepare dependencies

## Code 1

```python
dash==3.2.0
plotly==5.24.0
dash-bootstrap-components==2.0.4
pandas==2.2.3
Flask-Caching==2.1.0

pytest==8.3.3
dash[testing]==3.2.0
selenium==4.2.0
```

---

## Code 2

```bash
pip install -r requirements.txt
```

---

## Code 3

```bash
pip check
```

---

## Code 4

```python
from __future__ import annotations
def test_imports():
    """
    Verifies that all major dependencies of the project
    are installed and importable.

    This test fails if any of the required libraries are not
    present or if an incompatibility prevents their import.
    """
    import dash
    import plotly
    import pandas
    import flask_caching
    import dash_bootstrap_components
```

---

## Code 5

```bash
pytest -q
```

---

## First Dash application

## Code 7

```python
from __future__ import annotations
from dash import Dash, html

app = Dash(__name__)
app.title = "Dash Demo"
app.layout = html.Div(
    children=[
        html.H1("Hello Dash 3.2.0!"),
        html.P("Your first local Dash application is up and running.")
    ]
)

if __name__ == "__main__":
    app.run(debug=True)
```

---

## Create your first unit test

## Code 10

```python
from __future__ import annotations
from dash import Dash, html

def test_app_starts() -> None:
    """
    Verify that the Dash application can be instantiated
    and that a layout can be defined.
    """
    
    # Create the Dash application
    app: Dash = Dash(__name__)

    # Define a minimal layout
    app.layout = html.Div("Test Dash 3.2.0")

    # Simple checks
    assert app is not None
    assert hasattr(app, "layout")
```

---

# Chapter 2 - Understanding Dash

## Anatomy of a Dash application

## Code 1

```python
from __future__ import annotations
from dash import Dash, html, Input, Output, dcc

# Create the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Define the layout (the page content)
app.layout = html.Div(
    [
        # Name input field
        dcc.Input(
            id="name",
            value="Dash",
            type="text"
        ),

        # Message display area
        html.Div(
            id="greeting",
        ),
    ]
)

# Defining the callback (interactivity)
@app.callback(
    Output("greeting", "children"),
    Input("name", "value"),
)
def update_greeting(name: str) -> str:
    """
    Updates the greeting message displayed on the screen.

    Args:
        name (str): text entered by the user.

    Returns:
        str: message to display.
    """
    return f"Hello {name} !"

if __name__ == "__main__":
    app.run(debug=True)
```

---

## Code 2

```python
from __future__ import annotations

# Import the necessary elements from Dash:
#     - Dash: main class that creates the web application
#     - html: HTML components (Div, etc.)
#     - dcc: "Dash Core Components" components (Input, Graph, etc.)
#     - Input / Output: objects describing callback dependencies
from dash import Dash, html, Input, Output, dcc

# Create the Dash application.
#     The module name (__name__) is used by Dash/Flask for
#     internal configuration and discovery of associated resources.
app = Dash(__name__)
# Set the browser tab title.
app.title = "Dash Demo"

# Defining the application layout.
app.layout = html.Div([
    # Input field:
    #     - id="name": unique identifier, used by the callback
    #     - value="Dash": initial value displayed in the field
    #     - type="text": classic HTML text field
    dcc.Input(
      id="name",
      value="Dash",
      type="text"
    ),

    # Initial area, which will be dynamically updated by the
      # callback based on user input:
    #     - id="greeting": unique identifier, used by the callback
    html.Div(id="greeting")
])

# Dash callback declaration.
#     The callback connects:
#         - one or more inputs,
#         - to one or more outputs.
#
#     Here:
#         - Input("name", "value"): the callback is triggered each time
#           the value of the input field identified as ‘name’ is modified.
#         - Output("greeting", "children"): the content of the <div>
#          identified as "greeting" is automatically updated.
@app.callback(
    Output("greeting", "children"),
    Input("name", "value"))

def update_greeting(name) -> str:
    """
    Updates the greeting message based on the user's input.
    This function is called automatically by Dash each time

    This function is called automatically by Dash each time
    the value of the input field changes. The `name` parameter
    corresponds to the current value of the dcc.Input component.
    
    Args:
        name (str): text entered by the user.

    Returns:
        str: message to display.
    """
    return f"Hello {name}!"

# Application entry point.
if __name__ == "__main__":
    # Launch the Dash development server. Debug mode
    # enables automatic reloading and notification of
    # detailed error messages during development.
    app.run(debug=True)
```

---

## Testing the structure of a layout

## Code 3

```python
from __future__ import annotations
from dash import Dash, html

def test_layout_structure() -> None:
    """
    Checks that the Dash application has a valid layout
    and that its basic structure is correct.
    """
    
    # Create the Dash application
    app: Dash = Dash(__name__)

    # Define a simple layout with two components
    app.layout = html.Div(
        [
            html.H1("Dash 3.2.0 structure test"),
            html.P("This test checks that the layout loads correctly."),
        ]
    )

    # Checks that the layout exists
    assert app.layout is not None

    # Verifies that the layout's children are defined as a list
    assert isinstance(app.layout.children, list)

    # Verifies that at least one H1 heading is present in the layout
    assert any(
        isinstance(child, html.H1)
        for child in app.layout.children
    )
```

---

# Chapter 3 - Layout and interface structure

## The layout: the visual framework of the application

## Code 1

```python
from __future__ import annotations
from dash import Dash, html

# Create the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Define the layout (the page content)
app.layout = html.Div(
    [
        # A title
        html.H1("Local Data Analysis"),

        # A paragraph
        html.P("First Dash 3.2.0 structure."),
    ]
)

if __name__ == "__main__":
    # Launching Dash development server
    app.run(debug=True)
```

---

## Declaring a layout

## Code 2

```python
from __future__ import annotations
from dash import Dash, html, dcc

# Create the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Define the layout (the page content)
app.layout = html.Div(
    children=[
        # A title
        html.H1("Local Dashboard"),

        # A paragraph
        html.P("Dash application running locally."),

        # A container to define an input form
        html.Div(
            children=[
                html.Div(html.Label("Name :")),
                html.Div(dcc.Input(type="text")),
                html.Button("Validate")
            ],
        ),

        # Empty container (reserved for future content)
        html.Div(),
    ],
)

if __name__ == "__main__":
    # Launch the Dash development server
    app.run(debug=True)
```

---

## Style

## Code 5

```css
body {
  background-color: lightblue;
  text-align: center;
}
h1 {
  color: blue;
}
p {
  color: green;
}
```

---

## Prioritize and document

## Code 6

```python
# Zone 1
header_area = html.Div([
    html.H1("Local Dash application"),
    html.P("Example of a hierarchical layout.")
])

# Zone 1
content_area = html.Div([
    html.Label("Selection :"),
    dcc.Dropdown(["A", "B", "C"], "A")
])

app.layout = html.Div([header_area, content_area])
```

---

## A complete example

### Style

## Code 7

```css
body{
  margin:0;
  background:#FFE0B2;
  color:#1E88E5;
  font-family:system-ui, -apple-system, Roboto, Ubuntu, Arial;
}
.page{
  padding:1rem;
}
```

---

### Le code

## Code 8

```python
from __future__ import annotations
from dash import Dash, dcc, html

def build_area_01() -> html.Div:
    """
    Builds an area containing a single child component.

    This function illustrates the case where `children` receives a
    single component: a paragraph.
    """
    return html.Div(
        children=html.P(
            "Children single-component (paragraph)",
            title="simple example",
        ),
        style={
            "border": "1px solid #ccc",
            "borderRadius": "8px",
            "textAlign": "center",
            "margin": "5px",
        },
    )

def build_area_02() -> html.Div:
    """
    Builds an area containing several child components.

    This function illustrates the case where `children` receives two
    components: an input field and a button.
    """
    return html.Div(
        children=[
            html.P("Children multi-component (input and button)"),

            # Line 1: an input field
            html.Div(
                dcc.Input(
                    type="text",
                    placeholder="Your name",
                    value="Dash",
                    maxLength=40,
                    style={"textAlign": "center"}
                ),
                style={"textAlign": "center", "margin": "8px"}
            ),

            # Line 2: a button
            html.Div(
                html.Button(
                    "Button (disabled, no callback)",
                    n_clicks=0,
                    title="Disabled (no callback)",
                ),
                style={"marginBottom": "10px"},
            ),
        ],
        style={
            "border": "1px solid #ccc",
            "borderRadius": "8px",
            "textAlign": "center",
            "paddingBottom": "10px"
        },
    )

def serve_layout() -> html.Div:
    """
    Builds the main layout of the application.

    This function is called by Dash to generate
    the page content.
    """
    return html.Div(
        className="page",
        children=[
            # A single component returned by the function
            build_area_01(),

            # Two components returned by the function
            build_area_02(),
        ],
    )

# Create the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Layout creation
app.layout = serve_layout

if __name__ == "__main__":
    # Launch the Dash development server
    app.run(debug=True)
```

---

## Testing the structure of a layout

## Code 9

```python
from __future__ import annotations
from dash import Dash, html, dcc

def test_layout_construction() -> None:
    """
    Checks that the Dash layout contains the essential elements
    and that their structure is correct.
    """
    
    # Creation of the Dash application
    app: Dash = Dash(__name__)

    # Create the layout with three components
    app.layout = html.Div(
        [
            html.H1("Test title"),
            dcc.Input(id="input_test"),
            html.Div(id="result_test"),
        ]
    )

    # Checks that the layout contains a list of components
    assert isinstance(app.layout.children, list)

    # Checks for the presence of an H1 title
    assert any(
        isinstance(child, html.H1)
        for child in app.layout.children
    )

    # Retrieves the IDs of components with an ‘id’ attribute
    ids = [
        child.id
        for child in app.layout.children
        if hasattr(child, "id")
    ]

    # Checks that the expected components are present
    assert "input_test" in ids
    assert "result_test" in ids
```

---

# Chapter 4 - Callbacks

## A complete example with interactive text

## Code 2

```python
from __future__ import annotations
from dash import Dash, html, Input, Output, dcc

# Create the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Create the layout
app.layout = html.Div(
    [
        # Main title
        html.H1("Dash 3.2.0 Callbacks Test"),

        # Instruction text
        html.P("Enter your name:"),

        # Name input field
        dcc.Input(
            id="name",
            type="text",
            value="",
            style={"textAlign": "center"},
        ),

        # Result display area
        html.Div(
            id="result",
            style={"marginTop": "20px"},
        ),
    ],
    style={
        "textAlign": "center",
    },
)

@app.callback(
    Output("result", "children"),
    Input("name", "value"),
)
def update_greeting(name: str) -> str:
    """
    Updates the message displayed based on the name entered.

    Args:
        name (str): text entered by the user.

    Returns:
        str: greeting message displayed.
    """
    return f"Hello {name}!"

if __name__ == "__main__":
    # Launch Dash development server
    app.run(debug=True)
```

---

## Multiple inputs and one output

## Code 3

```python
from __future__ import annotations
from dash import Dash, html, dcc, Input, Output

# Create the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Layout creation
app.layout = html.Div(
    [
        # First numeric field
        dcc.Input(
            id="first_value",
            value="2",
            type="number",
            style={"textAlign": "center"},
        ),

        # Second numeric field
        dcc.Input(
            id="second_value",
            value="3",
            type="number",
            style={"textAlign": "center"},
        ),

        # Result display area
        html.Div(id="result", style={"marginTop": "10px"})
    ],
    style={
        "textAlign": "center",
        "marginTop": "20px",
    },
)

@app.callback(
    Output("result", "children"),
    Input("first_value", "value"),
    Input("second_value", "value"),
)
def add(a: float | int | None, b: float | int | None) -> str:
    """
    Calculates the sum of two values entered by the user.

    Args:
        a (float | int | None): first numerical value entered.
        b (float | int | None): second numerical value entered.

    Returns:
        str: calculation result or error message.
    """
    try:
        x = float(a)
        y = float(b)
        return f"{x} + {y} = {x + y}"
    except (TypeError, ValueError):
        return "Invalid entry."

if __name__ == "__main__":
    # Launch the Dash development server
    app.run(debug=True)
```

---

## Multiple outputs for synchronizing components

## Code 4

```python
@app.callback(
    Output("first_text", "children"),
    Output("second_text", "children"),
    Input("name", "value"),
)
def double_update(name: str) -> tuple[str, str]:
    """
    Updates two text fields from a single entry.

    Args:
        name (str): text entered by the user.

    Returns:
        tuple[str, str]:
        - the message displaying the name entered
      - the message displaying the length of the name
    """
    return (
        f"Name entered: {name}",
        f"Length: {len(name)} characters",
    )
```

---

## Input, Output, State: understanding the difference

## Code 5

```python
from __future__ import annotations
from dash import Dash, html, Input, Output, State, dcc

# Create the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Create the layout
app.layout = html.Div(
    [
        # Instruction text
        html.P("Enter your first name:"),

        html.Div(
            dcc.Input(
                id="name",
                type="text",
                value="",
                debounce=True,
                style={"textAlign": "center", "marginBottom": "10px"})
        ),

        # Validation button
        html.Div(
            html.Button(
                "OK",
                id="ok_button",
                n_clicks=0,
                style={"marginBottom": "10px"},)
        ),

        # Message display area
        html.Div(
            id="result"
        ),
    ],
    style={
        "textAlign": "center",
    },
)

@app.callback(
    Output("result", "children"),
    Input("ok_button", "n_clicks"),
    State("name", "value"),
)
def display_message(n_clicks: int, name: str) -> str:
    """
    Displays a message when the user clicks the button.

    Args:
        n_clicks (int): number of clicks on the “OK” button.
        name (str): name entered by the user.

    Returns:
        str: message displayed on the page.
    """
    if not n_clicks:
        return ""

    # Add a space only if a name is entered
    suffix = f" {name}" if name else ""
    
    return f"Hello{suffix}, you clicked {n_clicks} times."

if __name__ == "__main__":
    # Launch the Dash development server
    app.run(debug=True)
```

---

## Handling errors and stability

## Complete exercise: interactive calculation

## Code 7

```python
from __future__ import annotations
from dash import Dash, html, dcc, Input, Output, State
from dash.exceptions import PreventUpdate

# Create the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Create the layout
app.layout = html.Div(
    [
        # Page title
        html.H1("Add two numbers"),

        # First numeric field
        dcc.Input(
            id="first_value",
            type="number",
            value=0,
            style={"textAlign": "center"},
        ),

        # Second numeric field
        dcc.Input(
            id="second_value",
            type="number",
            value=0,
            style={"textAlign": "center"},
        ),

        # Calculation trigger button
        html.Div(html.Button(
            "Add",
            id="button_add",
        style={"marginTop": "10px"})
        ),

        # Result display area
        html.Div(
            id="result",
            style={"marginTop": "10px"},
        ),
    ],
    style={
        "textAlign": "center",
    },
)

@app.callback(
    Output("result", "children"),
    Input("button_add", "n_clicks"),
    State("first_value", "value"),
    State("second_value", "value"),
)
def calculate(
    n_clicks: int,
    first_value: float | int | None,
    second_value: float | int | None,
) -> str:
    """
    Calculates the sum of two numbers entered by the user
    after clicking the button.

    Args:
        n_clicks (int): number of clicks on the “Add” button.
        first_value (float | int | None): first numerical value entered.
        second_value (float | int | None): second numerical value entered.

    Returns:
        str: result of the calculation or error message.
    """
    # Prevents the callback from executing before the first click
    if not n_clicks:
        raise PreventUpdate

    try:
        result = float(first_value) + float(second_value)
        return f"Result: {result}"
    except (TypeError, ValueError):
        return "Error: please enter valid numbers."

if __name__ == "__main__":
    # Launch the Dash development server
    app.run(debug=True)
```

---

## Layout loading and validation_layout

## Code 8

```python
from __future__ import annotations
from dash import Dash, Input, Output, dcc, html
from dash.exceptions import PreventUpdate

# Dropdown menu options
OPTIONS = [
    {"label": "Option A", "value": "A"},
    {"label": "Option B", "value": "B"},
]

# Creating the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# 1) Validation layout: this minimal layout is used
#    only to declare the identifiers (id) used
#    in callbacks, for control purposes.
app.validation_layout = html.Div(
    [
        dcc.Dropdown(id="options_list"),
        html.Div(id="main_container"),
    ]
)

# 2) Creation of the actual layout via a function.
def serve_layout() -> html.Div:
    """
    Builds the application layout.

    This function is called by Dash when loading
    or reloading the page.
    """
    return html.Div(
        [
            # Dropdown menu with options
            dcc.Dropdown(
                id="options_list",
                options=OPTIONS,
                value="A",
                clearable=False,
            ),

            # Main area updated by the callback
            html.Div(
                id="main_container",
                style={"marginTop": "12px"},
            ),

            # ... more complex components possible here ...
        ],
        style={
            "maxWidth": "600px",
            "margin": "40px auto",
            "textAlign": "center",
        },
    )

# 3) Assign the actual layout.
app.layout = serve_layout

# 4) Callback.
@app.callback(
    Output("main_container", "children"),
    Input("options_list", "value"),
)
def update(option_selected: str) -> str:
    """
    Updates the main content according to the selected option.

    Args:
        option_selected (str): value selected from the drop-down list.

    Returns:
        str: text displayed in the main area.
    """
    # Security: no update if no option selected
    if option_selected is None:
        raise PreventUpdate

    return f"Choice: {option_selected}"

if __name__ == "__main__":
    # Launch Dash development server
    app.run(debug=True)
```

---

## End-to-end verification

## Code 9

```python
from __future__ import annotations
from dash import Dash, html, Input, Output, dcc

# Create the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Create the layout
app.layout = html.Div(
    [
        # Main title
        html.H1("Dash 3.2.0 Callbacks Test"),

        # Instruction text
        html.P("Enter your name:"),

        # Name input field
        dcc.Input(
            id="name",
            type="text",
            value="",
            style={"textAlign": "center"},
        ),

        # Result display area
        html.Div(
            id="result",
            style={"marginTop": "20px"},
        ),
    ],
    style={
        "textAlign": "center",
    },
)

@app.callback(
    Output("result", "children"),
    Input("name", "value"),
)
def update_greeting(name: str) -> str:
    """
    Updates the message displayed based on the name entered.

    Args:
        name (str): text entered by the user.

    Returns:
        str: greeting message displayed.
    """
    return f"Hello {name}!"

if __name__ == "__main__":
    # Launch Dash development server
    app.run(debug=True)
```

---

## Code 10

```python
from __future__ import annotations
from dash.testing.application_runners import import_app

def test_callback_structure(dash_duo) -> None:
    """
    Verifies that a Dash callback responds correctly
    to user input.

    The test:
    - launches the Dash application
    - enters text in the ‘name’ field
    - verifies that the message displayed is correct
    """
    
    # Import the Dash application to be tested
    app = import_app("app_callback")

    # Start the Dash test server
    dash_duo.start_server(app)

    # Search for the input field by its ID
    champ = dash_duo.find_element("#name")

    # Simulating user input
    champ.send_keys("Python")

    # Checks that the expected text appears on the screenn
    dash_duo.wait_for_text_to_equal(
        "#result",
        "Hello Python!",
        timeout=4,
    )
```

---

# Chapter 5 - Interactivity and visualization

## A first Plotly graph in Dash

## Code 2

```python
from __future__ import annotations
import pandas as pd
import plotly.express as px
from dash import Dash, html, dcc

# Create local data
DF = pd.DataFrame(
    {
        "City": ["New York", "Los Angeles", "Chicago",
                  "Houston", "Phoenix"],
        "Population": [8.3, 3.9, 2.7, 2.3, 1.6],
        "Region": ["New York", "California", "Illinois",
                   "Texas", "Arizona"],
    }
)

# Creation of the Plotly figure
figure = px.bar(
    DF,
    x="City",
    y="Population",
    color="Region",
)

# Centering the Plotly figure title
figure.update_layout(
    title={
        "text": "Population (in millions)",
        "x": 0.5,
        "xanchor": "center",
    }
)

# Creating the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Creating the layout
app.layout = html.Div(
    [
        # Title page
        html.H1(
            "Plotly graph integrated into Dash",
            style={"textAlign": "center"},
        ),

        # Integration of the Plotly graph
        dcc.Graph(
            id="graphic",
            figure=figure,
        ),
    ]
)

if __name__ == "__main__":
    # Launching the development server
    app.run(debug=True)
```

---

## Enhance the figure with a responsive chart

## Code 3

```python
from __future__ import annotations
import pandas as pd
import plotly.express as px
from dash import Dash, html, dcc, Input, Output

# Create local data
DF = pd.DataFrame(
    {
        "City": ["New York", "Los Angeles", "Chicago",
                  "Houston", "Phoenix"],
        "Population": [8.3, 3.9, 2.7, 2.3, 1.6],
        "Region": ["New York", "California", "Illinois",
                   "Texas", "Arizona"],
    }
)

# Creating the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Creating the layout
app.layout = html.Div(
    [
        # Title page
        html.H1(
            "Population by region",
            style={"textAlign": "center"},
        ),

        # Dropdown list for selecting a region
        dcc.Dropdown(
            id="regions_list",
            options=[
                {"label": r, "value": r}
                for r in DF["Region"].unique()
            ],
            value="Arizona",
            clearable=False,
            style={"textAlign": "center"},
        ),

        # Graph display area
        dcc.Graph(id="graphic"),
    ]
)

# Callback
@app.callback(
    Output("graphic", "figure"),
    Input("regions_list", "value"),
)
def update_graphic(region_selected: str):
    """
    Updates the graph according to the selected region.

    Args:
        region_selected (str): region selected from the drop-down list.

    Returns:
        plotly.graph_objs.Figure: Plotly graph to display.
    """
    # Filtering data according to region
    dff = DF[DF["Region"] == region_selected]

    # Creating the Plotly graph
    figure = px.bar(
        dff,
        x="City",
        y="Population",
        color="City",
    )

    return figure

if __name__ == "__main__":
    # Launching the development server
    app.run(debug=True)
```

---

## Explore native interactions

## Code 4

```python
from __future__ import annotations
import pandas as pd
import plotly.express as px
from dash import Dash, html, dcc, Input, Output

# Create local data
DF = pd.DataFrame(
    {
        "City": ["New York", "Los Angeles", "Chicago",
                  "Houston", "Phoenix"],
        "Population": [8.3, 3.9, 2.7, 2.3, 1.6],
        "Region": ["New York", "California", "Illinois",
                   "Texas", "Arizona"],
    }
)

# Message displayed below the graph
MSG_DEFAULT = "Click on a bar to display details."

# Creation of the Plotly figure
figure = px.bar(
    DF,
    x="City",
    y="Population",
    color="Region",
)

# Centering the Plotly figure title
figure.update_layout(
    title={
        "text": "Population (in millions)",
        "x": 0.5,
        "xanchor": "center",
    }
)

# Create the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Create the layout
app.layout = html.Div(
    [
        # Page title
        html.H1(
            "Plotly graph integrated into Dash",
            style={"textAlign": "center"}
        ),

        # Integration of the Plotly graph
        dcc.Graph(
            id="graphic",
            figure=figure,
        ),

        # Information message
        html.Div(
            id="message",
            children=MSG_DEFAULT,
            style={
                "marginTop": "10px",
                "textAlign": "center",
            },
        ),

        # Details display area
        html.Div(
            id="details_area",
            style={
                "marginTop": "10px",
                "textAlign": "center",
            }
        ),
    ]
)

# Callback
@app.callback(
    Output("details_area", "children"),
    Input("graphic", "clickData"),
)
def update_details(click_data: dict | None) -> str:
    """
    Displays information about the clicked bar.

    Args:
        click_data (dict | None): data returned by Plotly
            when the graph is clicked.

    Returns:
        str: descriptive text for the selected city.
    """
    # No click → nothing to display
    if not click_data:
        return ""

    # Retrieve the clicked point
    point = click_data["points"][0]
    town = point.get("x", "")
    population = point.get("y", "")

    return f"{town}: {population} million inhabitants"

if __name__ == "__main__":
    # Launch Dash development server
    app.run(debug=True)
```

---

## Complete example for exploring a real dataset

## Code 5

```python
from __future__ import annotations
import plotly.express as px
from dash import Dash, html, dcc, Input, Output

# Load the Gapminder Plotly dataset
DF = px.data.gapminder()

# Create the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Create the layout
app.layout = html.Div(
    [
        # Main title
        html.H1("Gapminder Local Data Explorer",
                style = {"textAlign": "center"}),

        # Slider label
        html.Label("Choose a year:"),

        # Year selection slider
        dcc.Slider(
            id="year_slider",
            value=int(DF["year"].min()),     # initial value
            min=int(DF["year"].min()),       # minimum year
            max=int(DF["year"].max()),       # maximum year
            step=None,                       # only defined values
            marks={
                str(year): str(year)
                for year in DF["year"].unique()
            },                               # graduations by year
        ),

        # Graph display area
        dcc.Graph(id="graphic"),
    ],
    style={"maxWidth": "900px", "margin": "0 auto"},
)

# Callback
@app.callback(
    Output("graphic", "figure"),
    Input("year_slider", "value"),
)
def update_graphic(year: int):
    """
    Updates the graph according to the selected year.

    Args:
        year (int): year selected via the slider.

    Returns:
        plotly.graph_objs.Figure: Plotly graph to display.
    """
    # Filter data for the selected year
    dff = DF[DF["year"] == year]

    # Create scatter plot (scatter)
    figure = px.scatter(
        dff,
        x="gdpPercap",              # GDP per capita
        y="lifeExp",                # life expectancy
        size="pop",                 # bubble size
        color="continent",          # color by continent
        hover_name="country",       # info on hover
        log_x=True,                 # logarithmic X axis
        size_max=55,
        title=f"Life expectancy and GDP per capita ({year})"
    )
    figure.update_layout(
        plot_bgcolor="#FFE0B2"      # Background color of the graph
    )

    return figure

if __name__ == "__main__":
    # Launch Dash development server
    app.run(debug=True)
```

---

## Code 6

```python
# The graph adapts to the year selected on the slider
@app.callback(
  Output("graphic", "figure"),
  Input("year_slider", "value")
)
def update_graphic(year):
  dff = DF[DF["year"] == year]
  figure = px.scatter(dff,
                      x="gdpPercap",
                      y="lifeExp",
                      log_x=True,
                      size_max=55,
                      size="pop", color="continent",
                   		hover_name="country",
                   		hover_data={
                        "pop": False, 				 # do not show
            						"continent": False,    # do not show
            						"gdpPercap": ":,.0f",  # format (separators, 0 decimal places)
            						"lifeExp": ":.1f",     # 1 decimal place
                      },
                   		title=f"Life expectancy and GDP per capita ({year})")
  return figure
```

---

## Verification and testing in PyCharm

## Code 7

```python
from __future__ import annotations
from dash.testing.application_runners import import_app

def test_app_graphic_2(dash_duo) -> None:
    """
    Verifies that the application starts and that the graphic is present
    after changing the region via the dropdown menu.

    The test:
    - loads the Dash application
    - starts the test server
    - selects a region from the dropdown menu
    - checks that the title and graph are present
    """
    # Import the Dash application to be tested (module name without .py)
    app = import_app("app_graphic_2")

    # Start the Dash test server
    dash_duo.start_server(app)

    # Select a value from the Dash dropdown menu
    dash_duo.select_dcc_dropdown("#regions_list", "Illinois")

    # Check that the page has loaded correctly (expected title)
    dash_duo.wait_for_text_to_equal("h1", "Population by region")

    # Check that the Graph component is present on the page
    figure = dash_duo.find_element("#graphic")
    assert figure is not None
```

---

# Chapter 6 - Local Data Management

## Read a local file

## Code 1

```python
from __future__ import annotations
from dash import Dash, html, dcc
import plotly.express as px
import pandas as pd

app = Dash(__name__)
df = pd.read_csv("data/sales.csv")

app.layout = html.Div([
    html.H1(
        "Revenue by city", 
        style={"textAlign": "center"}
    ),
    dcc.Graph(
        figure=px.bar(df, x="City", y="Revenue", title="Sales 2023")
    ),
])

if __name__ == "__main__":
    app.run(debug=True)
```

---

## Selecting a file from the interface

## Code 2

```python
from __future__ import annotations
import base64
from dash import Dash, Input, Output, State, dcc, html
from dash.exceptions import PreventUpdate

# ======================
# Inline style constants
# ======================
UPLOAD_CHILD_STYLE = {
    "alignItems": "center",
    "textAlign": "center",
}

UPLOAD_BOX_STYLE = {
    "borderWidth": "2px",
    "borderStyle": "dashed",
    "borderRadius": "12px",
    "cursor": "pointer",
    "margin": "10px 20px 0 10px",
    "padding": "24px",
}

RESULT_STYLE = {
    "textAlign": "center",
}

CARD_STYLE = {
    "border": "1px solid #1565C0",
    "borderRadius": "8px",
    "margin": "10px 20px 0 10px",
    "padding": "12px",
}

TITLE_STYLE = {
    "margin": "0 0 6px 0",
}

PREVIEW_STYLE = {
    "maxWidth": "200px",
    "marginTop": "10px",
}

# ============================================================
# Utility function: create a display card for an uploaded file
# ============================================================
def build_file_card(
    name: str,
    mime_type: str,
    size_kb: float,
    content: str,
) -> html.Div:
    """
    Builds a display card for an uploaded file.

    Args:
        name (str): File name.
        mime_type (str): MIME type of the file (image/png, text/csv, etc.).
    		size_kb (float): File size in kilobytes.
        content (str): Base64-encoded content (data:<mime>;base64,...).

    Returns:
        html.Div: Dash card containing the file information.
    """
    # Preview only for images
    preview = (
        html.Img(src=content, style=PREVIEW_STYLE)
        if mime_type.startswith("image/")
        else None
    )

    return html.Div(
        [
            html.H4(name, style=TITLE_STYLE),
            html.Div(f"Type : {mime_type}"),
            html.Div(f"Size : {size_kb:.1f} KB"),
            preview,
        ],
        style=CARD_STYLE,
    )

# ================
# Dash application
# ================
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Create the layout
app.layout = html.Div(
    [
        # Upload area
        dcc.Upload(
            id="upload",
            accept="image/*,.csv",
            multiple=True,
            children=html.Div(
                [
                    html.Div("Drag and drop your files here"),
                    html.Div("or"),
                    html.Button("Choose files", type="button"),
                ],
                style=UPLOAD_CHILD_STYLE,
            ),
            style=UPLOAD_BOX_STYLE,
        ),

        # Results display area
        html.Div(
            id="upload_result",
            style=RESULT_STYLE,
        ),
    ]
)

# Callback
@app.callback(
    Output("upload_result", "children"),
    Input("upload", "contents"),
    State("upload", "filename"),
    State("upload", "last_modified"),
)
def display_upload(
    contents: list[str] | None,
    filenames: list[str] | None,
    last_modified: list[int] | None,
):
    """
    Displays information about uploaded files.

    Args:
        contents (list[str] | None): list of base64-encoded contents.
        filenames (list[str] | None): list of file names.
        last_modified (list[int] | None): modification dates (not used here).

    Returns:
        list[html.Div]: list of file display cards.
    """
    if not contents:
        raise PreventUpdate

    items: list[html.Div] = []

    for content, name in zip(contents, filenames):
        # Split the header and base64 content
        header, encoded = content.split(",", 1)

        # Extract MIME type
        mime_type = header.split(";")[0].replace("data:", "")

        # Calculate file size
        size_kb = len(base64.b64decode(encoded)) / 1024

        # Build the display card
        items.append(
            build_file_card(
                name,
                mime_type,
                size_kb,
                content,
            )
        )

    return items

if __name__ == "__main__":
    app.run(debug=True)
```

---

## Display data in table form

## Code 4

```python
from __future__ import annotations
import base64
import io
import pandas as pd
from dash import Dash, Input, Output, State, dash_table, dcc, html

# Creating the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Create the layout
app.layout = html.Div(
    [
        # Main title
        html.H1(
            "Import local CSV file",
            style={"textAlign": "center"},
        ),

        # Upload area (drag and drop or select)
        dcc.Upload(
            id="upload_file",
            children=html.Div(
                ["Drag and drop a file or click here to select one"]
            ),
            accept=".csv,text/csv",
            multiple=False,
            style={"width": "50%",
                   "margin": "auto",
                   "padding": "20px",
                   "border": "2px dashed #1976D2",
                   "textAlign": "center",
            },
        ),

        # File information display area
        html.Div(
            id="file_info",
            style={"textAlign": "center",
                   "padding": "10px 0"},
        ),

        # Table display area
        html.Div(id="table"),
    ]
)

@app.callback(
    Output("file_info", "children"),
    Output("table", "children"),
    Input("upload_file", "contents"),
    State("upload_file", "filename"),
)
def load_and_display(
    contents: str | None,
    file_name: str | None,
):
    """
    Loads a CSV file sent via dcc.Upload, reads it with Pandas,
    then displays:
    - a technical summary (name, number of rows, columns),
    - a Dash DataTable with sorting/filtering.

    Args:
        contents (str | None): contents of the file encoded in base64
            (format: data:...;base64,...).
        file_name (str | None): name of the selected file.

    Returns:
        tuple[object, object]:
        - contents of the “file_info” area (text or html.Div)
        - contents of the “table” area (DataTable or None)
    """
    # No file selected → message and no table
    if not contents:
        return "No files loaded.", None

    try:
        # contents = "data:<mime>;base64,<payload>"
        _, contenu_base64 = contents.split(",", 1)
        raw = base64.b64decode(contenu_base64)

        # Reading: UTF-8/UTF-8-SIG and replacement of invalid characters
        text = raw.decode("utf-8-sig", errors="replace")

        # CSV reading: auto-detection of separators
        df = pd.read_csv(io.StringIO(text), sep=None, engine="python")

        # Technical summary of the file
        info = html.Div(
            [
                html.P(f"File : {file_name or '(no name)'}"),
                html.P(f"Rows : {len(df)}"),
                html.P(f"Columns : {', '.join(map(str, df.columns))}"),
            ]
        )

        # Interactive table (sort + filter)
        table = html.Div(
            dash_table.DataTable(
                data=df.to_dict("records"),
                columns=[{"name": str(c), "id": str(c)} for c in df.columns],
                sort_action="native",
                filter_action="native",

                style_table={"overflowX": "auto"},

                style_header={"backgroundColor": "#FFCC80",
                              "fontWeight": "bold"},

                style_cell={"textAlign": "center",
                            "color": "black",
                            "backgroundColor": "#FFE0B2"},
            ),
            style={"margin": "20px auto",
                   "maxWidth": "95%"
                   }
        )

        return info, table

    except Exception as exc:
        # In case of error: explicit message + technical detail
        error_message = html.Div(
            [
                html.P(f"File : {file_name or '(no name)'}"),
                html.P("Error reading CSV."),
                html.Pre(str(exc)),
            ],
            style={"color": "#d32f2f"},
        )
        return error_message, None

if __name__ == "__main__":
    # Launch Dash development server
    app.run(debug=True)
```

---

## Modify and save the table

## Code 5

```python
from __future__ import annotations
import pandas as pd
from dash import Dash, Input, Output, State, dash_table, html

# File paths (read/write)
FILE_TO_READ = "data/sales.csv"
FILE_TO_SAVE = "data/modified_sales.csv"

# Read CSV data into a Pandas DataFrame
df: pd.DataFrame = pd.read_csv(FILE_TO_READ)

# Create the Dash application
app: Dash = Dash(__name__)
app.title = "Das Demo"

# Creating the layout
app.layout = html.Div(
    [
        # Main title
        html.H1("Edit and save table data"),

        # Interactive and editable table
        html.Div(
            dash_table.DataTable(
                id="sales_table",
                data=df.to_dict("records"),
                columns=[
                    # Each column is editable
                    {"name": c, "id": c, "editable": True}
                    for c in df.columns
                ],
                sort_action="native",
                filter_action="native",
                editable=True,
                style_table={"overflowX": "auto"},
                style_header={
                    "backgroundColor": "#FFCC80",
                    "fontWeight": "bold",
                },
                style_cell={
                    "textAlign": "center",
                    "color": "black",
                    "backgroundColor": "#FFE0B2",
                },
            ),
            style={
                "margin": "20px auto",
                "maxWidth": "95%"}
        ),

        # Save button
        html.Button(
            "Save",
            id="btn_save",
            style={"margin": "20px 20px"},
        ),

        # Message area (save result)
        html.Div(id="message"),
    ],
    style={"textAlign": "center"},
)

# Callback
@app.callback(
    Output("message", "children"),
    Input("btn_save", "n_clicks"),
    State("sales_table", "data"),
)
def save_sales_table(
    n_clicks: int,
    sales: list[dict] | None,
) -> str:
    """
    Saves all current DataTable data to a CSV file.

    Args:
        n_clicks (int): number of clicks on the “Save” button.
    	sales (list[dict] | None): table data (list of rows in
            the form of dictionaries).

    Returns:
        str: confirmation message (or empty string if no clicks).
    """
    # No clicks → do nothing
    if not n_clicks:
        return ""

    # Security: if the table is empty or unavailable
    if not sales:
        return "No data to save."

    # Conversion to DataFrame then CSV writing
    df_modified = pd.DataFrame(sales)
    df_modified.to_csv(FILE_TO_SAVE, index=False)

    return f"{len(df_modified)} rows saved in '{FILE_TO_SAVE}'."

if __name__ == "__main__":
    # Launch Dash development server
    app.run(debug=True)
```

---

## Testing file writing and content

## Code 6

```python
from __future__ import annotations
import pandas as pd
from pathlib import Path

def test_read_write_csv(tmp_path: Path) -> None:
    """
    Checks that writing and then reading a CSV file works.

    The test:
    - creates a temporary CSV file,
    - writes a Pandas DataFrame to it,
    - reads the file back,
    - compares the data obtained.
    """
    # Create a path to a temporary CSV file
    fichier: Path = tmp_path / "test.csv"

    # Initial DataFrame
    df_init: pd.DataFrame = pd.DataFrame(
        {
            "A": [1, 2, 3],
            "B": [4, 5, 6],
        }
    )

    # Write the DataFrame to the CSV file
    df_init.to_csv(fichier, index=False)

    # Reading the CSV file into a new DataFrame
    df_charge: pd.DataFrame = pd.read_csv(fichier)

    # Verifies that the data read is identical 
    assert df_charge.equals(df_init)
```

---

# Chapter 7 - Persistence and Local Storage

## The dcc.Store component

## Code 1

```python
from __future__ import annotations
from dash import Dash, Input, Output, State, dcc, html

# Create the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Creating the layout
app.layout = html.Div(
    [
        # Main title
        html.H2("Demo - Storage via dcc.Store"),

        # Browser-side storage
        dcc.Store(
            id="memory",
            storage_type="local",
        ),

        # Input field
        html.Div(
            dcc.Input(
                id="field",
                type="text",
                value="",
                style={
                    "margin": "5px",
                    "textAlign": "center",
                },
            )
        ),

        # Save button
        html.Div(
            html.Button(
                "Save",
                id="btn_save",
                style={"margin": "5px"},
            )
        ),

        # Message display area
        html.Div(
            id="message",
            style={"marginTop": "5px"},
        ),
    ],
    style={
        "textAlign": "center",
        "marginTop": "10px",
    },
)

# Callback
@app.callback(
    Output("memory", "data"),
    Input("btn_save", "n_clicks"),
    State("field", "value"),
    prevent_initial_call=True,
)
def save_value(
        _n_clicks: int,
        field: str | None,
) -> dict:
    """
    Saves the value of the input field to local storage.

    Args:
    	_n_clicks (int): number of clicks on the “Save” button.
    	field (str | None): value entered by the user.

    Returns:
        dict: dictionary to be stored in dcc.Store.
    """
    return {"text": field}

# Callback
@app.callback(
    Output("message", "children"),
    Input("memory", "data"),
)
def display_message(data: dict | None) -> str:
    """
    Displays the value restored from local storage.

    Args:
        data (dict | None): Data read from dcc.Store.

    Returns:
        str: Message displayed.
    """
    if data is None:
        return "No data saved."

    return f"Restored value: {data['text']}"
  
if __name__ == "__main__":
    # Launch Dash development server
    app.run(debug=True)
```

---

## Code 3

```python
from __future__ import annotations
from dash import Dash, html, dcc, Input, Output

# Create the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Creating the layout
app.layout = html.Div(
    [
        # Main title
        html.H2("Demo - Automatic memorization"),

        # Text input field:
        # - persistence=True: Dash automatically memorizes the entered value
        # - persistence_type="local": the value is stored in the browser
        dcc.Input(
            id="first_name",
            type="text",
            placeholder="Your first name",
            persistence=True,
            persistence_type="local",
            style={"textAlign": "center"},
        ),

        # Message display area below the input field
        html.Div(
            id="message",
            style={"marginTop": "20px"},
        ),
    ],
    # Global container style
    style={"textAlign": "center"},
)

# Callback
@app.callback(
    Output("message", "children"),
    Input("first_name", "value"),
)
def update_message(first_name: str | None) -> str:
    """
    Updates the greeting message based on the first name entered.

    This callback is automatically triggered each time
    the value of the input field is changed.

    Args:
        first_name (str | None): value entered in the input field.

    Returns:
        str: message displayed in the text area
    """
    # No text entered
    if not first_name:
        return "No first name entered"

    # Text present: display personalized message
    return f"Hello {first_name} !"

# Launch application in development mode
if __name__ == "__main__":
    app.run(debug=True)
```

---

## Use cases

## Code 4

```python
from __future__ import annotations
from dash import Dash, Input, Output, dcc, html

# =======================
# Inline style (constant)
# =======================
ZONE_BASE_STYLE = {
    "padding": "5px",
    "margin": "10px 50px",
}

# ================
# Dash application
# ================
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Creating the layout
app.layout = html.Div(
    [
        # Local storage of user preferences
        dcc.Store(
            id="prefs",
            storage_type="local",
        ),

        # Title
        html.H2("Choose a background color"),

        # Dropdown list of colors
        html.Div(
            dcc.Dropdown(
                id="colors_list",
                options=[
                    {"label": "White", "value": "white"},
                    {"label": "Light blue", "value": "#e3f2fd"},
                    {"label": "Gray", "value": "#eeeeee"},
                ],
                value="white",
                clearable=False,
            ),
            style={"margin": "10px 50px"},
        ),

        # Area whose background color changes
        html.Div(
            id="message",
            children="Hello !",
            style=ZONE_BASE_STYLE,
        ),
    ],
    style={"textAlign": "center"},
)

# ==============================
# Callback 1: saving preferences
# ==============================
@app.callback(
    Output("prefs", "data"),
    Input("colors_list", "value"),
    prevent_initial_call=True,
)
def save_color(color: str) -> dict:
    """
    Saves the selected color to local storage.

    Args:
        color (str): color chosen from the drop-down list.

    Returns:
        dict: dictionary containing the background color.
    """
    return {"background": color}

# ==============================
# Callback 2: applying the color
# ==============================
@app.callback(
    Output("message", "style"),
    Input("prefs", "data"),
)
def apply_color(pref: dict | None) -> dict:
    """
    Applies the saved background color to the display area.

    Args:
        pref (dict | None): preference retrieved from dcc.Store.

    Returns:
        dict: style applied to the message area.
    """
    background = (pref or {}).get("background")

    # No preference → default style
    if not background:
        return ZONE_BASE_STYLE

    # Merge the base style with the chosen color
    return {
        **ZONE_BASE_STYLE,
        "backgroundColor": background,
    }

if __name__ == "__main__":
    # Launch Dash development server
    app.run(debug=True)
```

---

## Functional limitations and workarounds

## Code 5

```python
from __future__ import annotations
import json
import os
import tempfile
from pathlib import Path
from dash import Dash, Input, Output, State, dcc, html
from dash.exceptions import PreventUpdate

# ====================
# Constant: local file
# ====================
NOTES_FILE: Path = Path("data/notes.json")

# =================
# Utility functions
# =================
def load_notes_json() -> str:
    """
    Loads the contents of the notepad from a local JSON file.

    The expected file contains a JSON object with a “content” key.
    If the file does not exist or if the JSON is invalid, an empty string is returned.

    Returns:
        str: text loaded from the file, or empty string if unavailable.
    """
    if not NOTES_FILE.exists():
        return ""

    try:
        content = NOTES_FILE.read_text(encoding="utf-8")
        json_data = json.loads(content)
        return str(json_data.get("content", ""))
    except (OSError, json.JSONDecodeError, TypeError, ValueError):
        # File inaccessible or invalid JSON → empty content
        return ""

def save_to_file(path: Path, text: str) -> None:
    """
    Writes a file atomically (robustly).

    Principle:
    - write to a temporary file,
    - force writing to disk (flush + fsync),
    - replace the final file in one go (os.replace).

    Args:
    	path (Path): path of the final file to write.
    	text (str): content to write to the file.
    """
    path.parent.mkdir(parents=True, exist_ok=True)

    fd, tmp_path = tempfile.mkstemp(prefix=path.name, dir=str(path.parent))
    os.close(fd)  # we will reopen via the path, which is simpler and more portable

    try:
        with open(tmp_path, "w", encoding="utf-8") as f:
            f.write(text)
            f.flush()
            os.fsync(f.fileno())

        # Atomic replacement if the target file is not open elsewhere
        os.replace(tmp_path, path)
    finally:
        # Clean up if something failed
        try:
            if os.path.exists(tmp_path):
                os.remove(tmp_path)
        except OSError:
            pass

def save_notes_json(text_area_content: str) -> None:
    """
    Serializes and saves the text from the notepad to a local JSON file.

    Args:
        text_area_content (str): text to save.
    """
    payload = json.dumps(
        {"content": text_area_content},
        ensure_ascii=False,
        indent=2,
    )
    save_to_file(NOTES_FILE, payload)

# ================
# Dash Application
# ================
app: Dash = Dash(__name__)
app.title = "Dash Demo"

app.layout = html.Div(
    [
        # Page title
        html.H1("Local Dash Notepad"),

        # Input field (restored on load via load_notes_json)
        dcc.Textarea(
            id="text_area",
            value=load_notes_json(),
            style={
                "width": "80%",
                "maxWidth": "900px",
                "height": "200px",
                "display": "block",
                "margin": "0 auto",
            },
            spellCheck=True,
            # keeps input in browser session
            persistence=True,
            persistence_type="session",
        ),

        # Save button
        html.Div(
            html.Button(
                "Save",
                id="btn_save",
                style={"marginTop": "10px"},
            )
        ),

        # Save message display area
        html.Div(
            id="message",
            style={"marginTop": "10px"},
        ),
    ],
    style={"textAlign": "center"},
)

@app.callback(
    Output("message", "children"),
    Input("btn_save", "n_clicks"),
    State("text_area", "value"),
    prevent_initial_call=True,
)
def save_notes(_n_clicks: int, content: str | None) -> str:
    """
    Saves the text entered in the local file when the button is clicked.
    Args:
        _n_clicks (int): number of clicks on the “btn_save” button.
        content (str | None): current content of the text area.
    Returns:

    Returns:
        str: message displayed to the user (success or error).

    Raises:
        PreventUpdate: prevents any updates if the content is
            unavailable.
    """
    if content is None:
        raise PreventUpdate

    try:
        save_notes_json(content)
        return "Notes saved locally."
    except OSError as exc:
        return f"Save error: {exc.strerror or str(exc)}"

if __name__ == "__main__":
    # Launch Dash development server
    app.run(debug=True)
```

---

## Verification and testing in PyCharm

## Code 6

```python
from __future__ import annotations
import json
from pathlib import Path
from app_notepad_2 import load_notes_json

def test_save_and_reload(tmp_path):
    """
    Verifies that the contents of a JSON file can be written
    and then correctly read back from disk.

    The pytest fixture `tmp_path` provides an isolated temporary folder
    that is automatically cleaned up after the test.
    """
    # Path to a temporary file
    file: Path = tmp_path / "notes.json"

    # Test content
    content: str = "Dash persistence test"

    # Write the JSON file simulating a save
    file.write_text(
        json.dumps({"content": content}),
        encoding="utf-8",
    )

    # Reading and decoding the file
    data: dict = json.loads(file.read_text(encoding="utf-8"))

    # Verifying the content
    assert data["content"] == content

def test_load_by_default(monkeypatch):
    """
    Checks that the `load_notes_json` function returns an empty string
    when the notes file does not exist.

    The pytest fixture `monkeypatch` allows you to temporarily modify
    a global variable in the module being tested.
    """
    # Replaces the NOTES_FILE constant with a non-existent path
    monkeypatch.setattr(
        "app_notepad_2.NOTES_FILE",
        Path("inexistant.json"),
    )

    # The function must return an empty string
    assert load_notes_json() == ""
```

---

# Chapter 8 - Themes and adaptive formatting

## The assets folder

### CSS style sheets

## Code 1

```python
from __future__ import annotations
from dash import Dash, html, dcc

# Create the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Create the layout
app.layout = html.Div(
    [
        # Main title
        html.H1("Main Title"),

        # Text input field
        dcc.Input(
            type="text",
            placeholder="Your name",
            style={"textAlign": "center"}
        ),

        # Line breaks
        html.Br(),
        html.Br(),

        # Dropdown list of options
        dcc.Dropdown(
            options=[
                {"label": "Option A", "value": "A"},
                {"label": "Option B", "value": "B"},
            ],
            value="A"
        ),

        # Additional line breaks for spacing
        html.Br(),
        html.Br(),

        # Simple button (no associated callback here)
        html.Button("OK"),

        # Footer
        html.Footer("Small CSS demo"),
    ]
)

# Application entry point
if __name__ == "__main__":
    app.run(debug=True)
```

---

## Code 2

```css
/* layout_1.css file */ 
body {
  background:#FFE0B2;
  font-family: "Segoe UI", Arial, sans-serif;
  text-align: center; /* centre le texte */
}
```

---

## Code 3

```css
/* layout_2.css file */ 
input, select, button {
  display: block;
  margin: 0.5rem auto;
  border-radius: 6px;
}
footer {
  color: #777;
  margin-top: 40px;
}
```

---

### Images

## Code 4

```python
from __future__ import annotations
from dash import Dash, html, dcc

# Create the Dash application
app: Dash = Dash(__name__)
app.title = "Dash Demo"

# Create the layout
app.layout = html.Div(
    [
        # Logo image (loaded from the assets/ folder)
        html.Img(
            src="/assets/logo.png",  # relative path to assets/
            alt="Logo",
            style={
                "height": "80px",
                "marginBottom": "20px",
            },
        ),

        # Main page title
        html.H1("Main title"),

        # Text input field
        dcc.Input(
            type="text",
            placeholder="Your name",
            style={"textAlign": "center"}
        ),

        # Line breaks for vertical spacing
        html.Br(),
        html.Br(),

        # Dropdown list of options
        dcc.Dropdown(
            options=[
                {"label": "Option A", "value": "A"},
                     {"label": "Option B", "value": "B"}
            ],
            value="A",
        ),

        # Additional line breaks for spacing
        html.Br(),
        html.Br(),

        # Simple button (no associated callback here)
        html.Button("OK"),

        # Footer
        html.Footer(
            [
                # Footer text
                html.Div("Small CSS demo"),

                # Decorative image in the footer
                html.Img(
                    src="/assets/bubble.png",
                    alt="Illustration footer",
                    style={
                        "marginTop": "8px",
                        "height": "40px",
                    },
                ),
            ],
            style={
                "marginTop": "40px",
                "textAlign": "center",
            },
        ),
    ]
)

# Application entry point
if __name__ == "__main__":
    app.run(debug=True)
```

---

## Dash Bootstrap Components

### Building an interface with DBC

## Code 6

```python
from __future__ import annotations
import dash_bootstrap_components as dbc
import pandas as pd
import plotly.express as px
from dash import Dash, Input, Output, dcc, html

# -------------------------------------------------------------------
# Local data (simple DataFrame for demonstration purposes)
# -------------------------------------------------------------------
df = pd.DataFrame(
    {
        "Town": ["Paris", "London", "Boston"],
        "Sales": [120, 80, 60],
    }
)

# -------------------------------------------------------------------
# Creation of the Dash application with the Bootstrap FLATLY theme
# -------------------------------------------------------------------
app: Dash = Dash(
    __name__,
    external_stylesheets=[dbc.themes.FLATLY],
)
app.title = "Dash Demo"

# -------------------------------------------------------------------
# Layout creation
# -------------------------------------------------------------------
app.layout = dbc.Container(
    [
        # -------------------------------------------------------------
        # Navigation bar (Navbar)
        # -------------------------------------------------------------
        dbc.NavbarSimple(
            brand="Local dashboard",				# text on the left
            color="primary",                # Bootstrap color
            dark=True,                      # light text
            class_name="mb-4",              # bottom margin (Bootstrap)
        ),

        # -------------------------------------------------------------
        # Bootstrap row containing 2 columns
        # -------------------------------------------------------------
        dbc.Row(
            [
                # -----------------------------------------------------
                # Left column: city selection
                # -----------------------------------------------------
                dbc.Col(
                    [
                        html.H3(
                            "City selection",
                            className="mb-2",
                        ),

                        # Dash dropdown menu
                        dcc.Dropdown(
                            id="towns_list",
                            options=[
                                {"label": v, "value": v}
                                for v in df["Town"].tolist()
                            ],
                            value="Paris",
                            clearable=False,
                            persistence=True,
                            persistence_type="session",
                            placeholder="Choose a city",
                        ),
                    ],
                    width=4,  # 4 columns out of 12 (Bootstrap)
                ),

                # -----------------------------------------------------
                # Right column: graph
                # -----------------------------------------------------
                dbc.Col(
                    [
                        dcc.Graph(id="graphic"),
                    ],
                    width=8,  # 8 columns out of 12 (Bootstrap)
                ),
            ]
        ),
    ],
    fluid=True,  # fluid width (Bootstrap))
)

# -------------------------------------------------------------------
# Callback
# -------------------------------------------------------------------
@app.callback(
    Output("graphic", "figure"),
    Input("towns_list", "value"),
)
def update_graph(town: str):
    """
    Updates the sales graph based on the selected city.

    Args:
        town (str): city selected from the drop-down list.

    Returns:
        plotly.graph_objects.Figure: graph to display.
    """
    if not town:
        # Default case: all cities
        fig = px.bar(
            df,
            x="Town",
            y="Sales",
            title="Sales (all cities)",
        )
    else:
        # Case with a selected city
        dff = df[df["Town"] == town]
        fig = px.bar(
            dff,
            x="Town",
            y="Sales",
            title=f"Sales in {town}",
        )

        # Visual adjustments to the graph
        fig.update_layout(
            margin=dict(l=80, r=20, t=60, b=40),
            yaxis_title="Sales",
            height=420,
        )
        fig.update_yaxes(
            automargin=True,
            ticks="outside",
            tickfont=dict(size=12),
        )

    return fig

# -------------------------------------------------------------------
# Launching the application
# -------------------------------------------------------------------
if __name__ == "__main__":
    app.run(debug=True)
```

---

### Adapting the layout

## Code 7

```python
import dash_bootstrap_components as dbc
from dash import html

# Bootstrap row: a row represents a horizontal line divided into columns
dbc.Row(
    [
        # ---------------------------------------------------
        # Left column: width=4 means: 4/12 of the total width
        # ---------------------------------------------------
        dbc.Col(
            html.Div("Left column"),
            width=4,
        ),
        # ----------------------------------------------------
        # Right column: width=8 means: 8/12 of the total width
        # ----------------------------------------------------
        dbc.Col(
            html.Div("Right column"),
            width=8,
        ),
    ]
)
```

---

### 3.6. A complete example: thematic mini-dashboard

## Code 9

```python
from __future__ import annotations
import dash_bootstrap_components as dbc
import plotly.express as px
from dash import Dash, Input, Output, dcc, html

# Data: Gapminder dataset filtered for the year 2007
DF = px.data.gapminder().query("year == 2007")

# Creation of the Dash application with the CERULEAN theme (Bootstrap)
app: Dash = Dash(__name__, external_stylesheets=[dbc.themes.CERULEAN])
app.title = "Dash Demo"

# Layout creation
app.layout = dbc.Container(
    [
        # Navigation bar (Navbar)
        dbc.Navbar(
            dbc.Container(
                dbc.NavbarBrand(
                    "Global indicators",
                    className="mx-auto",  # Bootstrap class: centers the text
                )
            ),
            color="info",          # Bootstrap color
            dark=True,             # light text (on colored background)
            class_name="mb-3",     # bottom margin (Bootstrap)
        ),

        # Main row: menu (left) + graph (right)
        dbc.Row(
            [
                # Left column (3/12): continent selection
                dbc.Col(
                    [
                        html.H3("Continent :"),
                        dcc.Dropdown(
                            id="continents_list",
                            options=[
                                {"label": c, "value": c}
                                for c in DF["continent"].unique()
                            ],
                            value="Europe",
                            clearable=False,
                            persistence=True,
                            persistence_type="session",
                            placeholder="Choose a continent",
                        ),
                    ],
                    width=3,
                ),

                # Right column (9/12): graph area              
                dbc.Col(
                    [
                        dcc.Graph(id="graphic"),
                    ],
                    width=9,
                ),
            ]
        ),

        # Footer
        html.Footer(
            "Local Dash 3.2.0 application — CERULEAN theme",
            className="mt-3",  # top margin (Bootstrap)
            style={"textAlign": "center"},
        ),
    ],
    fluid=True,  # fluid width (Bootstrap)
)

# Callback
@app.callback(
    Output("graphic", "figure"),
    Input("continents_list", "value"),
)
def update_graphic(continent: str) -> "px.Figure":
    """
    Updates the graph based on the selected continent.

    Args:
        continent (str): continent chosen from the drop-down list.

    Returns:
        plotly.graph_objs.Figure: Plotly graph (scatter) to display.
    """
    # Filtering data on the selected continent
    dff = DF[DF["continent"] == continent]

    # Scatter: GDP/capita vs life expectancy, size=population
    figure = px.scatter(
        dff,
        x="gdpPercap",
        y="lifeExp",
        size="pop",
        color="country",
        log_x=True,
        title=f"Life expectancy in {continent} (2007)",
    )

    # Simple visual adjustment
    figure.update_layout(height=420)
    return figure

if __name__ == "__main__":
    # Launch Dash development server
    app.run(debug=True)
```

---

## Verification and testing in PyCharm

## Code 10

```python
from __future__ import annotations
from dash.testing.application_runners import import_app

def test_theme_loading(dash_duo) -> None:
    """
    Checks that the Bootstrap theme (and therefore the formatting) is loaded.

    The test:
    - imports the Dash application from the Python module,
    - starts the test server,
    - checks that a Bootstrap Navbar element exists,
    - checks that the title “Continent” is displayed on the screen.
    """
    # Import the Dash application to be tested (module name without .py)
    app = import_app("explore_gapminder_bootstrap")

    # Start the Dash test server
    dash_duo.start_server(app)

    # The Bootstrap Navbar usually has the CSS class “.navbar”
    navbar = dash_duo.find_element(".navbar")
    assert navbar is not None

    # Verify that the title text contains “Continent”
    titre = dash_duo.find_element("h3")
    assert "Continent" in titre.text
```

---

# Appendix - Installing Firefox

## With macOS

## Code 1

```bash
brew install geckodriver
```

---

## Code 2

```bash
geckodriver --version
```

---

## Avec Linux

## Code 3

```bash
sudo apt update
sudo apt install firefox-geckodriver
```

---

## Code 4

```bash
geckodriver --version
```

---

## Avec Windows

## Code 5

```bash
geckodriver --version
```

---

## pytest settings

## Code 6

```python
[pytest]
pythonpath = .
addopts = --webdriver Firefox --headless
filterwarnings = default
```

---

# Appendix - A minimalist notepad with SQLite

## Imports

## Code 1

```python
from __future__ import annotations

import sqlite3
from pathlib import Path
from typing import Any

from dash import Dash, Input, Output, State, dash_table, dcc, html, no_update
from dash.exceptions import PreventUpdate
```

---

## Data access, persistence, and pluralization functions

## Code 2

```python
# ---------------------------------------------------------------------------
# SQLite helpers
# ---------------------------------------------------------------------------
DB_PATH = Path("data/demo.sqlite")

def get_con() -> sqlite3.Connection:
    """
    Creates and returns an SQLite connection configured for the application.

    This application reads and writes to a local SQLite file. A “raw” SQLite connection
    already works, but a few adjustments improve
    stability and development comfort:
    - ‘timeout=5’: SQLite locks the file during certain writes.
    	A timeout gives the engine a few seconds to complete the operation
        before raising a “database is locked” error.
    - ‘row_factory = sqlite3.Row’: allows columns to be accessed by name
        (e.g., row[“title”]) instead of only by index, which
    	simplifies mapping to dictionaries.
		- ‘PRAGMA journal_mode=WAL’: WAL (Write-Ahead Logging) improves
        read/write concurrency in scenarios where there is frequent reading and
        occasional writing. This mode is particularly useful with an
        interactive interface.

    Returns:
        sqlite3.Connection: ready-to-use SQLite connection. It must be
        used via a context manager (‘with get_con() as con:’) in order to
        ensure commit or rollback and clean closure.
    """
    con = sqlite3.connect(DB_PATH, timeout=5)
    con.row_factory = sqlite3.Row
    con.execute("PRAGMA journal_mode=WAL;")
    return con

def init_db() -> None:
    """
    Initializes the SQLite database if necessary.

    This function prepares the application's local storage:

    1) creates the parent folder of the ‘DB_PATH’ file if necessary,
    2) creates the ‘notes’ table if it does not exist.

    Schema of the ‘notes’ table:
    - id:
        INTEGER PRIMARY KEY AUTOINCREMENT
        unique technical identifier.
    - title:
        TEXT NOT NULL + CHECK
        the title cannot be empty.
    - note:
        TEXT NOT NULL + CHECK
        the note cannot be empty and its size is limited to 2000 characters.
    - created_at:
        TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        automatic date generated by SQLite upon insertion.

    Notes:
    - CHECK constraints protect the database even if a bug on the interface side
    	allows invalid values to pass through;
    - creation is idempotent: init_db() can be called at each
    	startup.
    """
    DB_PATH.parent.mkdir(parents=True, exist_ok=True)
    with get_con() as con:
        con.execute(
            """
            CREATE TABLE IF NOT EXISTS notes (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                title TEXT NOT NULL CHECK(length(trim(title)) > 0),
                note  TEXT NOT NULL CHECK(length(trim(note)) > 0 AND length(note) <= 2000),
                created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
            )
            """
        )

def insert_row(title: str, note: str) -> None:
    """
    Inserts a note into the database.

    This function writes to the ‘notes’ table:
    - normalization via ‘strip()’,
    - execution of a parameterized query to avoid direct SQL injection.

    Args:
        title: title of the note, must be non-empty after normalization.
        note: content of the note, must be non-empty after normalization and must not
					exceed 2000 characters.

		Raises:
			sqlite3.Error: in case of an SQLite problem (file inaccessible, lock,
			CHECK constraint error, etc.).
    """
    with get_con() as con:
        con.execute(
            "INSERT INTO notes(title, note) VALUES(?, ?)",
            (title.strip(), note.strip()),
        )

def update_row(row_id: int, title: str, note: str) -> None:
    """
    Updates an existing note.

    The update is based on the technical identifier ‘id’.
    Values are normalized with ‘strip()’ before writing.

    Args:
        row_id: identifier of the note to be updated.
        title: new title.
        note: new content.

    Raises:
        sqlite3.Error: SQLite error during the query (constraint, lock,
        etc.).
    """
    with get_con() as con:
        con.execute(
            "UPDATE notes SET title = ?, note = ? WHERE id = ?",
            (title.strip(), note.strip(), row_id),
        )

def delete_row(row_id: int) -> None:
    """
    Deletes a note by its ID.

    Args:
        row_id: ID of the note to be deleted.

    Raises:
        sqlite3.Error: SQLite error during the query.
    """
    with get_con() as con:
        con.execute("DELETE FROM notes WHERE id = ?", (row_id,))

def fetch_rows() -> list[dict[str, Any]]:
    """
    Retrieves all notes.

    The function reads all records from the table and returns a list of
    dictionaries, directly compatible with ‘dash_table.DataTable(data=...)’.

    Descending sort (‘ORDER BY id DESC’) puts the most recent notes at the top.

    Returns:
        list[dict[str, Any]]: list of records in the form of dictionaries.
        	Expected keys:
          - “id”
          - “title”
          - “note”
          - “created_at”
    """
    with get_con() as con:
        rows = con.execute(
            "SELECT id, title, note, created_at FROM notes ORDER BY id DESC"
        ).fetchall()
    return [dict(r) for r in rows]

def pluralization(nb_records: int) -> str:
    """
    Returns a user message in French.

    This function avoids repeating “if/else” logic in callbacks.

    Args:
        nb_records: number of records taken into account.

    Returns:
        str: the agreed message.
        str: le message accordé.
    """
    if nb_records <= 1:
        return "Modification saved"
    return "Modifications saved"
```

---

## Initializing the application and creating the layout

## Code 3

```python
# ---------------------------------------------------------------------------
# Dash Application
# ---------------------------------------------------------------------------
init_db()

app = Dash(__name__)
app.title = "Dash + SQLite"

# Factored styles
# ---------------------------------------------------------------------------

BASE_FONT_FAMILY = (
    "system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial"
)

LABEL_STYLE = {
    "display": "block",
    "marginBottom": "6px",
    "fontWeight": 600,
    "textAlign": "center"
}

FIELD_CONTAINER_STYLE = {
    "maxWidth": "700px",
    "margin": "0 auto 12px auto"
}

FIELD_STYLE = {
    "width": "100%",
    "padding": "8px 10px",
    "border": "1px solid #ddd",
    "borderRadius": "8px",
    "backgroundColor": "#FFF3D6",
    "boxSizing": "border-box",
    "textAlign": "center",
    "fontSize": "14px",
    "fontFamily": BASE_FONT_FAMILY
}

BTN_BASE_STYLE = {
    "padding": "10px 14px",
    "border": "1px solid #bbb",
    "borderRadius": "10px",
    "backgroundColor": "#FFF3E0",
    "cursor": "pointer",
    "fontFamily": BASE_FONT_FAMILY
}

# Layout
# ---------------------------------------------------------------------------

app.layout = html.Div(
    [
        # Title
        # --------------------------------------------------------------
        html.H1(
            "Dash Demo + SQLite (CRUD)",
            style={"margin": "0 0 16px 0"},
        ),

        # Form
        # --------------------------------------------------------------
        html.Div(
            [
                # --- Title (Input) ---
                html.Div(
                    [
                        html.Label(
                            "Title",
                            htmlFor="input_title",
                            style=LABEL_STYLE,
                        ),
                        dcc.Input(
                            id="input_title",
                            type="text",
                            placeholder="Title",
                            style=FIELD_STYLE,
                        ),
                    ],
                    style=FIELD_CONTAINER_STYLE,
                ),

                # --- Note (Textarea) ---
                html.Div(
                    [
                        html.Label(
                            "Note",
                            htmlFor="input_note",
                            style=LABEL_STYLE,
                        ),
                        dcc.Textarea(
                            id="input_note",
                            placeholder="Your note…",
                            style={
                                **FIELD_STYLE,
                                "height": "110px",
                                "resize": "both",
                            },
                        ),
                    ],
                    style=FIELD_CONTAINER_STYLE,
                ),

                # --- Add button ---
                html.Div(
                    html.Button(
                        "Add",
                        id="btn_add",
                        n_clicks=0,
                        style=BTN_BASE_STYLE,
                    ),
                    style={"textAlign": "right"},
                ),
            ],
            style={
                "padding": "14px",
                "border": "1px solid #e0c7a0",
                "borderRadius": "12px",
                "backgroundColor": "#FFF3D6",
                "marginBottom": "14px",
            },
        ),

        # Actions + message
        # --------------------------------------------------------------
        html.Div(
            [
                html.Div(
                    [
                        html.Button(
                            "Save changes",
                            id="btn_save",
                            n_clicks=0,
                            style=BTN_BASE_STYLE,
                        ),
                        html.Button(
                            "Delete selection",
                            id="btn_delete",
                            n_clicks=0,
                            style={
                                **BTN_BASE_STYLE,
                                "backgroundColor": "#FFEAEA",
                            },
                        ),
                    ],
                    style={
                        "display": "flex",
                        "gap": "10px",
                        "flexWrap": "wrap",
                        "justifyContent": "flex-start",
                    },
                ),
                html.Div(
                    id="msg",
                    style={
                        "marginTop": "10px",
                        "minHeight": "1.4em",
                        "padding": "8px 10px",
                        "border": "1px solid #e0c7a0",
                        "borderRadius": "10px",
                        "backgroundColor": "#FFF3D6",
                    },
                ),
            ],
            style={"marginBottom": "14px"},
        ),

        # Store + interval
        # --------------------------------------------------------------
        dcc.Store(id="notes_version", data=0),
        dcc.Interval(id="boot", interval=0, n_intervals=0, max_intervals=1),

        # DataTable
        # --------------------------------------------------------------
        html.Div(
            [
                html.H2(
                    "Notes",
                    style={"margin": "0 0 10px 0", "fontSize": "1.2em"},
                ),
                dash_table.DataTable(
                    id="tbl",
                    columns=[
                        {"name": "id", "id": "id", "editable": False},
                        {"name": "title", "id": "title", "editable": True},
                        {"name": "note", "id": "note", "editable": True},
                        {
                            "name": "created on",
                            "id": "created_at",
                            "editable": False,
                        },
                    ],
                    data=[],
                    editable=True,
                    row_selectable="single",
                    selected_rows=[],
                    page_size=8,
                    sort_action="native",
                    filter_action="native",
                    style_table={
                        "overflowX": "auto",
                        "border": "1px solid #e0c7a0",
                        "borderRadius": "12px",
                        "backgroundColor": "#FFF3D6",
                    },
                    style_header={
                        "backgroundColor": "#FFF3E0",
                        "fontWeight": "bold",
                        "borderBottom": "1px solid #e0c7a0",
                        "textAlign": "left",
                    },
                    style_cell={
                        "textAlign": "left",
                        "whiteSpace": "normal",
                        "height": "auto",
                        "padding": "10px",
                        "lineHeight": "1.35",
                        "fontFamily": BASE_FONT_FAMILY,
                        "fontSize": "14px",
                        "backgroundColor": "#FFF3D6",
                        "boxSizing": "border-box",
                    },
                    style_data_conditional=[
                        {
                            "if": {"column_id": "id"},
                            "backgroundColor": "#FFF8ED",
                            "color": "#666",
                            "fontWeight": "bold",
                            "width": "120px",
                        },
                        {
                            "if": {"state": "selected"},
                            "backgroundColor": "#FFE0B2",
                            "border": "1px solid #e0a96d",
                        },
                    ],
                ),
            ]
        ),
    ],
    style={
        "maxWidth": "980px",
        "margin": "24px auto",
        "padding": "0 14px 28px 14px",
        "fontFamily": BASE_FONT_FAMILY,
        "backgroundColor": "#FFE0B2",
    },
)
```

---

## Interaction management and application logic via callbacks

## Code 4

```python
# ---------------------------------------------------------------------------
# Callbacks
# ---------------------------------------------------------------------------

# Callback “Add a note”
@app.callback(
    Output("msg", "children"),
    Output("input_title", "value"),
    Output("input_note", "value"),
    Output("notes_version", "data"),
    Input("btn_add", "n_clicks"),
    State("input_title", "value"),
    State("input_note", "value"),
    State("notes_version", "data"),
    prevent_initial_call=True,
)
def add_note(
        _n: int, title: str | None, note: str | None, ver: int
) -> tuple[str, str | Any, str | Any, int | Any]:
    """
    Adds a note based on the content of the input fields.

    Trigger:
        click on the “Add” button (‘btn_add.n_clicks’).  

    Roles:
        - normalize fields (None -> “”, remove spaces);
        - validate:
              - title not empty,
            - note not empty and <= 2000 characters;
        - insert into the ‘notes’ table via the ‘insert_row’ function;
        - if successful:
              - display a message,
              - clear the fields,
            - increment the data version (‘notes_version’) to
              reload the table;
        - in case of error:
              - display a message,
            - do not overwrite the entry (no_update).

    Args:
        _n: number of clicks on the “Add” button (‘btn_add.n_clicks’).
            -> The value n is not used, only the event counts.
      title:  value of the ‘title’ field (may be None at startup).
      note: value of the note (may be None).
      ver: current version of the data stored in ‘dcc.Store’.

    Returns:
        tuple:
            str: message to display
            str or no_update: input_title.value -> “” if successful
            str or no_update: input_note.value -> “” if successful
            int or no_update: ver (current version of the data) + 1 if successful
    """
    title = (title or "").strip()
    contenu = (note or "").strip()

    if not title or not contenu:
        return "Please enter a title and a note.", no_update, no_update, no_update
    if len(contenu) > 2000:
        return "Note too long (max 2000 characters).", no_update, no_update, no_update

    try:
        insert_row(title, contenu)
        return "✔ Added note.", "", "", ver + 1
    except sqlite3.Error as exc:
        return f"SQLite error: {exc}", no_update, no_update, no_update

# Callback “Load table”
@app.callback(
    Output("tbl", "data"),
    Input("boot", "n_intervals"),
    Input("notes_version", "data"),
)
def load_table(_boot: int, _ver: int) -> list[dict[str, Any]]:
    """
    Loads (or reloads) the DataTable from SQLite.

    Triggers:
        - ‘boot.n_intervals’: once at startup,
        - ‘notes_version.data’: after each write (insert/update/delete).

    Roles:
        - read all notes in the ‘notes’ table via the ‘fetch_rows’ function,
        - return a list of dictionaries compatible with ‘DataTable.data’.

    Args:
        _boot: intervals of the ‘dcc.Interval’ component.
               -> The value is not used, only the event counts.
          _ver: data version
                -> The value is not used, only the event matters.

    Returns:
        list[dict[str, Any]]: rows to display in the DataTable.
    """
    return fetch_rows()

# Callback “Save table”
@app.callback(
    Output("msg", "children", allow_duplicate=True),
    Output("notes_version", "data", allow_duplicate=True),
    Input("btn_save", "n_clicks"),
    State("tbl", "data"),
    State("notes_version", "data"),
    prevent_initial_call=True,
)
def save_all(
        _n: int, data: list[dict[str, Any]] | None, ver: int
) -> tuple[str, int | Any]:
    """
    Save all rows from the DataTable to the ‘notes’ table.

    Trigger:
        Click on the “Save changes” button (‘btn_save.n_clicks’).

    Strategy:
        An ‘UPDATE’ is executed for each row in the DataTable. This is simple and
        readable, but not optimal if the table becomes very large.
        In a “production” version, it would be necessary to:
            - detect only the modified cells,
            - or perform a targeted backup by row.

    Validations:
        - title and note not empty (after strip),
        - note <= 2000 characters.

    Args:
        _n: number of clicks on the ‘btn_save.n_clicks’ button.
            -> The value n is not used, only the event counts.
        data: current DataTable data. Each dictionary must define
            the keys “id”, ‘title’ and “note”.
        ver: current version of the data stored in ‘dcc.Store’.

    Returns:
        tuple:
          str: message to display
          int or no_update: ver (current version of the data) + 1 if successful

    Raises:
        PreventUpdate: if ‘data’ is empty / None.
    """
    if not data:
        raise PreventUpdate

    try:
        updated = 0
        for r in data:
            row_id = int(r["id"])
            title = (r.get("title") or "").strip()
            note = (r.get("note") or "").strip()

            if not title or not note:
                return "Error: title/note cannot be empty.", no_update
            if len(note) > 2000:
                return f"Error: note too long (id={row_id}).", no_update

            update_row(row_id, title, note)
            updated += 1

        return pluralization(updated), ver + 1
    except (ValueError, KeyError, sqlite3.Error) as exc:
        return f"Save error: {exc}", no_update

# Callback “Delete selected note”
@app.callback(
    Output("msg", "children", allow_duplicate=True),
    Output("notes_version", "data", allow_duplicate=True),
    Input("btn_delete", "n_clicks"),
    State("tbl", "data"),
    State("tbl", "selected_rows"),
    State("notes_version", "data"),
    prevent_initial_call=True,
)
def delete_selected(
        _n: int,
        data: list[dict[str, Any]] | None,
        selected: list[int] | None,
        ver: int,
) -> tuple[str, int | Any]:
    """
    Deletes the selected row.

    Trigger:
        click on the “Delete selection” button (‘btn_delete.n_clicks’).

    Selection:
        the DataTable is configured as ‘row_selectable=“single”’. We therefore expect
        a ‘selected_rows’ list of size 0 or 1.

    Roles:
        - check that a selection exists,
        - retrieve the ID of the selected row via ‘data[selected[0]]’,
        - delete from the database using the ‘delete_row’ function,
        - increment the data version (‘notes_version’) to reload
            the table.

    Args:
        _n: number of clicks on the “Delete selection” button (btn_delete.n_clicks')
            -> The value n is not used, only the event counts.
        data: current data in the table.
        selected: index of the selected row.
        ver: current version of the data stored in ‘dcc.Store’.

    Returns:
        tuple:
          str: message to display
          int or no_update: ver (current version of the data) + 1 if successful
    """
    if not data or not selected:
        return "Select a row to delete.", no_update

    try:
        idx = selected[0]
        row_id = int(data[idx]["id"])
        delete_row(row_id)
        return f"✔ Line id={row_id} deleted.", ver + 1
    except (IndexError, KeyError, ValueError, sqlite3.Error) as exc:
        return f"Deletion error:  {exc}", no_update
```

---

## Launching the application

## Code 5

```python
if __name__ == "__main__":
    app.run(debug=True)
```
