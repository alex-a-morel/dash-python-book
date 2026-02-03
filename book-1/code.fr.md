# Chapitre 1 - Mise en place de l’environnement

## Préparer les dépendances

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
    Vérifie que toutes les dépendances principales du projet
    sont installées et importables.

    Ce test échoue si l'une des bibliothèques requises n'est pas
    présente ou si une incompatibilité empêche son import.
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

## Première application Dash

## Code 7

```python
from __future__ import annotations
from dash import Dash, html

app = Dash(__name__)
app.title = "Démo Dash"
app.layout = html.Div(
    children=[
        html.H1("Bonjour Dash 3.2.0 !"),
        html.P("Votre première application Dash locale est opérationnelle.")
    ]
)

if __name__ == "__main__":
    app.run(debug=True)
```

---

## Créer un premier test unitaire

## Code 10

```python
from __future__ import annotations
from dash import Dash, html

def test_app_starts() -> None:
    """
    Vérifie que l'application Dash peut être instanciée
    et qu'un layout peut être défini.
    """
    
    # Création de l'application Dash
    app: Dash = Dash(__name__)

    # Définition d'un layout minimal
    app.layout = html.Div("Test Dash 3.2.0")

    # Vérifications simples
    assert app is not None
    assert hasattr(app, "layout")
```

---

# Chapitre 2 - Comprendre Dash

## Anatomie d’une application Dash

## Code 1

```python
from __future__ import annotations
from dash import Dash, html, Input, Output, dcc

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Définition du layout (le contenu de la page)
app.layout = html.Div(
    [
        # Champ de saisie du nom
        dcc.Input(
            id="name",
            value="Dash",
            type="text",
        ),

        # Zone d'affichage du message
        html.Div(
            id="greeting",
        ),
    ]
)

# Définition du callback (l'interactivité)
@app.callback(
    Output("greeting", "children"),
    Input("name", "value"),
)
def update_greeting(name: str) -> str:
    """
    Met à jour le message de salutation affiché à l'écran.

    Args:
    	name (str) : texte saisi par l'utilisateur.

    Returns:
    	str : message à afficher.
    """
    return f"Bonjour {name} !"

if __name__ == "__main__":
    app.run(debug=True)
```

---

## Code 2

```python
from __future__ import annotations

# Import des éléments nécessaires depuis Dash :
# 	- Dash : classe principale qui crée l'application web
# 	- html : composants HTML (Div, etc.)
# 	- dcc  : composants "Dash Core Components" (Input, Graph, etc.)
# 	- Input / Output : objets décrivant les dépendances des callbacks
from dash import Dash, html, Input, Output, dcc

# Création de l'application Dash.
# 	Le nom du module (__name__) est utilisé par Dash/Flask pour la
# 	configuration interne et la découverte des ressources associées.
app = Dash(__name__)
# Définit l'intitulé de l'onglet du navigayeur.
app.title = "Démo Dash"

# Définition de la mise en page (layout) de l'application.
app.layout = html.Div([
    # Champ de saisie :
    # 	- id="name" : identifiant unique, utilisé par le callback
    # 	- value="Dash" : valeur initiale affichée dans le champ
    # 	- type="text" : champ texte HTML classique
    dcc.Input(id="name", value="Dash", type="text"),

    # Zone initiale, qui sera mise à jour dynamiquement par le
  	# callback en fonction de la saisie utilisateur :
    # 	- id="greeting" : identifiant unique, utilisé par le callback
    html.Div(id="greeting")
])

# Déclaration du callback Dash.
# 	Le  callback relie :
# 		- une ou plusieurs entrées (Input),
# 		- à une ou plusieurs sorties (Output).
#
# 	Ici :
# 		- Input("name", "value") : le callback est déclenché à chaque
#   		modification de la valeur du champ de saisie identifié 'name'
# 		- Output("greeting", "children") : le contenu de la <div>
#	      identifiée "greeting" est mis à jour automatiquement.
@app.callback(
    Output("greeting", "children"),
    Input("name", "value")
)
def update_greeting(name) -> str:
    """
    Met à jour le message de salutation en fonction de la saisie
    de l'utilisateur.

    Cette fonction est appelée automatiquement par Dash à chaque
    changement de la valeur du champ de saisie. Le paramètre `name`
    correspond à la valeur actuelle du composant dcc.Input.
    
    Args:
    	name (str) : texte saisi par l'utilisateur.

    Returns:
    	str : message à afficher.
    """
    return f"Bonjour {name} !"

# Point d'entrée de l'application.
if __name__ == "__main__":
    # Lancement du serveur de développement Dash. Le mode debug
    # permet le rechargement automatique et la notification des
    # messages d'erreur détaillés pendant le développement.
    app.run(debug=True)
```

---

## Tester la structure d'un layout

## Code 3

```python
from __future__ import annotations
from dash import Dash, html

def test_layout_structure() -> None:
    """
    Vérifie que l'application Dash possède un layout valide
    et que sa structure de base est correcte.
    """
    
    # Création de l'application Dash
    app: Dash = Dash(__name__)

    # Définition d'un layout simple avec deux composants
    app.layout = html.Div(
        [
            html.H1("Test structure Dash 3.2.0"),
            html.P("Ce test vérifie que le layout se charge correctement."),
        ]
    )

    # Vérifie que le layout existe
    assert app.layout is not None

    # Vérifie que les enfants du layout sont définis une liste
    assert isinstance(app.layout.children, list)

    # Vérifie qu'au moins un titre H1 est présent dans le layout
    assert any(
        isinstance(child, html.H1)
        for child in app.layout.children
    )
```

---

# Chapitre 3 - Layout et structure de l’interface

## Le layout : la charpente visuelle de l’application

## Code 1

```python
from __future__ import annotations
from dash import Dash, html

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Définition du layout (le contenu de la page)
app.layout = html.Div(
    [
        # Un titre
        html.H1("Analyse de données locales"),

        # Un paragraphe
        html.P("Première structure Dash 3.2.0."),
    ]
)

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## Déclarer un layout

## Code 2

```python
from __future__ import annotations
from dash import Dash, html, dcc

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Définition du layout (le contenu de la page)
app.layout = html.Div(
    children=[
        # Un titre
        html.H1("Tableau de bord local"),

        # Un paragraphe
        html.P("Application Dash exécutée en local."),

        # Un conteneur pour définir un formulaire de saisie
        html.Div(
            children=[
                html.Div(html.Label("Nom :")),
                html.Div(dcc.Input(type="text")),
                html.Button("Valider")
            ],
        ),

        # Conteneur vide (réservé à un contenu futur)
        html.Div(),
    ],
)

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## Le style

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

## Hiérarchiser et documenter

## Code 6

```python
# Zone 1
header_area = html.Div([
    html.H1("Application locale Dash"),
    html.P("Exemple de layout hiérarchisé.")
])

# Zone 2
content_area = html.Div([
    html.Label("Sélection :"),
    dcc.Dropdown(["A", "B", "C"], "A")
])

app.layout = html.Div([header_area, content_area])
```

---

## Un exemple complet

