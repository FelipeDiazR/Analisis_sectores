# Analisis_sectores




<details>
  <summary>Países</summary>
  <ul>
    <li><a href="https://colab.research.google.com/drive/1L9vBj8SMAbduPFreR_xdOKp9xos11O2n?usp=sharing" target="_blank" rel="noopener noreferrer">Acceder a países</a></li>
  </ul>
</details>

<details>
  <summary>Industrias</summary>
  <ul>
    <li><a href="https://colab.research.google.com/drive/1sAEIBC0aP1aRSu9KyAeySy_4c1aylNsw?usp=sharing" target="_blank" rel="noopener noreferrer">Acceder a industrias</a></li>
  </ul>
</details>


<details>
   <summary> Código fuente python - informe Industrias </summary>

 >  ### Código Industrias

  Aquí puedes agregar más información, listas, o incluso código.

  ```python
  # Código Industrias

  import pandas as pd
  import matplotlib.pyplot as plt
  import seaborn as sns
  from google.colab import files

  # Paso 1: Subir el archivo desde la computadora
  uploaded = files.upload()

  # Paso 2: Cargar el archivo Excel en un DataFrame
  file_path = list(uploaded.keys())[0]  # Obtiene el nombre del archivo subido
  df = pd.read_excel(file_path)

  # Paso 3: Configuración de estilo para las visualizaciones
  sns.set(style="whitegrid")

  # Gráfico de las principales inversiones por empresa (Top 10)
  top_empresas = df.groupby('Name')['Market Value(USD)'].sum().sort_values(ascending=False).head(10).reset_index()

  plt.figure(figsize=(10, 6))
  sns.barplot(x='Market Value(USD)', y='Name', data=top_empresas, palette='viridis')
  plt.title('Top 10 Empresas por Valor de Mercado (USD)')
  plt.xlabel('Valor de Mercado (USD)')
  plt.ylabel('Empresa')
  plt.show()

  # Gráfico de las principales inversiones por país (Top 10)
  top_paises = df.groupby('Country')['Market Value(USD)'].sum().sort_values(ascending=False).head(10).reset_index()

  plt.figure(figsize=(10, 6))
  sns.barplot(x='Market Value(USD)', y='Country', data=top_paises, palette='magma')
  plt.title('Top 10 Países por Valor de Mercado (USD)')
  plt.xlabel('Valor de Mercado (USD)')
  plt.ylabel('País')
  plt.show()

  # Gráfico 3: Distribución del Valor de Mercado por Industria
  plt.figure(figsize=(12, 8))
  sns.boxplot(x='Market Value(USD)', y='Industry', data=df, palette='coolwarm')
  plt.title('Distribución del Valor de Mercado por Industria')
  plt.xlabel('Valor de Mercado (USD)')
  plt.ylabel('Industria')
  plt.show()

  # Gráfico 4: Relación entre Propiedad (Ownership) y Valor de Mercado
  plt.figure(figsize=(10, 6))
  sns.scatterplot(x='Ownership', y='Market Value(USD)', hue='Industry', data=df, palette='deep')
  plt.title('Relación entre Propiedad y Valor de Mercado')
  plt.xlabel('Porcentaje de Propiedad')
  plt.ylabel('Valor de Mercado (USD)')
  plt.legend(title='Industria')
  plt.show()
````
<details>
   <summary> Código fuente python - informe Países </summary>

 >  ### Código Países

  

  ```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Cargar el archivo Excel
file_path = 'EQ_2024_06_30_Country.xlsx'  # Make sure the file is in the same directory as the notebook OR
# provide the full absolute path to the file. 
xls = pd.ExcelFile(file_path)
data = pd.read_excel(xls, sheet_name='Holdings Report')


# Gráfica 1: Distribución del valor de mercado (USD) por los primeros 20 países (mejorada con valores reducidos)
plt.figure(figsize=(14, 8))
top_20_countries = data.groupby('Country')['Market Value(USD)'].sum().sort_values(ascending=False).head(20)
colors = sns.color_palette("coolwarm", len(top_20_countries))

# Convertir los valores a miles de millones (B) o millones (M)
def format_value(value):
    if value >= 1e9:
        return f'{value/1e9:.1f}B'  # Miles de millones
    else:
        return f'{value/1e6:.1f}M'  # Millones

top_20_countries.plot(kind='bar', color=colors)

# Añadir valores sobre las barras
for index, value in enumerate(top_20_countries):
    plt.text(index, value, format_value(value), ha='center', va='bottom', fontsize=10, rotation=90)

plt.title('Distribución del Valor de Mercado (USD) por los Primeros 20 Países', fontsize=16)
plt.ylabel('Valor de Mercado (USD)', fontsize=14)
plt.xlabel('País', fontsize=14)
plt.xticks(rotation=45, ha='right')
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.tight_layout()
plt.show()

