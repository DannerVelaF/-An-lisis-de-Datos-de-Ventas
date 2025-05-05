# 📊 Análisis de Datos de Ventas

Este proyecto tiene como objetivo poner en práctica mis conocimientos en análisis de datos, visualización y herramientas de inteligencia de negocios. Utilizo Python y Power BI para explorar, analizar y presentar los datos de ventas de forma clara y visualmente informativa.

---

## 🔍 Objetivo

Desarrollar un análisis completo de un conjunto de datos de ventas, identificando patrones, comportamientos del cliente y oportunidades comerciales, mediante el uso de:

- Análisis de datos con Python (`pandas`, `matplotlib`, `seaborn`)
- Practicar el flujo completo de análisis de datos desde la carga hasta la visualización.
- Explorar y limpiar un conjunto de datos de ventas.
- Presentar los hallazgos mediante visualizaciones efectivas.

---

## 🧾 Dataset

- **Origen del dataset:** Se hizo uso de un dataset publico publicado en la plataforma kaggle, el cual me brindó información relevante acerca ordenes de venta entre los años 2020 - 2025 https://www.kaggle.com/datasets/shantanugarg274/sales-dataset
- **Formato:** CSV
- **Características clave:** 'Order ID', 'Amount', 'Profit', 'Quantity', 'Category', 'Sub-Category','PaymentMode', 'Order Date', 'CustomerName', 'State', 'City','Year-Month

---

## Tratamiento de datos en python

### 1. Importar librerías y carga de datos

``` 
# Manejo de datos
import pandas as pd

# Graficación
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px

# Estilo de seaborn
sns.set(style="whitegrid")

plt.rcParams['figure.figsize'] = (10, 6)

# Formato linear, no cientifico
plt.rcParams['axes.formatter.useoffset'] = False

# Carga de datos
sales_df = pd.read_csv('Sales Dataset.csv')
```

### 2. Revisión inicial
```
df.info()
df.describe()
df.head()
```

### 3. Limpieza de datos

```
# Casting de datos
# Convertimos las columnas categoricas a tipo 'category'
columnas = ['Category', 'Sub-Category', 'City', 'PaymentMode', 'State', ]

for cof in columnas:
    sales_df[cof] = sales_df[cof].astype('category')

# Convertimos la columna 'Order Date' y 'Year-Month' a tipo 'datetime'
sales_df['Order Date'] = pd.to_datetime(sales_df['Order Date'])
sales_df['Year-Month'] = pd.to_datetime(sales_df['Year-Month'], format='%Y-%m')
```

### 4. Agregación de columnas para fechas

```
# Creamos la columa "Year" y "Month" a partir de la columna "Order Date" (Para poder graficar)
sales_df['Year'] = sales_df['Order Date'].dt.year
sales_df['Month'] = sales_df['Order Date'].dt.month
```

## 🐍 Visualizaciones en Python

A continuación, se muestran algunas visualizaciones generadas con Python:

### 📌 Ventas por Categoría
```
# Grafico de ventas por categoria
plt.ticklabel_format(style='plain', axis='y')
ax = sns.barplot(x='Category', y='Amount', data=sales_df, estimator=sum, hue='Category', errorbar=None)

# Titulos y etiquetas
plt.title('Ventas por Categoria')
plt.xlabel('Categoria')
plt.ylabel('Cantidad de ventas')

for p in ax.patches:
    height = p.get_height()
    ax.annotate(f'${height:,.0f}',             # formato sin decimales y con separador de miles
                (p.get_x() + p.get_width() / 2, height),  # posición (x, y)
                ha='center', va='bottom',     # alineación horizontal y vertical
                fontsize=9, fontweight='bold')

# Mostrar el grafico
plt.show()
```
![image](https://github.com/user-attachments/assets/e9a3b4ec-8ade-4b13-8cf6-2b811e1cd03d)

### 📍Ventas por sub categorias
```
# Grafico de ventas por sub categoria
plt.figure(figsize=(12, 6))
ax = sns.barplot(x='Sub-Category', y='Amount', data=sales_df, estimator=sum, hue='Category', errorbar=None)

# Titulos y etiquetas
plt.title('Ventas por Sub-Categoria')
plt.xlabel('Sub-Categoria')
plt.ylabel('Ventas')
plt.xticks(rotation=45)
plt.legend(title='Categoria')

for p in ax.patches:
    height = p.get_height()
    if height > 0:  # Evita anotar barras invisibles si las hay
        ax.annotate(f'{height:,.0f}', 
                    (p.get_x() + p.get_width() / 2, height),
                    ha='center', va='bottom',
                    fontsize=8, fontweight='bold')

# Mostrar el grafico
plt.show()
```
![image](https://github.com/user-attachments/assets/ff01e1ae-c5ea-4ee4-adeb-99b5948f8ffe)

### 💳Método de pago que mas ganancias genera
```
# Grafico de metodo de pago
ax = sns.barplot(x='PaymentMode', y="Amount", data=sales_df, hue='PaymentMode', errorbar=None)

# Titulos y etiquetas
plt.title('Pago de ventas')
plt.xlabel('Metodo de pago')
plt.ylabel('Cantidad de ventas')

for p in ax.patches:
    height = p.get_height()
    if height > 0:  # Evita anotar barras invisibles si las hay
        ax.annotate(f'$ {height:,.0f}', 
                    (p.get_x() + p.get_width() / 2, height),
                    ha='center', va='bottom',
                    fontsize=8, fontweight='bold')


# Mostrar el grafico
plt.show()
```
![image](https://github.com/user-attachments/assets/f67f7af2-a6ee-42c1-8adf-1ee9b3b5f1b9)

### 📈 Tendencia de ventas anual
```
# Grafico de tendencia
plt.ticklabel_format(style='plain', axis='y')
sns.lineplot(x='Year-Month', y='Amount', data=sales_df, estimator=sum, errorbar=None)

# Titulos y etiquetas
plt.title('Ventas por año')
plt.xlabel('Año')
plt.ylabel('Ventas')

# Mostrar el grafico
plt.show()
```
![image](https://github.com/user-attachments/assets/db7ea861-edc3-48a2-a0a8-21b1bb8d74f0)

### 🏳️Ventas por estado (Estados unidos)
```
# Grafico de ventas por estado
sns.barplot(x='State', y='Amount', data=sales_df, estimator=sum, hue='Category', errorbar=None)

# Titulos y etiquetas
plt.title('Ventas por estado')
plt.xlabel('Estado')
plt.ylabel('Ventas')

# Mostrar el grafico
plt.show()
```
![image](https://github.com/user-attachments/assets/44f18930-1fd3-47cd-9039-dc1782a2621c)

### 🗺️Mapa de calor de ventas por estado
```
# Se usara la liberia plotly. Es necesario tener las abreviaciones de los Estados
# Abreviaciones de los estados de EE.UU.
us_state_abbrev = {
    'Alabama': 'AL', 'Alaska': 'AK', 'Arizona': 'AZ', 'Arkansas': 'AR',
    'California': 'CA', 'Colorado': 'CO', 'Connecticut': 'CT', 'Delaware': 'DE',
    'Florida': 'FL', 'Georgia': 'GA', 'Hawaii': 'HI', 'Idaho': 'ID',
    'Illinois': 'IL', 'Indiana': 'IN', 'Iowa': 'IA', 'Kansas': 'KS',
    'Kentucky': 'KY', 'Louisiana': 'LA', 'Maine': 'ME', 'Maryland': 'MD',
    'Massachusetts': 'MA', 'Michigan': 'MI', 'Minnesota': 'MN', 'Mississippi': 'MS',
    'Missouri': 'MO', 'Montana': 'MT', 'Nebraska': 'NE', 'Nevada': 'NV',
    'New Hampshire': 'NH', 'New Jersey': 'NJ', 'New Mexico': 'NM', 'New York': 'NY',
    'North Carolina': 'NC', 'North Dakota': 'ND', 'Ohio': 'OH', 'Oklahoma': 'OK',
    'Oregon': 'OR', 'Pennsylvania': 'PA', 'Rhode Island': 'RI', 'South Carolina': 'SC',
    'South Dakota': 'SD', 'Tennessee': 'TN', 'Texas': 'TX', 'Utah': 'UT',
    'Vermont': 'VT', 'Virginia': 'VA', 'Washington': 'WA', 'West Virginia': 'WV',
    'Wisconsin': 'WI', 'Wyoming': 'WY'
}

# Agrgamos la columna de abreviaciones de los estados al DataFrame
sales_df['State_abbrev'] = sales_df['State'].map(us_state_abbrev)

# Agrupar por abreviaciones
ventas_estado = sales_df.groupby("State_abbrev", observed=False)["Amount"].sum().reset_index() 

# Crear el mapa de calor
fig = px.choropleth(
    ventas_estado,
    locations="State_abbrev",
    locationmode="USA-states",  # Esto usa las abreviaciones oficiales (CA, TX, etc.)
    color="Amount",
    color_continuous_scale="Blues",
    scope="usa",
    labels={'Amount': 'Ventas'},
    title="Mapa de Calor de Ventas por Estado (EE.UU.)"
)

fig.show()
```
![image](https://github.com/user-attachments/assets/cc5427db-bc28-4d43-9feb-e77258514cf6)

### 🪙 Clientes que más compran
```
# Agrupamos por clientes y tramos los 5 primeros
top_clientes = sales_df.groupby("CustomerName", observed=False)["Amount"].sum().reset_index().sort_values(by="Amount", ascending=False).head(5)

# Creamos una gráfica de barras con el top 5 clientes
ax = sns.barplot(x='CustomerName', y='Amount', data=top_clientes, estimator=sum, errorbar=None, hue='CustomerName')

# Titulos y etiquetas
plt.title("Top 5 Clientes que mas compran")
plt.xlabel("Cliente")
plt.ylabel("Ventas")

for p in ax.patches:
    ax.annotate(f'$ {p.get_height():,.0f}', (p.get_x() + p.get_width() / 2., p.get_height()), ha='center', va='center', fontsize=10, color='black', xytext=(0, 5), textcoords='offset points')  

# Graficamos
plt.show()
```
![image](https://github.com/user-attachments/assets/cfe66d9d-2b7d-47f3-a38d-733e84e69706)

---

## 📊 Dashboard en Power BI

Se creó un dashboard en Power BI para representar los hallazagos presentados.
Enlace público: https://app.powerbi.com/view?r=eyJrIjoiNzUyNTViMzgtYzNlMi00OWU2LTkxYmEtN2UyMWQxOGQ4YjI4IiwidCI6ImM0YTY2YzM0LTJiYjctNDUxZi04YmUxLWIyYzI2YTQzMDE1OCIsImMiOjR9
📷 **Vista previa del dashboard:**

![image](https://github.com/user-attachments/assets/2b4b584c-1cf7-49cc-a1a9-b5c8c87984f3)

---

## 🧠 Conclusiones

- El categoria más vendida fue fue `Office Supplies` con un total de `$ 2089510` dólares.
- La mayor parte de las ventas se concentran en el estado `New York` con un total de `$ 1130048` dólares.
- Se observan picos de ventas fue en el año `2022` con un total de `$ 1459775` dólares.
- El método de pago más usado por los clientes fue `Debit card`, generando `$ 1395035` dólares.
---

## ⚙️ Tecnologías Utilizadas

- Python
- Pandas
- Matplotlib
- Seaborn
- Power BI

---

## ✍️ Autor

Proyecto realizado por [Danner Vela] como práctica de análisis de datos y visualización.