### Le style

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
    Construit une zone contenant un seul composant enfant.

    Cette fonction illustre le cas où `children` reçoit un
    composant unique : un paragraphe.
    """
    return html.Div(
        children=html.P(
            "Children mono-composant (paragraphe)",
            title="exemple simple",
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
    Construit une zone contenant plusieurs composants enfants.

    Cette fonction illustre le cas où `children` reçoit deux
    composants : une zone de saisie et un bouton.
    """
    return html.Div(
        children=[
            html.P("Children multi-composants (input et bouton)"),

            # Ligne 1 : une zone de saisie
            html.Div(
                dcc.Input(
                    type="text",
                    placeholder="Votre nom",
                    value="Dash",
                    maxLength=40,
                    style={"textAlign": "center"}
                ),
                style={"textAlign": "center", "margin": "8px"}
            ),

            # Ligne 2 : un bouton
            html.Div(
                html.Button(
                    "Bouton (désactivé, pas de callback)",
                    n_clicks=0,
                    title="Désactivé (aucun callback)",
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
    Construit le layout principal de l'application.

    Cette fonction est appelée par Dash pour générer
    le contenu de la page.
    """
    return html.Div(
        className="page",
        children=[
            # Un seul composant retourné par la fonction
            build_area_01(),

            # Deux composants retournés par la fonction
            build_area_02(),
        ],
    )

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Création du layout
app.layout = serve_layout

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## Tester la structure d'un layout

## Code 9

```python
from __future__ import annotations
from dash import Dash, html, dcc

def test_layout_construction() -> None:
    """
    Vérifie que le layout Dash contient les éléments essentiels
    et que leur structure est correcte.
    """
    
    # Création de l'application Dash
    app: Dash = Dash(__name__)

    # Création du layout avec trois composants
    app.layout = html.Div(
        [
            html.H1("Titre test"),
            dcc.Input(id="input_test"),
            html.Div(id="result_test"),
        ]
    )

    # Vérifie que le layout contient une liste de composants
    assert isinstance(app.layout.children, list)

    # Vérifie la présence d'un titre H1
    assert any(
        isinstance(child, html.H1)
        for child in app.layout.children
    )

    # Récupère les identifiants des composants ayant un attribut 'id'
    ids = [
        child.id
        for child in app.layout.children
        if hasattr(child, "id")
    ]

    # Vérifie que les composants attendus sont présents
    assert "input_test" in ids
    assert "result_test" in ids
```

---

# Chapitre 4 - Les callbacks

## Un exemple complet avec un texte interactif

## Code 2

```python
from __future__ import annotations
from dash import Dash, html, Input, Output, dcc

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Création du layout
app.layout = html.Div(
    [
        # Titre principal
        html.H1("Essai de callbacks Dash 3.2.0"),

        # Texte d'instruction
        html.P("Saisissez votre nom :"),

        # Champ de saisie du nom
        dcc.Input(
            id="name",
            type="text",
            value="",
            style={"textAlign": "center"},
        ),

        # Zone d'affichage du résultat
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
def maj_greeting(name: str) -> str:
    """
    Met à jour le message affiché en fonction du nom saisi.

    Args:
    	name (str) : texte saisi par l'utilisateur.

    Returns:
    	str : message de salutation affiché.
    """
    return f"Bonjour {name} !"

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## Plusieurs entrées et une sortie

## Code 3

```python
from __future__ import annotations
from dash import Dash, html, dcc, Input, Output

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Création du layout
app.layout = html.Div(
    [
        # Premier champ numérique
        dcc.Input(
            id="first_value",
            value="2",
            type="number",
            style={"textAlign": "center"},
        ),

        # Second champ numérique
        dcc.Input(
            id="second_value",
            value="3",
            type="number",
            style={"textAlign": "center"},
        ),

        # Zone d'affichage du résultat
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
    Calcule la somme de deux valeurs saisies par l'utilisateur.

    Args :
    	a (float | int | None) : première valeur numérique saisie.
    	b (float | int | None) : seconde valeur numérique saisie.

    Returns:
    	str : résultat du calcul ou message d'erreur.
    """
    try:
        x = float(a)
        y = float(b)
        return f"{x} + {y} = {x + y}"
    except (TypeError, ValueError):
        return "Entrée non valide."

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## Plusieurs sorties pour synchroniser des composants

## Code 4

```python
@app.callback(
    Output("first_text", "children"),
    Output("second_text", "children"),
    Input("name", "value"),
)
def double_update(name: str) -> tuple[str, str]:
    """
    Met à jour deux zones de texte à partir d'une seule saisie.

    Args:
    	name (str) : texte saisi par l'utilisateur.

    Returns:
    	tuple[str, str] :
    	- le message affichant le nom saisi
      - le message affichant la longueur du nom
    """
    return (
        f"Nom saisi : {name}",
        f"Longueur : {len(name)} caractères",
    )
```

---

## Input, Output, State : comprendre la différence

## Code 5

```python
from __future__ import annotations
from dash import Dash, html, Input, Output, State, dcc

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Création du layout
app.layout = html.Div(
    [
        # Texte d'instruction
        html.P("Saisissez votre prénom :"),

        # Champ de saisie du nom
        html.Div(
            dcc.Input(
                id="name",
                type="text",
                value="",
                debounce=True,
                style={"textAlign": "center", "marginBottom": "10px"})
        ),

        # Bouton de validation
        html.Div(
            html.Button(
                "OK",
                id="ok_button",
                n_clicks=0,
                style={"marginBottom": "10px"},)
        ),

        # Zone d'affichage du message
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
    Affiche un message lorsque l'utilisateur clique sur le bouton.

    Args:
    	n_clicks (int) : nombre de clics sur le bouton "OK".
    	name (str) : nom saisi par l'utilisateur.

    Returns:
    	str : message affiché dans la page.
    """
    if not n_clicks:
        return ""

    # Ajoute un espace uniquement si un nom est saisi
    suffix = f" {name}" if name else ""
    
    return f"Bonjour{suffix}, vous avez cliqué {n_clicks} fois."

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## Gérer les erreurs et la stabilité

## Exercice complet : calcul interactif

## Code 7

```python
from __future__ import annotations
from dash import Dash, html, dcc, Input, Output, State
from dash.exceptions import PreventUpdate

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Création du layout
app.layout = html.Div(
    [
        # Titre de la page
        html.H1("Additionner deux nombres"),

        # Premier champ numérique
        dcc.Input(
            id="first_value",
            type="number",
            value=0,
            style={"textAlign": "center"},
        ),

        # Second champ numérique
        dcc.Input(
            id="second_value",
            type="number",
            value=0,
            style={"textAlign": "center"},
        ),

        # Bouton déclencheur du calcul
        html.Div(html.Button(
            "Additionner",
            id="button_add",
        style={"marginTop": "10px"})
        ),

        # Zone d'affichage du résultat
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
    Calcule la somme de deux nombres saisis par l'utilisateur
    après un clic sur le bouton.

    Args:
    	n_clicks (int) : nombre de clics sur le bouton "Additionner".
    	first_value (float | int | None) : première valeur numérique saisie.
    	second_value (float | int | None) : seconde valeur numérique saisie.

    Returns:
    	str : résultat du calcul ou message d'erreur.
    """
    
    # Empêche l'exécution du callback avant le premier clic
    if not n_clicks:
        raise PreventUpdate

    try:
        result = float(first_value) + float(second_value)
        return f"Résultat : {result}"
    except (TypeError, ValueError):
        return "Erreur : veuillez saisir des nombres valides."

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## Chargement du layout et validation_layout

## Code 8

```python
from __future__ import annotations
from dash import Dash, Input, Output, dcc, html
from dash.exceptions import PreventUpdate

# Options du menu déroulant
OPTIONS = [
    {"label": "Option A", "value": "A"},
    {"label": "Option B", "value": "B"},
]

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# 1) Layout de validation : ce layout minimal sert
#    uniquement à déclarer les identifiants (id) utilisés
#    dans les callbacks, à des fins de contrôle.
app.validation_layout = html.Div(
    [
        dcc.Dropdown(id="options_list"),
        html.Div(id="main_container"),
    ]
)

# 2) Création du layout réel via une fonction.
def serve_layout() -> html.Div:
    """
    Construit le layout de l'application.

    Cette fonction est appelée par Dash lors du chargement
    ou du rechargement de la page.
    """
    return html.Div(
        [
            # Menu déroulant avec les options
            dcc.Dropdown(
                id="options_list",
                options=OPTIONS,
                value="A",
                clearable=False,
            ),

            # Zone principale mise à jour par le callback
            html.Div(
                id="main_container",
                style={"marginTop": "12px"},
            ),

            # ... composants plus complexes possibles ici ...
        ],
        style={
            "maxWidth": "600px",
            "margin": "40px auto",
            "textAlign": "center",
        },
    )

# 3) Affectation du layout réel.
app.layout = serve_layout

# 4) Callback.
@app.callback(
    Output("main_container", "children"),
    Input("options_list", "value"),
)
def update(option_selected: str) -> str:
    """
    Met à jour le contenu principal selon l'option choisie.

    Args:
    	option_selected (str) : valeur sélectionnée dans la liste déroulante.

    Returns:
    	str : texte affiché dans la zone principale.
    """
    # Sécurité : aucune mise à jour si aucune option
    if option_selected is None:
        raise PreventUpdate

    return f"Choix : {option_selected}"

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## Vérification de bout en bout

## Code 9

```python
from __future__ import annotations
from dash import Dash, html, Input, Output, dcc

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Création du layout
app.layout = html.Div(
    [
        # Titre principal
        html.H1("Essai de callbacks Dash 3.2.0"),

        # Texte d'instruction
        html.P("Saisissez votre nom :"),

        # Champ de saisie du nom
        dcc.Input(
            id="name",
            type="text",
            value="",
            style={"textAlign": "center"},
        ),

        # Zone d'affichage du résultat
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
def maj_greeting(name: str) -> str:
    """
    Met à jour le message affiché en fonction du nom saisi.

    Args:
    	name (str) : texte saisi par l'utilisateur.

    Returns:
    	str : message de salutation affiché.
    """
    return f"Bonjour {name} !"

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## Code 10

```python
from __future__ import annotations
from dash.testing.application_runners import import_app

def test_callback_structure(dash_duo) -> None:
    """
    Vérifie qu'un callback Dash réagit correctement
    à une saisie utilisateur.

    Le test :
    - lance l'application Dash
    - saisit un texte dans le champ 'name'
    - vérifie que le message affiché est correct
    """
    
    # Importation de l'application Dash à tester
    app = import_app("app_callback")

    # Démarrage du serveur Dash de test
    dash_duo.start_server(app)

    # Recherche du champ de saisie par son identifiant
    champ = dash_duo.find_element("#name")

    # Simulation de la saisie utilisateur
    champ.send_keys("Python")

    # Vérifie que le texte attendu apparaît à l'écran
    dash_duo.wait_for_text_to_equal(
        "#result",
        "Bonjour Python !",
        timeout=4,
    )
```

---

# Chapitre 5 - Interactivité et visualisation

## Un premier graphique Plotly dans Dash

## Code 2

```python
from __future__ import annotations
import pandas as pd
import plotly.express as px
from dash import Dash, html, dcc

# Création des données locales
DF = pd.DataFrame(
    {
        "Ville": ["Paris", "Lyon", "Marseille",
                  "Bordeaux", "Toulouse"],
        "Population": [2.1, 0.5, 0.87, 0.26, 0.48],
        "Région": ["Île-de-France", "Auvergne-Rhône-Alpes",
                   "Provence-Alpes-Côte d’Azur",
                   "Nouvelle-Aquitaine", "Occitanie"],
    }
)

# Création de la figure Plotly
figure = px.bar(
    DF,
    x="Ville",
    y="Population",
    color="Région",
)

# Centrage du titre de la figure Plotly
figure.update_layout(
    title={
        "text": "Population (en millions)",
        "x": 0.5,
        "xanchor": "center",
    }
)

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Création du layout
app.layout = html.Div(
    [
        # Titre de la page
        html.H1(
            "Graphique Plotly intégré dans Dash",
            style={"textAlign": "center"},
        ),

        # Intégration du graphique Plotly
        dcc.Graph(
            id="graphic",
            figure=figure,
        ),
    ]
)

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## Dynamiser la figure avec un graphique réactif

## Code 3

```python
from __future__ import annotations
import pandas as pd
import plotly.express as px
from dash import Dash, html, dcc, Input, Output

# Création des données locales
DF = pd.DataFrame(
    {
        "Ville": ["Paris", "Lyon", "Marseille",
                  "Bordeaux", "Toulouse"],
        "Population": [2.1, 0.5, 0.87, 0.26, 0.48],
        "Région": ["Île-de-France", "Auvergne-Rhône-Alpes",
                   "Provence-Alpes-Côte d’Azur",
                   "Nouvelle-Aquitaine", "Occitanie"],
    }
)

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Création du layout
app.layout = html.Div(
    [
        # Titre de la page
        html.H1(
            "Population par région",
            style={"textAlign": "center"},
        ),

        # Liste déroulante pour choisir une région
        dcc.Dropdown(
            id="regions_list",
            options=[
                {"label": r, "value": r}
                for r in DF["Région"].unique()
            ],
            value="Île-de-France",
            clearable=False,
            style={"textAlign": "center"},
        ),

        # Zone d'affichage du graphique
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
    Met à jour le graphique en fonction de la région sélectionnée.

    Args:
    	region_selected (str) : région choisie dans la liste déroulante.

    Returns:
    	plotly.graph_objs.Figure : graphique Plotly à afficher.
    """
    # Filtrage des données selon la région
    dff = DF[DF["Région"] == region_selected]

    # Création du graphique Plotly
    figure = px.bar(
        dff,
        x="Ville",
        y="Population",
        color="Ville",
    )

    return figure

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## Explorer les interactions natives

## Code 4

```python
from __future__ import annotations
import pandas as pd
import plotly.express as px
from dash import Dash, html, dcc, Input, Output

# Création des données locales
DF = pd.DataFrame(
    {
        "Ville": ["Paris", "Lyon", "Marseille",
                  "Bordeaux", "Toulouse"],
        "Population": [2.1, 0.5, 0.87, 0.26, 0.48],
        "Région": ["Île-de-France", "Auvergne-Rhône-Alpes",
                   "Provence-Alpes-Côte d’Azur",
                   "Nouvelle-Aquitaine", "Occitanie"],
    }
)

# Message affiché sous le graphique
MSG_DEFAULT = "Cliquer sur une barre pour afficher le détail."

# Création de la figure Plotly
figure = px.bar(
    DF,
    x="Ville",
    y="Population",
    color="Région",
)

# Centrage du titre de la figure Plotly
figure.update_layout(
    title={
        "text": "Population (en millions)",
        "x": 0.5,
        "xanchor": "center",
    }
)

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Création du layout
app.layout = html.Div(
    [
        # Titre de la page
        html.H1(
            "Graphique Plotly intégré dans Dash",
            style={"textAlign": "center"}
        ),

        # Intégration du graphique Plotly
        dcc.Graph(
            id="graphic",
            figure=figure,
        ),

        # Message d'information
        html.Div(
            id="message",
            children=MSG_DEFAULT,
            style={
                "marginTop": "10px",
                "textAlign": "center",
            },
        ),

        # Zone d'affichage des détails
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
    Affiche les informations de la barre cliquée.

    Args:
        click_data (dict | None) : données renvoyées par Plotly
        	lors d'un clic sur le graphique.

    Returns:
        str : texte descriptif de la ville sélectionnée.
    """
    # Aucun clic → rien à afficher
    if not click_data:
        return ""

    # Récupération du point cliqué
    point = click_data["points"][0]
    town = point.get("x", "")
    population = point.get("y", "")

    return f"{town} : {population} million(s) d'habitants"

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## Exemple complet pour explorer un dataset réel

## Code 5

```python
from __future__ import annotations
import plotly.express as px
from dash import Dash, html, dcc, Input, Output

# Chargement du jeu de données Gapminder Plotly
DF = px.data.gapminder()

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Création du layout
app.layout = html.Div(
    [
        # Titre principal
        html.H1("Explorateur de données locales Gapminder",
                style = {"textAlign": "center"}),

        # Libellé du curseur
        html.Label("Choisissez une année :"),

        # Slider de sélection de l'année
        dcc.Slider(
            id="year_slider",
            value=int(DF["year"].min()),     # valeur initiale
            min=int(DF["year"].min()),       # année minimale
            max=int(DF["year"].max()),       # année maximale
            step=None,                       # uniquement les valeurs définies
            marks={
                str(year): str(year)
                for year in DF["year"].unique()
            },                               # graduations par années
        ),

        # Zone d'affichage du graphique
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
    Met à jour le graphique en fonction de l'année sélectionnée.

    Args:
    	year (int) : année choisie via le slider.

    Returns:
    	plotly.graph_objs.Figure : graphique Plotly à afficher.
    """
    # Filtrage des données pour l'année sélectionnée
    dff = DF[DF["year"] == year]

    # Création du graphique de dispersion (scatter)
    figure = px.scatter(
        dff,
        x="gdpPercap",              # PIB par habitant
        y="lifeExp",                # espérance de vie
        size="pop",                 # taille des bulles
        color="continent",          # couleur par continent
        hover_name="country",       # info au survol
        log_x=True,                 # axe X logarithmique
        size_max=55,
        title=f"Espérance de vie et PIB par habitant ({year})"
    )
    figure.update_layout(
        plot_bgcolor="#FFE0B2"      # Couleur de fond du graphique
    )

    return figure

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## Code 6

```python
 # Le graphique s'adapte à l'année sélectionnée sur le slider
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
                        "pop": False, 					# ne pas montrer
            						"continent": False,    # ne pas montrer
            						"gdpPercap": ":,.0f",  # formater (séparateurs, 0 décimale)
            						"lifeExp": ":.1f",     # 1 décimale
                      },
                   		title=f"Espérance de vie et PIB par habitant ({year})")
  return figure
```

---

## Vérification et tests dans PyCharm

## Code 7

```python
from __future__ import annotations
from dash.testing.application_runners import import_app

def test_app_graphic_2(dash_duo) -> None:
    """
    Vérifie que l'application démarre et que le graphique est présent
    après un changement de région via le menu déroulant.

    Le test :
    - charge l'application Dash
    - démarre le serveur de test
    - sélectionne une région dans le Dropdown
    - vérifie la présence du titre et du graphique
    """
    # Importation de l'application Dash à tester (nom du module sans .py)
    app = import_app("app_graphic_2")

    # Démarrage du serveur Dash de test
    dash_duo.start_server(app)

    # Sélection d'une valeur dans le Dropdown Dash
    dash_duo.select_dcc_dropdown("#regions_list", "Occitanie")

    # Vérifie que la page est bien chargée (titre attendu)
    dash_duo.wait_for_text_to_equal("h1", "Population par région")

    # Vérifie que le composant Graph est présent dans la page
    figure = dash_duo.find_element("#graphic")
    assert figure is not None
```

---

# Chapitre 6 - Gestion des données locales

## Lire un fichier local

## Code 1

```python
from __future__ import annotations
from dash import Dash, html, dcc
import plotly.express as px
import pandas as pd

app = Dash(__name__)
df = pd.read_csv("data/ventes.csv")

app.layout = html.Div([
    html.H1(
        "Chiffre d’affaires par ville",
        style={"textAlign": "center"}
    ),
    dcc.Graph(
        figure=px.bar(
            df,
            x="Ville",
            y="ChiffreAffaires",
            title="Ventes 2023",
            labels={"ChiffreAffaires": "Chiffre d’affaires (€)"},
        )
    )
])

if __name__ == "__main__":
    app.run(debug=True)
```

---

## Choisir un fichier depuis l’interface

## Code 2

```python
from __future__ import annotations
import base64
from dash import Dash, Input, Output, State, dcc, html
from dash.exceptions import PreventUpdate

# ==========================
# Styles inline (constantes)
# ==========================
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

# =========================================
# Fonction utilitaire : carte d’un fichier
# =========================================
def build_file_card(
    name: str,
    mime_type: str,
    size_kb: float,
    content: str,
) -> html.Div:
    """
    Construit une carte d'affichage pour un fichier uploadé.

    Args:
    	name (str) : nom du fichier.
    	mime_type (str) : type MIME du fichier (image/png, text/csv, etc.).
    	size_kb (float) : taille du fichier en kilo-octets.
    	content (str) : contenu encodé en base64 (data:<mime>;base64,...).

    Returns:
    	html.Div : carte Dash contenant les informations du fichier.
    """
    # Aperçu uniquement pour les images
    preview = (
        html.Img(src=content, style=PREVIEW_STYLE)
        if mime_type.startswith("image/")
        else None
    )

    return html.Div(
        [
            html.H4(name, style=TITLE_STYLE),
            html.Div(f"Type : {mime_type}"),
            html.Div(f"Taille : {size_kb:.1f} KB"),
            preview,
        ],
        style=CARD_STYLE,
    )

# ================
# Application Dash
# ================
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Création du layout
app.layout = html.Div(
    [
        # Zone d’upload
        dcc.Upload(
            id="upload",
            accept="image/*,.csv",
            multiple=True,
            children=html.Div(
                [
                    html.Div("Glisser-déposer vos fichiers ici"),
                    html.Div("ou"),
                    html.Button("Choisir des fichiers", type="button"),
                ],
                style=UPLOAD_CHILD_STYLE,
            ),
            style=UPLOAD_BOX_STYLE,
        ),

        # Zone d’affichage des résultats
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
    Affiche les informations des fichiers uploadés.

    Args:
    	contents (list[str] | None) : liste des contenus encodés en base64.
    	filenames (list[str] | None) : liste des noms de fichiers.
    	last_modified (list[int] | None) : dates de modification (non utilisées ici).

    Returns:
    	list[html.Div] : liste de cartes d'affichage des fichiers.
    """
    if not contents:
        raise PreventUpdate

    items: list[html.Div] = []

    for content, name in zip(contents, filenames):
        # Découpe du header et du contenu base64
        header, encoded = content.split(",", 1)

        # Extraction du type MIME
        mime_type = header.split(";")[0].replace("data:", "")

        # Calcul de la taille du fichier
        size_kb = len(base64.b64decode(encoded)) / 1024

        # Construction de la carte
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

## Afficher les données sous forme de table

## Code 4

```python
from __future__ import annotations
import base64
import io
import pandas as pd
from dash import Dash, Input, Output, State, dash_table, dcc, html

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Création du layout
app.layout = html.Div(
    [
        # Titre principal
        html.H1(
            "Importation de fichier CSV local",
            style={"textAlign": "center"},
        ),

        # Zone d'upload (glisser-déposer ou sélection)
        dcc.Upload(
            id="upload_file",
            children=html.Div(
                ["Glisser-déposer un fichier ou cliquer ici pour en choisir un"]
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

        # Zone d'affichage d'informations sur le fichier
        html.Div(
            id="file_info",
            style={"textAlign": "center",
                   "padding": "10px 0"},
        ),

        # Zone d'affichage de la table
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
    Charge un fichier CSV envoyé via dcc.Upload, le lit avec Pandas,
    puis affiche :
    - un résumé technique (nom, nombre de lignes, colonnes),
    - une DataTable Dash avec tri/filtre.

    Args:
    	contents (str | None) : contenu du fichier encodé en base64
    	    (format: data:...;base64,...).
    	file_name (str | None) : nom du fichier sélectionné.

    Returns:
    	tuple[object, object] :
        - contenu de la zone "file_info" (texte ou html.Div)
        - contenu de la zone "table" (DataTable ou None)
    """
    # Aucun fichier sélectionné → message et pas de table
    if not contents:
        return "Aucun fichier chargé.", None

    try:
        # contents = "data:<mime>;base64,<payload>"
        _, contenu_base64 = contents.split(",", 1)
        raw = base64.b64decode(contenu_base64)

        # Lecture : UTF-8/UTF-8-SIG et remplacement des caractères invalides
        text = raw.decode("utf-8-sig", errors="replace")

        # Lecture CSV : auto-détection du séparateur
        df = pd.read_csv(io.StringIO(text), sep=None, engine="python")

        # Résumé technique du fichier
        info = html.Div(
            [
                html.P(f"Fichier : {file_name or '(sans nom)'}"),
                html.P(f"Lignes : {len(df)}"),
                html.P(f"Colonnes : {', '.join(map(str, df.columns))}"),
            ]
        )

        # Table interactive (tri + filtre)
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
        # En cas d'erreur : message explicite + détail technique
        error_message = html.Div(
            [
                html.P(f"Fichier : {file_name or '(sans nom)'}"),
                html.P("Erreur de lecture du CSV."),
                html.Pre(str(exc)),
            ],
            style={"color": "#d32f2f"},
        )
        return error_message, None

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## Modifier et sauvegarder la table

## Code 5

```python
from __future__ import annotations
import pandas as pd
from dash import Dash, Input, Output, State, dash_table, html

# Chemins des fichiers (lecture / écriture)
FILE_TO_READ = "data/ventes.csv"
FILE_TO_SAVE = "data/ventes_modifiees.csv"

# Lecture des données CSV dans un DataFrame Pandas
df: pd.DataFrame = pd.read_csv(FILE_TO_READ)

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Création du layout
app.layout = html.Div(
    [
        # Titre principal
        html.H1("Modifier et sauvegarder les données d'une table"),

        # Table interactive et éditable
        html.Div(
            dash_table.DataTable(
                id="sales_table",
                data=df.to_dict("records"),
                columns=[
                    # Chaque colonne est éditable
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

        # Bouton de sauvegarde
        html.Button(
            "Sauvegarder",
            id="btn_save",
            style={"margin": "20px 20px"},
        ),

        # Zone de message (résultat de la sauvegarde)
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
    Sauvegarde la totalité des données actuelles de la DataTable dans un
    fichier CSV.

    Args :
    	n_clicks (int) : nombre de clics sur le bouton "Sauvegarder".
    	sales (list[dict] | None) : données de la table (liste de lignes sous
    	    forme de dictionnaires).

    Returns:
    	str : message de confirmation (ou chaîne vide si aucun clic).
    """
    # Pas de clic → on ne fait rien
    if not n_clicks:
        return ""

    # Sécurité : si la table est vide ou non disponible
    if not sales:
        return "Aucune donnée à sauvegarder."

    # Conversion en DataFrame puis écriture CSV
    df_modified = pd.DataFrame(sales)
    df_modified.to_csv(FILE_TO_SAVE, index=False)

    return f"{len(df_modified)} lignes sauvegardées dans '{FILE_TO_SAVE}'."

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## Tester l'écriture et le contenu d'un fichier

## Code 6

```python
from __future__ import annotations
import pandas as pd
from pathlib import Path

def test_read_write_csv(tmp_path: Path) -> None:
    """
    Vérifie que l'écriture puis la lecture d'un fichier CSV fonctionnent.

    Le test :
    - crée un fichier CSV temporaire,
    - écrit un DataFrame Pandas dedans,
    - relit ce fichier,
    - compare les données obtenues.
    """
    # Création d'un chemin vers un fichier CSV temporaire
    fichier: Path = tmp_path / "test.csv"

    # DataFrame de départ
    df_init: pd.DataFrame = pd.DataFrame(
        {
            "A": [1, 2, 3],
            "B": [4, 5, 6],
        }
    )

    # Écriture du DataFrame dans le fichier CSV
    df_init.to_csv(fichier, index=False)

    # Lecture du fichier CSV dans un nouveau DataFrame
    df_charge: pd.DataFrame = pd.read_csv(fichier)

    # Vérifie que les données lues sont identiques aux données écrites
    assert df_charge.equals(df_init)
```

---

# Chapitre 7 - Persistance et stockage local

## Le composant dcc.Store

## Code 1

```python
from __future__ import annotations
from dash import Dash, Input, Output, State, dcc, html

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Création du layout
app.layout = html.Div(
    [
        # Titre principal
        html.H2("Démo - Mémorisation via dcc.Store"),

        # Stockage côté navigateur
        dcc.Store(
            id="memory",
            storage_type="local",
        ),

        # Champ de saisie
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

        # Bouton de sauvegarde
        html.Div(
            html.Button(
                "Enregistrer",
                id="btn_save",
                style={"margin": "5px"},
            )
        ),

        # Zone d'affichage du message
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
    Sauvegarde la valeur du champ de saisie dans le stockage local.

    Args:
    	_n_clicks (int) : nombre de clics sur le bouton "Enregistrer".
    	field (str | None) : valeur saisie par l'utilisateur.

    Returns:
    	dict : dictionnaire à stocker dans dcc.Store.
    """
    return {"text": field}

# Callback
@app.callback(
    Output("message", "children"),
    Input("memory", "data"),
)
def display_message(data: dict | None) -> str:
    """
    Affiche la valeur restaurée depuis le stockage local.

    Args:
    	data (dict | None) : Données lues depuis dcc.Store.

    Returns:
    	str : Message affiché.
    """
    if data is None:
        return "Aucune donnée sauvegardée."

    print(data)

    return f"Valeur restaurée : {data["text"]}"

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## Code 3

```python
from __future__ import annotations
from dash import Dash, html, dcc, Input, Output

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Création du layout
app.layout = html.Div(
    [
        # Titre principal
        html.H2("Démo - Mémorisation automatique"),

        # Champ de saisie texte :
        # - persistence=True : Dash mémorise automatiquement la valeur saisie
        # - persistence_type="local" : la valeur est stockée dans le navigateur
        #   et restaurée même après fermeture / réouverture
        dcc.Input(
            id="first_name",
            type="text",
            placeholder="Votre prénom",
            persistence=True,
            persistence_type="local",
            style={"textAlign": "center"},
        ),

        # Zone d'affichage du message sous le champ de saisie
        html.Div(
            id="message",
            style={"marginTop": "20px"},
        ),
    ],
    # Style global du conteneur
    style={"textAlign": "center"},
)

# Callback
@app.callback(
    Output("message", "children"),
    Input("first_name", "value"),
)
def update_message(first_name: str | None) -> str:
    """
    Met à jour le message de salutation en fonction du prénom saisi.

    Ce callback est automatiquement déclenché à chaque modification
    de la valeur du champ de saisie.

    Args:
    	first_name (str | None) : valeur saisie dans le champ d'entrée.

    Returns:
    	str : message affiché dans la zone de texte
    """
    # Aucun texte saisi
    if not first_name:
        return "Aucun prénom saisi"

    # Texte présent : affichage du message personnalisé
    return f"Bonjour {first_name} !"

# Lancement de l'application en mode développement
if __name__ == "__main__":
    app.run(debug=True)
```

---

## Les cas d'usage

## Code 4

```python
from __future__ import annotations
from dash import Dash, Input, Output, dcc, html

# =========================
# Style inline (constante)
# =========================
ZONE_BASE_STYLE = {
    "padding": "5px",
    "margin": "10px 50px",
}

# ================
# Application Dash
# ================
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Création du layout
app.layout = html.Div(
    [
        # Stockage local des préférences utilisateur
        dcc.Store(
            id="prefs",
            storage_type="local",
        ),

        # Titre
        html.H2("Choisissez une couleur de fond"),

        # Liste déroulante de couleurs
        html.Div(
            dcc.Dropdown(
                id="colors_list",
                options=[
                    {"label": "Blanc", "value": "white"},
                    {"label": "Bleu clair", "value": "#e3f2fd"},
                    {"label": "Gris", "value": "#eeeeee"},
                ],
                value="white",
                clearable=False,
            ),
            style={"margin": "10px 50px"},
        ),

        # Zone dont la couleur de fond change
        html.Div(
            id="message",
            children="Bonjour !",
            style=ZONE_BASE_STYLE,
        ),
    ],
    style={"textAlign": "center"},
)

# ==========
# Callback 1
# ==========
@app.callback(
    Output("prefs", "data"),
    Input("colors_list", "value"),
    prevent_initial_call=True,
)
def save_color(color: str) -> dict:
    """
    Sauvegarde dans le stockage local la couleur sélectionnée.

    Args:
    	color (str) : couleur choisie dans la liste déroulante.

    Returns:
    	dict : dictionnaire contenant la couleur de fond.
    """
    return {"background": color}

# ==========
# Callback 2
# ==========
@app.callback(
    Output("message", "style"),
    Input("prefs", "data"),
)
def apply_color(pref: dict | None) -> dict:
    """
    Applique la couleur de fond sauvegardée à la zone d'affichage.

    Args:
    	pref (dict | None) : préférence récupérée depuis dcc.Store.

    Returns:
    	dict : style appliqué à la zone de message.
    """
    background = (pref or {}).get("background")

    # Aucune préférence → style par défaut
    if not background:
        return ZONE_BASE_STYLE

    # Fusion du style de base avec la couleur choisie
    return {
        **ZONE_BASE_STYLE,
        "backgroundColor": background,
    }

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## La limite fonctionnelle et un palliatif

## Code 5

```python
from __future__ import annotations
import json
import os
import tempfile
from pathlib import Path
from dash import Dash, Input, Output, State, dcc, html
from dash.exceptions import PreventUpdate

# =========================
# Constante : fichier local
# =========================
NOTES_FILE: Path = Path("data/notes.json")

# =====================
# Fonctions utilitaires
# =====================
def load_notes_json() -> str:
    """
    Charge le contenu du bloc-notes depuis un fichier JSON local.

    Le fichier attendu contient un objet JSON avec une clé "content".
    Si le fichier n'existe pas ou si le JSON est invalide, on retourne
    une chaîne vide.

    Returns:
    	str : texte chargé depuis le fichier, ou chaîne vide si indisponible.
    """
    if not NOTES_FILE.exists():
        return ""

    try:
        content = NOTES_FILE.read_text(encoding="utf-8")
        json_data = json.loads(content)
        return str(json_data.get("content", ""))
    except (OSError, json.JSONDecodeError, TypeError, ValueError):
        # Fichier inaccessible ou JSON invalide → contenu vierge
        return ""

def save_to_file(path: Path, text: str) -> None:
    """
    Écrit un fichier de manière atomique (robuste).

    Principe :
    - écrire dans un fichier temporaire,
    - forcer l'écriture sur disque (flush + fsync),
    - remplacer le fichier final d'un seul coup (os.replace).

    Args:
    	path (Path) : chemin du fichier final à écrire.
    	text (str) : contenu à écrire dans le fichier.
    """
    path.parent.mkdir(parents=True, exist_ok=True)

    fd, tmp_path = tempfile.mkstemp(prefix=path.name, dir=str(path.parent))
    os.close(fd)  # on rouvrira via le chemin, plus simple et portable

    try:
        with open(tmp_path, "w", encoding="utf-8") as f:
            f.write(text)
            f.flush()
            os.fsync(f.fileno())

        # Remplacement atomique si le fichier cible n'est pas ouvert ailleurs
        os.replace(tmp_path, path)
    finally:
        # Nettoyage si quelque chose a échoué
        try:
            if os.path.exists(tmp_path):
                os.remove(tmp_path)
        except OSError:
            pass

def save_notes_json(text_area_content: str) -> None:
    """
    Sérialise et sauvegarde le texte du bloc-notes dans un fichier JSON local.

    Args:
    	text_area_content (str) : texte à sauvegarder.
    """
    payload = json.dumps(
        {"content": text_area_content},
        ensure_ascii=False,
        indent=2,
    )
    save_to_file(NOTES_FILE, payload)

# ================
# Application Dash
# ================
app: Dash = Dash(__name__)
app.title = "Démo Dash"

app.layout = html.Div(
    [
        # Titre de la page
        html.H1("Bloc-notes Dash local"),

        # Zone de saisie (restaurée au chargement via load_notes_json)
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
            # conserve la saisie dans la session du navigateur
            persistence=True,
            persistence_type="session",
        ),

        # Bouton de sauvegarde
        html.Div(
            html.Button(
                "Sauvegarder",
                id="btn_save",
                style={"marginTop": "10px"},
            )
        ),

        # Zone d'affichage du message de sauvegarde
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
    Sauvegarde le texte saisi dans le fichier local lorsqu'on clique sur
    le bouton.

    Args:
    	_n_clicks (int) : nombre de clics sur le bouton "btn_save".
    	content (str | None ) : contenu actuel de la zone de texte.

    Returns:
    	str : message affiché à l'utilisateur (succès ou erreur).

    Raises:
    	PreventUpdate : empêche toute mise à jour si le contenu est
    	    indisponible.
    """
    if content is None:
        raise PreventUpdate

    try:
        save_notes_json(content)
        return "Notes sauvegardées localement."
    except OSError as exc:
        return f"Erreur de sauvegarde: {exc.strerror or str(exc)}"

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## Vérification et tests dans PyCharm

## Code 6

```python
from __future__ import annotations
import json
from pathlib import Path
from app_notepad_2 import load_notes_json

def test_save_and_reload(tmp_path):
    """
    Vérifie que le contenu d'un fichier JSON peut être écrit
    puis relu correctement depuis le disque.

    Le fixture pytest `tmp_path` fournit un dossier temporaire
    isolé, automatiquement nettoyé après le test.
    """
    # Chemin vers un fichier temporaire
    file: Path = tmp_path / "notes.json"

    # Contenu de test
    content: str = "Test de persistance Dash"

    # Écriture du fichier JSON simulant une sauvegarde
    file.write_text(
        json.dumps({"content": content}),
        encoding="utf-8",
    )

    # Lecture et décodage du fichier
    data: dict = json.loads(file.read_text(encoding="utf-8"))

    # Vérification du contenu
    assert data["content"] == content

def test_load_by_default(monkeypatch):
    """
    Vérifie que la fonction `load_notes_json` retourne une chaîne
    vide lorsque le fichier de notes n'existe pas.

    Le fixture pytest `monkeypatch` permet de modifier temporairement
    une variable globale du module testé.
    """
    # Remplace la constante NOTES_FILE par un chemin inexistant
    monkeypatch.setattr(
        "app_notepad_2.NOTES_FILE",
        Path("inexistant.json"),
    )

    # La fonction doit retourner une chaîne vide
    assert load_notes_json() == ""
```

---

# Chapitre 8 - Thèmes et mise en forme adaptative

## Le dossier assets

### Les feuilles de styles CSS

## Code 1

```python
from __future__ import annotations
from dash import Dash, html, dcc

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Création du layout
app.layout = html.Div(
    [
        # Titre principal)
        html.H1("Titre principal"),

        # Champ de saisie texte
        dcc.Input(
            type="text",
            placeholder="Votre nom",
            style={"textAlign": "center"}
        ),

        # Sauts de ligne (équivalent <br> en HTML)
        html.Br(),
        html.Br(),

        # Liste déroulante d'options
        dcc.Dropdown(
            options=[
                {"label": "Option A", "value": "A"},
                {"label": "Option B", "value": "B"},
            ],
            value="A"
        ),

        # Autres sauts de ligne pour l'espacement
        html.Br(),
        html.Br(),

        # Bouton simple (aucun callback associé ici)
        html.Button("OK"),

        # Pied de page
        html.Footer("Petite démo CSS"),
    ]
)

# Point d'entrée de l'application
if __name__ == "__main__":
    app.run(debug=True)
```

---

## Code 2

```css
/* fichier layout_1.css */ 
body {
  background:#FFE0B2;
  font-family: "Segoe UI", Arial, sans-serif;
  text-align: center; /* centre le texte */
}
```

---

## Code 3

```css
/* fichier layout_2.css */ 
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

### Les images

## Code 4

```python
from __future__ import annotations
from dash import Dash, html, dcc

# Création de l'application Dash
app: Dash = Dash(__name__)
app.title = "Démo Dash"

# Création du layout
app.layout = html.Div(
    [
        # Image du logo (chargée depuis le dossier assets/)
        html.Img(
            src="/assets/logo.png",  # chemin relatif vers assets/
            alt="Logo",
            style={
                "height": "80px",
                "marginBottom": "20px",
            },
        ),

        # Titre principal de la page
        html.H1("Titre principal"),

        # Champ de saisie texte
        dcc.Input(
            type="text",
            placeholder="Votre nom",
            style={"textAlign": "center"}
        ),

        # Sauts de ligne pour l'espacement vertical
        html.Br(),
        html.Br(),

        # Liste déroulante d'options
        dcc.Dropdown(
            options=[
                {"label": "Option A", "value": "A"},
                     {"label": "Option B", "value": "B"}
            ],
            value="A",
        ),

        # Autres sauts de ligne pour l'espacement
        html.Br(),
        html.Br(),

        # Bouton simple (aucun callback associé ici)
        html.Button("OK"),

        # Pied de page
        html.Footer(
            [
                # Texte du footer
                html.Div("Petite démo CSS"),

                # Image décorative dans le footer
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

# Point d’entrée de l’application
if __name__ == "__main__":
    app.run(debug=True)
```

---

## Dash Bootstrap Components

### Construire une interface avec DBC

## Code 6

```python
from __future__ import annotations
import dash_bootstrap_components as dbc
import pandas as pd
import plotly.express as px
from dash import Dash, Input, Output, dcc, html

# -------------------------------------------------------------------
# Données locales (DataFrame simple pour la démonstration)
# -------------------------------------------------------------------
df = pd.DataFrame(
    {
        "Ville": ["Paris", "Lyon", "Marseille"],
        "Ventes": [120, 80, 60],
    }
)

# -------------------------------------------------------------------
# Création de l'application Dash avec le thème Bootstrap FLATLY
# -------------------------------------------------------------------
app: Dash = Dash(
    __name__,
    external_stylesheets=[dbc.themes.FLATLY],
)
app.title = "Démo Dash"

# -------------------------------------------------------------------
# Création du layout
# -------------------------------------------------------------------
app.layout = dbc.Container(
    [
        # -------------------------------------------------------------
        # Barre de navigation (Navbar)
        # -------------------------------------------------------------
        dbc.NavbarSimple(
            brand="Tableau de bord local",  # texte à gauche
            color="primary",                # couleur Bootstrap
            dark=True,                      # texte clair
            class_name="mb-4",              # marge basse (Bootstrap)
        ),

        # -------------------------------------------------------------
        # Ligne Bootstrap contenant 2 colonnes
        # -------------------------------------------------------------
        dbc.Row(
            [
                # -----------------------------------------------------
                # Colonne gauche : sélection de la ville
                # -----------------------------------------------------
                dbc.Col(
                    [
                        html.H3(
                            "Sélection de la ville",
                            className="mb-2",
                        ),

                        # Menu déroulant Dash
                        dcc.Dropdown(
                            id="towns_list",
                            options=[
                                {"label": v, "value": v}
                                for v in df["Ville"].tolist()
                            ],
                            value="Lyon",
                            clearable=False,
                            persistence=True,
                            persistence_type="session",
                            placeholder="Choisissez une ville",
                        ),
                    ],
                    width=4,  # 4 colonnes sur 12 (Bootstrap)
                ),

                # -----------------------------------------------------
                # Colonne droite : graphique
                # -----------------------------------------------------
                dbc.Col(
                    [
                        dcc.Graph(id="graphic"),
                    ],
                    width=8,  # 8 colonnes sur 12 (Bootstrap)
                ),
            ]
        ),
    ],
    fluid=True,  # largeur fluide (Boostrap)
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
    Met à jour le graphique des ventes en fonction de la ville choisie.

    Args:
        town (str): ville sélectionnée dans la liste déroulante.

    Returns:
        plotly.graph_objects.Figure: graphique à afficher.
    """
    if not town:
        # Cas par défaut : toutes les villes
        fig = px.bar(
            df,
            x="Ville",
            y="Ventes",
            title="Ventes (toutes villes)",
        )
    else:
        # Cas avec une ville sélectionnée
        dff = df[df["Ville"] == town]
        fig = px.bar(
            dff,
            x="Ville",
            y="Ventes",
            title=f"Ventes à {town}",
        )

        # Ajustements visuels du graphique
        fig.update_layout(
            margin=dict(l=80, r=20, t=60, b=40),
            yaxis_title="Ventes",
            height=420,
        )
        fig.update_yaxes(
            automargin=True,
            ticks="outside",
            tickfont=dict(size=12),
        )

    return fig

# -------------------------------------------------------------------
# Lancement de l'application
# -------------------------------------------------------------------
if __name__ == "__main__":
    app.run(debug=True)
```

---

### Adapter la mise en page

## Code 7

```python
import dash_bootstrap_components as dbc
from dash import html

# Ligne Bootstrap (Row) : une Row représente une ligne horizontale découpée
# en colonnes
dbc.Row(
    [
        # -------------------------------------------------------------
        # Colonne gauche : width=4 signifie : 4/12 de la largeur totale
        # -------------------------------------------------------------
        dbc.Col(
            html.Div("Colonne gauche"),
            width=4,
        ),
        # -------------------------------------------------------------
        # Colonne droite : width=8 signifie : 8/12 de la largeur totale
        # -------------------------------------------------------------
        dbc.Col(
            html.Div("Colonne droite"),
            width=8,
        ),
    ]
)
```

---

### 3.6. Un exemple complet : mini-dashboard thématique

## Code 9

```python
from __future__ import annotations
import dash_bootstrap_components as dbc
import plotly.express as px
from dash import Dash, Input, Output, dcc, html

# Données : jeu de données Gapminder filtré sur l'année 2007
DF = px.data.gapminder().query("year == 2007")

# Création de l'application Dash avec le thème CERULEAN (Bootstrap)
app: Dash = Dash(__name__, external_stylesheets=[dbc.themes.CERULEAN])
app.title = "Démo Dash"

# Création du layout
app.layout = dbc.Container(
    [
        # Barre de navigation (Navbar)
        dbc.Navbar(
            dbc.Container(
                dbc.NavbarBrand(
                    "Indicateurs mondiaux",
                    className="mx-auto",  # classe Bootstrap : centre le texte
                )
            ),
            color="info",          # couleur Bootstrap
            dark=True,             # texte clair (sur fond coloré)
            class_name="mb-3",     # marge basse (Bootstrap)
        ),

        # Ligne principale : menu (gauche) + graphique (droite)
        dbc.Row(
            [
                # Colonne gauche (3/12) : sélection du continent
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
                            placeholder="Choisissez un continent",
                        ),
                    ],
                    width=3,
                ),

                # Colonne droite (9/12) : zone du graphique
                dbc.Col(
                    [
                        dcc.Graph(id="graphic"),
                    ],
                    width=9,
                ),
            ]
        ),

        # Pied de page
        html.Footer(
            "Application locale Dash 3.2.0 — Thème CERULEAN",
            className="mt-3",  # marge haute (Bootstrap)
            style={"textAlign": "center"},
        ),
    ],
    fluid=True,  # largeur fluide (Boostrap)
)

# Callback
@app.callback(
    Output("graphic", "figure"),
    Input("continents_list", "value"),
)
def update_graphic(continent: str) -> "px.Figure":
    """
    Met à jour le graphique en fonction du continent sélectionné.

    Args:
    	continent (str) : continent choisi dans la liste déroulante.

    Returns:
    	plotly.graph_objs.Figure : graphique Plotly (scatter) à afficher.
    """
    # Filtrage des données sur le continent choisi
    dff = DF[DF["continent"] == continent]

    # Scatter : PIB/habitant vs espérance de vie, taille=population
    figure = px.scatter(
        dff,
        x="gdpPercap",
        y="lifeExp",
        size="pop",
        color="country",
        log_x=True,
        title=f"Espérance de vie en {continent} (2007)",
    )

    # Ajustement visuel simple
    figure.update_layout(
        xaxis_title="PIB par habitant",
        yaxis_title="Espérance de vie",
        legend_title_text="Pays",
        height=420
    )
    return figure

if __name__ == "__main__":
    # Lancement du serveur de développement
    app.run(debug=True)
```

---

## Vérification et tests dans PyCharm

## Code 10

```python
from __future__ import annotations
from dash.testing.application_runners import import_app

def test_theme_loading(dash_duo) -> None:
    """
    Vérifie que le thème Bootstrap (et donc la mise en forme) est chargé.

    Le test :
    - importe l'application Dash depuis le module Python,
    - démarre le serveur de test,
    - vérifie qu'un élément de Navbar Bootstrap existe,
    - vérifie que le titre "Continent" est bien présent à l'écran.
    """
    # Importation de l'application Dash à tester (nom du module sans .py)
    app = import_app("explore_gapminder_bootstrap")

    # Démarrage du serveur Dash de test
    dash_duo.start_server(app)

    # La Navbar Bootstrap a généralement la classe CSS ".navbar"
    navbar = dash_duo.find_element(".navbar")
    assert navbar is not None

    # Vérifie que le texte du titre contient "Continent"
    titre = dash_duo.find_element("h3")
    assert "Continent" in titre.text
```

---

# Annexe - Installer Firefox

## Avec macOS

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

## Paramètres de pytest

## Code 6

```python
[pytest]
pythonpath = src
addopts = --webdriver Firefox --headless
filterwarnings = default
```

---

# Annexe - Un bloc-notes minimaliste avec SQLite

## Les imports

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

## Les fonctions d'accès aux données, de persistance et de pluralisation

## Code 2

```python
# ---------------------------------------------------------------------------
# SQLite helpers
# ---------------------------------------------------------------------------
DB_PATH = Path("data/demo.sqlite")

def get_con() -> sqlite3.Connection:
    """
    Crée et retourne une connexion SQLite configurée pour l'application.

    Cette application lit et écrit dans un fichier SQLite local. Une connexion
    SQLite “brute” fonctionne déjà, mais quelques réglages améliorent la
    stabilité et le confort de développement :
    - 'timeout=5' : SQLite verrouille le fichier pendant certaines écritures.
    	Un timeout laisse quelques secondes au moteur pour terminer l'opération
    	avant de lever une erreur "database is locked".
    - 'row_factory = sqlite3.Row' : permet d'accéder aux colonnes par nom
    	(ex: row["title"]) au lieu d'un accès uniquement par index, ce qui
    	simplifie le mapping vers des dictionnaires.
    - 'PRAGMA journal_mode=WAL' : WAL (Write-Ahead Logging) améliore la
    	concurrence lecture/écriture dans les scénarios où l'on lit souvent et
    	écrit ponctuellement. Ce mode est particulièrement utile avec une
    	interface interactive.

    Returns:
        sqlite3.Connection: connexion SQLite prête à l'emploi. Elle devra être
        utilisée via un context manager ('with get_con() as con:') afin de
        garantir commit ou rollback et fermeture propre.
    """
    con = sqlite3.connect(DB_PATH, timeout=5)
    con.row_factory = sqlite3.Row
    con.execute("PRAGMA journal_mode=WAL;")
    return con
  
 def init_db() -> None:
    """
    Initialise la base SQLite si nécessaire.

    Cette fonction prépare le stockage local de l'application :

    1) crée le dossier parent du fichier 'DB_PATH' si besoin,
    2) crée la table 'notes' si elle n'existe pas.

    Schéma de la table 'notes' :
    - id:
        INTEGER PRIMARY KEY AUTOINCREMENT
        identifiant technique unique.
    - title:
        TEXT NOT NULL + CHECK
        le title ne peut pas être vide.
    - note:
        TEXT NOT NULL + CHECK
        la note ne peut pas être vide et sa taille est bornée à 2000 caractères.
    - created_at:
        TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        date automatique générée par SQLite à l'insertion.

    Notes:
    - les contraintes CHECK protègent la base même si un bug côté interface
    	laisse passer des valeurs invalides ;
    - la création est idempotente : on peut appeler init_db() à chaque
    	démarrage.
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
    Insère une note dans la base.

    Cette fonction effectue l'écriture dans la table 'notes' :
    - normalisation via 'strip()',
    - exécution d'une requête paramétrée pour éviter l'injection SQL directe.

    Args:
        title: titre de la note, doit être non vide après normalisation.
        note: contenu de la note, doit être non vide après normalisation et ne pas
        	dépasser 2000 caractères.

    Raises:
        sqlite3.Error: en cas de problème SQLite (fichier inaccessible, verrouillage,
            erreur de contrainte CHECK, etc.).
    """
    with get_con() as con:
        con.execute(
            "INSERT INTO notes(title, note) VALUES(?, ?)",
            (title.strip(), note.strip()),
        )

def update_row(row_id: int, title: str, note: str) -> None:
    """
    Met à jour une note existante.

    La mise à jour est basée sur l'identifiant technique 'id'.
    Les valeurs sont normalisées avec 'strip()' avant écriture.

    Args:
        row_id: identifiant de la note à mettre à jour.
        title: nouveau titre.
        note: nouveau contenu.

    Raises:
        sqlite3.Error: erreur SQLite lors de la requête (contrainte, verrouillage,
        	etc.).
    """
    with get_con() as con:
        con.execute(
            "UPDATE notes SET title = ?, note = ? WHERE id = ?",
            (title.strip(), note.strip(), row_id),
        )

def delete_row(row_id: int) -> None:
    """
    Supprime une note par son identifiant.

    Args:
        row_id: identifiant de la note à supprimer.

    Raises:
        sqlite3.Error: erreur SQLite lors de la requête.
    """
    with get_con() as con:
        con.execute("DELETE FROM notes WHERE id = ?", (row_id,))

def fetch_rows() -> list[dict[str, Any]]:
    """
    Récupère l'ensemble des notes.

    La fonction lit toutes les enregistrements de la table et renvoie une liste de
    dictionnaires, directement compatible avec 'dash_table.DataTable(data=...)'.

    Le tri décroissant ('ORDER BY id DESC') met les notes les plus récentes en tête.

    Returns:
        list[dict[str, Any]]: liste des enregistrements sous forme de dictionnaires.
        Clés attendues :
            - "id"
            - "title"
            - "note"
            - "created_at"
    """
    with get_con() as con:
        rows = con.execute(
            "SELECT id, title, note, created_at FROM notes ORDER BY id DESC"
        ).fetchall()
    return [dict(r) for r in rows]

def pluralization(nb_records: int) -> str:
    """
    Retourne un message utilisateur accordé en français.

    Cette fonction évite de répéter une logique “if/else” dans les callbacks.

    Args:
        nb_records: nombre d'enregistrements pris en compte.

    Returns:
        str: le message accordé.
    """
    if nb_records <= 1:
        return "Modification enregistrée."
    return "Modifications enregistrées."
```

---

## L'initialisation de l’application et la création du layout

## Code 3

```python
# ---------------------------------------------------------------------------
# Application Dash
# ---------------------------------------------------------------------------
init_db()

app = Dash(__name__)
app.title = "Dash + SQLite"

# Styles factorisés
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
        # Titre
        # --------------------------------------------------------------
        html.H1(
            "Démo Dash + SQLite (CRUD)",
            style={"margin": "0 0 16px 0"},
        ),

        # Formulaire
        # --------------------------------------------------------------
        html.Div(
            [
                # --- Titre (Input) ---
                html.Div(
                    [
                        html.Label(
                            "Titre",
                            htmlFor="input_title",
                            style=LABEL_STYLE,
                        ),
                        dcc.Input(
                            id="input_title",
                            type="text",
                            placeholder="Titre",
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
                            placeholder="Votre note…",
                            style={
                                **FIELD_STYLE,
                                "height": "110px",
                                "resize": "both",
                            },
                        ),
                    ],
                    style=FIELD_CONTAINER_STYLE,
                ),

                # --- Bouton Ajouter ---
                html.Div(
                    html.Button(
                        "Ajouter",
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
                            "Enregistrer modifications",
                            id="btn_save",
                            n_clicks=0,
                            style=BTN_BASE_STYLE,
                        ),
                        html.Button(
                            "Supprimer sélection",
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

        # Store + intervalle
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
                            "name": "créé le",
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

## La gestion des interactions et la logique applicative via les callbacks

## Code 4

```python
# ---------------------------------------------------------------------------
# Callbacks
# ---------------------------------------------------------------------------

# Callback "Ajouter une note"
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
    Ajoute une note à partir du contenu des champs de saisis.

    Déclencheur:
        clic du bouton "Ajouter" ('btn_add.n_clicks').   		

    Rôles :
        - normaliser les champs (None -> "", suppression des espaces) ;
        - valider :
      	    - title non vide,
            - note non vide et <= 2000 caractères ;
        - insérer dans la table 'notes' via la fonction 'insert_row' ;
        - en cas de succès :
      	    - afficher un message,
      	    - vider les champs,
            - incrémenter la version des données ('notes_version') pour
        	  recharger la table ;
        - en cas d'erreur :
      	    - afficher un message,
            - ne pas écraser la saisie (no_update).

    Args:
    	_n: nombre de clics du bouton "Ajouter" ('btn_add.n_clicks').
            -> La valeur n'est pas utilisée, seul l'évènement compte.
      title:  valeur du champ 'title' (peut être None au démarrage).
      note: valeur de la note (peut être None).
      ver: version actuelle des données stockée dans 'dcc.Store'.

    Returns:
        tuple:
            str : message à afficher
            str ou no_update : input_title.value -> "" en cas de succès
            str ou no_update : input_note.value -> "" en cas de succès
            int ou no_update : ver (version actuelle des données) + 1 en cas de succès
    """
    title = (title or "").strip()
    contenu = (note or "").strip()

    if not title or not contenu:
        return "Veuillez saisir un titre et une note.", no_update, no_update, no_update
    if len(contenu) > 2000:
        return "Note trop longue (max 2000 caractères).", no_update, no_update, no_update

    try:
        insert_row(title, contenu)
        return "✔ Note ajoutée.", "", "", ver + 1
    except sqlite3.Error as exc:
        return f"Erreur SQLite: {exc}", no_update, no_update, no_update

# Callback "Charger la table"
@app.callback(
    Output("tbl", "data"),
    Input("boot", "n_intervals"),
    Input("notes_version", "data"),
)
def load_table(_boot: int, _ver: int) -> list[dict[str, Any]]:
    """
    Charge (ou recharge) la DataTable depuis SQLite.

    Déclencheurs:
        - 'boot.n_intervals' : une fois au démarrage,
        - 'notes_version.data' : après chaque écriture (insert/update/delete).

    Rôles:
        - lire toutes les notes dans la table 'notes' via la fonction 'fetch_rows',
        - retourner une liste de dictionnaires compatible 'DataTable.data'.

    Args:
    	_boot: intervalles du composant 'dcc.Interval'.
               -> La valeur n'est pas utilisée, seul l'évènement compte.
          _ver: version des données
                -> La valeur n'est pas utilisée, seul l'évènement compte.

    Returns:
    	list[dict[str, Any]]: lignes à afficher dans la DataTable.
    """
    return fetch_rows()

# Callback "Enregistrer la table"
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
    Enregistrer toutes les lignes de la DataTable dans la table 'notes'.

    Déclencheur:
        clic du bouton "Enregistrer modifications" ('btn_save.n_clicks').

    Stratégie:
        un 'UPDATE' est exécuté pour chaque ligne de la DataTable. C'est simple,
        lisible, mais pas optimal si la table devient très grande.
        Dans une version “production”, il faudrait :
            - détecter seulement les cellules modifiées,
            - ou faire une sauvegarde ciblée par ligne.

    Validations:
        - title et note non vides (après strip),
        - note <= 2000 caractères.

    Args:
        _n: nombre de clics du bouton 'btn_save.n_clicks'.
            -> La valeur n'est pas utilisée, seul l'évènement compte.
        data: données courantes de la DataTable. Chaque dictionnaire doit définir
            les clés "id", "title" et "note".
        ver: version actuelle des données stockée dans 'dcc.Store'.

    Returns:
        tuple:
          str : message à afficher
          int ou no_update : ver (version actuelle des données) + 1 en cas de succès

    Raises:
        PreventUpdate: si 'data' est vide / None.
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
                return "Erreur: titre/note ne peuvent pas être vides.", no_update
            if len(note) > 2000:
                return f"Erreur: note trop longue (id={row_id}).", no_update

            update_row(row_id, title, note)
            updated += 1

        return pluralization(updated), ver + 1
    except (ValueError, KeyError, sqlite3.Error) as exc:
        return f"Erreur sauvegarde : {exc}", no_update

# Callback "Supprimer la note sélectionnée"
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
    Supprime la ligne sélectionnée.

    Déclencheur:
        clic du bouton "Supprimer sélection" ('btn_delete.n_clicks').

    Sélection:
        la DataTable est configurée en 'row_selectable="single"'. On s'attend
        donc à une liste 'selected_rows' de taille 0 ou 1.

    Rôles:
        - vérifier qu'une sélection existe,
        - récupérer l'id de la ligne sélectionnée via 'data[selected[0]]',
        - supprimer en base via la fonction 'delete_row',
        - incrémenter la version des données ('notes_version') pour recharger
        	la table.

    Args:
        _n: nombre de clics du bouton "Supprimer sélection" (btn_delete.n_clicks')
            -> La valeur n'est pas utilisée, seul l'évènement compte.
        data: données courantes de la table.
        selected: indice de la ligne sélectionnée.
        ver: version actuelle des données stockée dans 'dcc.Store'.

    Returns:
        tuple:
          str : message à afficher
          int ou no_update : ver (version actuelle des données) + 1 en cas de succès
    """
    if not data or not selected:
        return "Sélectionnez une ligne à supprimer.", no_update

    try:
        idx = selected[0]
        row_id = int(data[idx]["id"])
        delete_row(row_id)
        return f"✔ Ligne id={row_id} supprimée.", ver + 1
    except (IndexError, KeyError, ValueError, sqlite3.Error) as exc:
        return f"Erreur suppression: {exc}", no_update
```

---

## Le lancement de l'application

## Code 5

```python
if __name__ == "__main__":
    app.run(debug=True)
```
