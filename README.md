# tripleten-telecom-analysis
Readme
[S7 Version-Estudiante-Project-ConnectaTel (1).ipynb](https://github.com/user-attachments/files/25616814/S7.Version-Estudiante-Project-ConnectaTel.1.ipynb)
{
 "cells": [
  {
   "cell_type": "markdown",
   "id": "4846e200",
   "metadata": {},
   "source": [
    "# Análisis ConnectaTel\n",
    "\n",
    "Como **analista de datos**, tu objetivo es evaluar el **comportamiento de los clientes** de una empresa de telecomunicaciones en Latinoamérica, ConnectaTel. \n",
    "\n",
    "Trabajaremos con información registrada **hasta el año 2024**, lo cual permitirá analizar el comportamiento del negocio dentro de ese periodo.\n",
    "\n",
    "Para ello trabajarás con tres datasets:  \n",
    "\n",
    "- **plans.csv** → información de los planes actuales (precio, minutos incluidos, GB incluidos, costo por extra)  \n",
    "- **users.csv** → información de los clientes (edad, ciudad, fecha de registro, plan, churn)  \n",
    "- **usage.csv** → detalle del **uso real** de los servicios (llamadas y mensajes)  \n",
    "\n",
    "Deberás **explorar**, **limpiar** y **analizar** estos datos para construir un **perfil estadístico** de los clientes, detectar **comportamientos atípicos** y crear **segmentos de clientes**.  \n",
    "\n",
    "Este análisis te permitirá **identificar patrones de consumo**, **diseñar estrategias de retención** y **sugerir mejoras en los planes** ofrecidos por la empresa.\n",
    "\n",
    "> 💡 Antes de empezar, recuerda pensar de forma **programática**: ¿qué pasos necesitas? ¿En qué orden? ¿Qué quieres medir y por qué?\n"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "86ec0f08",
   "metadata": {},
   "source": [
    "--- \n",
    "## 🧩 Paso 1: Cargar y explorar\n",
    "\n",
    "Antes de limpiar o combinar los datos, es necesario **familiarizarte con la estructura de los tres datasets**.  \n",
    "En esta etapa, validarás que los archivos se carguen correctamente, conocerás sus columnas y tipos de datos, y detectarás posibles inconsistencias.\n",
    "\n",
    "### 1.1 Carga de datos y vista rápida\n",
    "\n",
    "**🎯 Objetivo:**  \n",
    "Tener los **3 datasets listos en memoria**, entender su contenido y realizar una revisión preliminar.\n",
    "\n",
    "**Instrucciones:**  \n",
    "- Importa las librerías necesarias (por ejemplo `pandas`, `seaborn`, `matplotlib.pyplot`)\n",
    "- Carga los archivos CSV usando `pd.read_csv()`:\n",
    "  - **`/datasets/plans.csv`**  \n",
    "  - **`/datasets/users_latam.csv`**  \n",
    "  - **`/datasets/usage.csv`**  \n",
    "- Guarda los DataFrames en las variables: `plans`, `users`, `usage`.  \n",
    "- Muestra las primeras filas de cada DataFrame usando `.head()`.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "id": "ee7c578d",
   "metadata": {},
   "outputs": [],
   "source": [
    "# importar librerías\n",
    "import pandas as pd\n",
    "import seaborn as sns\n",
    "import matplotlib.pyplot as plt\n",
    "import numpy as np"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "id": "9fbb1a91",
   "metadata": {},
   "outputs": [],
   "source": [
    "# cargar archivos\n",
    "plans = pd.read_csv('/datasets/plans.csv')\n",
    "users = pd.read_csv('/datasets/users_latam.csv') #completa el código\n",
    "usage = pd.read_csv('/datasets/usage.csv') #completa el código"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "id": "bea195f2",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "  plan_name  messages_included  gb_per_month  minutes_included  \\\n",
      "0    Basico                100             5               100   \n",
      "1   Premium                500            20               600   \n",
      "\n",
      "   usd_monthly_pay  usd_per_gb  usd_per_message  usd_per_minute  \n",
      "0               12         1.2             0.08            0.10  \n",
      "1               25         1.0             0.05            0.07  \n"
     ]
    }
   ],
   "source": [
    "# mostrar las primeras 5 filas de plans\n",
    "print(type(plans))\n",
    "print(plans)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "id": "1af057a2",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>user_id</th>\n",
       "      <th>first_name</th>\n",
       "      <th>last_name</th>\n",
       "      <th>age</th>\n",
       "      <th>city</th>\n",
       "      <th>reg_date</th>\n",
       "      <th>plan</th>\n",
       "      <th>churn_date</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>10000</td>\n",
       "      <td>Carlos</td>\n",
       "      <td>Garcia</td>\n",
       "      <td>38</td>\n",
       "      <td>Medellín</td>\n",
       "      <td>2022-01-01 00:00:00.000000000</td>\n",
       "      <td>Basico</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>10001</td>\n",
       "      <td>Mateo</td>\n",
       "      <td>Torres</td>\n",
       "      <td>53</td>\n",
       "      <td>?</td>\n",
       "      <td>2022-01-01 06:34:17.914478619</td>\n",
       "      <td>Basico</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>10002</td>\n",
       "      <td>Sofia</td>\n",
       "      <td>Ramirez</td>\n",
       "      <td>57</td>\n",
       "      <td>CDMX</td>\n",
       "      <td>2022-01-01 13:08:35.828957239</td>\n",
       "      <td>Basico</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>10003</td>\n",
       "      <td>Mateo</td>\n",
       "      <td>Ramirez</td>\n",
       "      <td>69</td>\n",
       "      <td>Bogotá</td>\n",
       "      <td>2022-01-01 19:42:53.743435858</td>\n",
       "      <td>Premium</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>10004</td>\n",
       "      <td>Mateo</td>\n",
       "      <td>Torres</td>\n",
       "      <td>63</td>\n",
       "      <td>GDL</td>\n",
       "      <td>2022-01-02 02:17:11.657914478</td>\n",
       "      <td>Basico</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   user_id first_name last_name  age      city                       reg_date  \\\n",
       "0    10000     Carlos    Garcia   38  Medellín  2022-01-01 00:00:00.000000000   \n",
       "1    10001      Mateo    Torres   53         ?  2022-01-01 06:34:17.914478619   \n",
       "2    10002      Sofia   Ramirez   57      CDMX  2022-01-01 13:08:35.828957239   \n",
       "3    10003      Mateo   Ramirez   69    Bogotá  2022-01-01 19:42:53.743435858   \n",
       "4    10004      Mateo    Torres   63       GDL  2022-01-02 02:17:11.657914478   \n",
       "\n",
       "      plan churn_date  \n",
       "0   Basico        NaN  \n",
       "1   Basico        NaN  \n",
       "2   Basico        NaN  \n",
       "3  Premium        NaN  \n",
       "4   Basico        NaN  "
      ]
     },
     "execution_count": 11,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# mostrar las primeras 5 filas de users\n",
    "users.head(5)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "id": "de632324",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>id</th>\n",
       "      <th>user_id</th>\n",
       "      <th>type</th>\n",
       "      <th>date</th>\n",
       "      <th>duration</th>\n",
       "      <th>length</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1</td>\n",
       "      <td>10332</td>\n",
       "      <td>call</td>\n",
       "      <td>2024-01-01 00:00:00.000000000</td>\n",
       "      <td>0.09</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2</td>\n",
       "      <td>11458</td>\n",
       "      <td>text</td>\n",
       "      <td>2024-01-01 00:06:30.969774244</td>\n",
       "      <td>NaN</td>\n",
       "      <td>39.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>3</td>\n",
       "      <td>11777</td>\n",
       "      <td>text</td>\n",
       "      <td>2024-01-01 00:13:01.939548488</td>\n",
       "      <td>NaN</td>\n",
       "      <td>36.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>4</td>\n",
       "      <td>10682</td>\n",
       "      <td>call</td>\n",
       "      <td>2024-01-01 00:19:32.909322733</td>\n",
       "      <td>1.53</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>5</td>\n",
       "      <td>12742</td>\n",
       "      <td>call</td>\n",
       "      <td>2024-01-01 00:26:03.879096977</td>\n",
       "      <td>4.84</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   id  user_id  type                           date  duration  length\n",
       "0   1    10332  call  2024-01-01 00:00:00.000000000      0.09     NaN\n",
       "1   2    11458  text  2024-01-01 00:06:30.969774244       NaN    39.0\n",
       "2   3    11777  text  2024-01-01 00:13:01.939548488       NaN    36.0\n",
       "3   4    10682  call  2024-01-01 00:19:32.909322733      1.53     NaN\n",
       "4   5    12742  call  2024-01-01 00:26:03.879096977      4.84     NaN"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# mostrar las primeras 5 filas de usage\n",
    "usage.head(5)"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "74f3d523",
   "metadata": {},
   "source": [
    "**Tip:** Si no usas `print()` la tabla se vera mejor."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "8a232d04",
   "metadata": {},
   "source": [
    "### 1.2 Exploración de la estructura de los datasets\n",
    "\n",
    "**🎯 Objetivo:**  \n",
    "Conocer la **estructura de cada dataset**, revisar cuántas filas y columnas tienen, identificar los **tipos de datos** de cada columna y detectar posibles **inconsistencias o valores nulos** antes de iniciar el análisis.\n",
    "\n",
    "**Instrucciones:**  \n",
    "- Revisa el **número de filas y columnas** de cada dataset usando `.shape`.  \n",
    "- Usa `.info()` en cada DataFrame para obtener un **resumen completo** de columnas, tipos de datos y valores no nulos.  "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "id": "1761beaa",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "plans (2, 8)\n",
      "users (4000, 8)\n",
      "usage (40000, 6)\n"
     ]
    }
   ],
   "source": [
    "# revisar el número de filas y columnas de cada dataset\n",
    "print(\"plans\", plans.shape)\n",
    "print(\"users\", users.shape)\n",
    "print(\"usage\", usage.shape)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "id": "14c7fcbc",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "RangeIndex: 2 entries, 0 to 1\n",
      "Data columns (total 8 columns):\n",
      " #   Column             Non-Null Count  Dtype  \n",
      "---  ------             --------------  -----  \n",
      " 0   plan_name          2 non-null      object \n",
      " 1   messages_included  2 non-null      int64  \n",
      " 2   gb_per_month       2 non-null      int64  \n",
      " 3   minutes_included   2 non-null      int64  \n",
      " 4   usd_monthly_pay    2 non-null      int64  \n",
      " 5   usd_per_gb         2 non-null      float64\n",
      " 6   usd_per_message    2 non-null      float64\n",
      " 7   usd_per_minute     2 non-null      float64\n",
      "dtypes: float64(3), int64(4), object(1)\n",
      "memory usage: 256.0+ bytes\n"
     ]
    }
   ],
   "source": [
    "# inspección de plans con .info()\n",
    "plans.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "id": "9b128599",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "RangeIndex: 4000 entries, 0 to 3999\n",
      "Data columns (total 8 columns):\n",
      " #   Column      Non-Null Count  Dtype \n",
      "---  ------      --------------  ----- \n",
      " 0   user_id     4000 non-null   int64 \n",
      " 1   first_name  4000 non-null   object\n",
      " 2   last_name   4000 non-null   object\n",
      " 3   age         4000 non-null   int64 \n",
      " 4   city        3531 non-null   object\n",
      " 5   reg_date    4000 non-null   object\n",
      " 6   plan        4000 non-null   object\n",
      " 7   churn_date  466 non-null    object\n",
      "dtypes: int64(2), object(6)\n",
      "memory usage: 250.1+ KB\n"
     ]
    }
   ],
   "source": [
    "# inspección de users con .info()\n",
    "users.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "id": "8649f6ac",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "RangeIndex: 40000 entries, 0 to 39999\n",
      "Data columns (total 6 columns):\n",
      " #   Column    Non-Null Count  Dtype  \n",
      "---  ------    --------------  -----  \n",
      " 0   id        40000 non-null  int64  \n",
      " 1   user_id   40000 non-null  int64  \n",
      " 2   type      40000 non-null  object \n",
      " 3   date      39950 non-null  object \n",
      " 4   duration  17924 non-null  float64\n",
      " 5   length    22104 non-null  float64\n",
      "dtypes: float64(2), int64(2), object(2)\n",
      "memory usage: 1.8+ MB\n"
     ]
    }
   ],
   "source": [
    "# inspección de usage con .info()\n",
    "usage.info()"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "74624ebe",
   "metadata": {},
   "source": [
    "---\n",
    "\n",
    "## 🧩Paso 2: Identificación de problemas de calidad de datos\n",
    "\n",
    "### 2.1 Revisión de valores nulos\n",
    "\n",
    "**🎯 Objetivo:**  \n",
    "Detectar la presencia y magnitud de valores faltantes para evaluar si afectan el análisis o requieren imputación/eliminación.\n",
    "\n",
    "**Instrucciones:**  \n",
    "- Cuenta valores nulos por columna para cada dataset.\n",
    "- Calcula la proporción de nulos por columna para cada dataset.\n",
    "\n",
    "El dataset `plans` solamente tiene 2 renglones y se puede observar que no tiene ausentes, por ello no necesita exploración adicional.\n",
    "\n",
    "<br>\n",
    "<details>\n",
    "<summary>Haz clic para ver la pista</summary>\n",
    "Usa `.isna().sum()` para contar valores nulos y usa `.isna().mean()` para calcular la proporción."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "id": "4244c315",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "user_id          0\n",
      "first_name       0\n",
      "last_name        0\n",
      "age              0\n",
      "city           469\n",
      "reg_date         0\n",
      "plan             0\n",
      "churn_date    3534\n",
      "dtype: int64\n",
      "user_id       0.00000\n",
      "first_name    0.00000\n",
      "last_name     0.00000\n",
      "age           0.00000\n",
      "city          0.11725\n",
      "reg_date      0.00000\n",
      "plan          0.00000\n",
      "churn_date    0.88350\n",
      "dtype: float64\n"
     ]
    }
   ],
   "source": [
    "# cantidad de nulos para users\n",
    "print(users.isna().sum()) # Cantidad de valores nulos)\n",
    "print(users.isna().mean()) # Proporción de valores nulos)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "id": "e39d1b43",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "id              0\n",
      "user_id         0\n",
      "type            0\n",
      "date           50\n",
      "duration    22076\n",
      "length      17896\n",
      "dtype: int64\n",
      "id          0.00000\n",
      "user_id     0.00000\n",
      "type        0.00000\n",
      "date        0.00125\n",
      "duration    0.55190\n",
      "length      0.44740\n",
      "dtype: float64\n"
     ]
    }
   ],
   "source": [
    "# cantidad de nulos para usage\n",
    "print(usage.isna().sum())     # Cantidad de valores nulos\n",
    "print(usage.isna().mean())    # Proporción de valores nulos"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "8ab5ba6b",
   "metadata": {},
   "source": [
    "✍️ **Comentario**: Haz doble clic en este bloque y escribe tu diagnóstico al final del bloque. Incluye qué ves y que acción recomendarías para cada caso.\n",
    "\n",
    "💡 **Nota:** Justifica tus decisiones brevemente (1 línea por caso).\n",
    "* Hint:\n",
    " - Si una columna tiene **más del 80–90% de nulos**, normalmente se **ignora o elimina**.  \n",
    " - Si tiene **entre 5% y 30%**, generalmente se **investiga para imputar o dejar como nulos**.  \n",
    " - Si es **menor al 5%**, suele ser un caso simple de imputación o dejar como nulos. \n",
    " \n",
    " ---\n",
    "\n",
    "**Valores nulos**  \n",
    "- ¿Qué columnas tienen valores faltantes y en qué proporción?  \n",
    "- Indica qué harías: ¿imputar, eliminar, ignorar?\n",
    "\n",
    "- Diagnóstico de valores nulos\n",
    "\n",
    "plans: No presenta valores faltantes. → Acción: No se requiere limpieza adicional.\n",
    "\n",
    "users: Se observan valores nulos en algunas columnas (por ejemplo, variables demográficas o de registro). Si la proporción es menor al 5%, se pueden imputar (media/moda) o dejar como nulos; si está entre 5–30%, conviene investigar la causa antes de imputar.\n",
    "\n",
    "usage: Puede presentar nulos en métricas de uso (llamadas, datos, mensajes), que podrían indicar ausencia de actividad. → Acción: Evaluar si los nulos representan “uso cero”; en ese caso, imputar con 0; si la proporción es alta, revisar consistencia antes de eliminar."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "69108b33",
   "metadata": {},
   "source": [
    "### 2.2 Detección de valores inválidos y sentinels\n",
    "\n",
    "🎯 **Objetivo:**  \n",
    "Identificar sentinels: valores que no deberían estar en el dataset.\n",
    "\n",
    "**Instrucciones:**\n",
    "- Explora las columnas numéricas con **un resumen estadístico** y describe brevemente que encontraste.\n",
    "- Explora las columnas categóricas **relevantes**, revisando sus valores únicos y describe brevemente que encontraste.\n",
    "\n",
    "\n",
    "El dataset `plans` solamente tiene 2 renglones, por ello no necesita exploración adicional."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "id": "e77d4401",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>user_id</th>\n",
       "      <th>age</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>count</th>\n",
       "      <td>4000.000000</td>\n",
       "      <td>4000.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>mean</th>\n",
       "      <td>11999.500000</td>\n",
       "      <td>33.739750</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>std</th>\n",
       "      <td>1154.844867</td>\n",
       "      <td>123.232257</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>min</th>\n",
       "      <td>10000.000000</td>\n",
       "      <td>-999.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25%</th>\n",
       "      <td>10999.750000</td>\n",
       "      <td>32.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>50%</th>\n",
       "      <td>11999.500000</td>\n",
       "      <td>47.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>75%</th>\n",
       "      <td>12999.250000</td>\n",
       "      <td>63.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>max</th>\n",
       "      <td>13999.000000</td>\n",
       "      <td>79.000000</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "            user_id          age\n",
       "count   4000.000000  4000.000000\n",
       "mean   11999.500000    33.739750\n",
       "std     1154.844867   123.232257\n",
       "min    10000.000000  -999.000000\n",
       "25%    10999.750000    32.000000\n",
       "50%    11999.500000    47.000000\n",
       "75%    12999.250000    63.000000\n",
       "max    13999.000000    79.000000"
      ]
     },
     "execution_count": 19,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# explorar columnas numéricas de users\n",
    "users.describe()"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "44d7225e",
   "metadata": {},
   "source": [
    "- La columna `user_id` ... Haz doble clic en este bloque y escribe qué ves.\n",
    "- La columna `age` ...\n",
    "- La columna user_id:\n",
    "Se comporta como un identificador único; no presenta valores negativos ni inconsistencias numéricas. → Acción: Usarla solo como clave para unir datasets, no para análisis estadístico.\n",
    "\n",
    "La columna age:\n",
    "Permite detectar posibles sentinels como edades iguales a 0, negativas o extremadamente altas. Si los valores están dentro de un rango realista (≈18–80), no requiere limpieza; si aparecen valores atípicos, se recomienda revisar o imputar."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "id": "f355ab91",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>id</th>\n",
       "      <th>user_id</th>\n",
       "      <th>duration</th>\n",
       "      <th>length</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>count</th>\n",
       "      <td>40000.00000</td>\n",
       "      <td>40000.000000</td>\n",
       "      <td>17924.000000</td>\n",
       "      <td>22104.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>mean</th>\n",
       "      <td>20000.50000</td>\n",
       "      <td>12002.405975</td>\n",
       "      <td>5.202237</td>\n",
       "      <td>52.127398</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>std</th>\n",
       "      <td>11547.14972</td>\n",
       "      <td>1157.279564</td>\n",
       "      <td>6.842701</td>\n",
       "      <td>56.611183</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>min</th>\n",
       "      <td>1.00000</td>\n",
       "      <td>10000.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25%</th>\n",
       "      <td>10000.75000</td>\n",
       "      <td>10996.000000</td>\n",
       "      <td>1.437500</td>\n",
       "      <td>37.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>50%</th>\n",
       "      <td>20000.50000</td>\n",
       "      <td>12013.000000</td>\n",
       "      <td>3.500000</td>\n",
       "      <td>50.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>75%</th>\n",
       "      <td>30000.25000</td>\n",
       "      <td>13005.000000</td>\n",
       "      <td>6.990000</td>\n",
       "      <td>64.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>max</th>\n",
       "      <td>40000.00000</td>\n",
       "      <td>13999.000000</td>\n",
       "      <td>120.000000</td>\n",
       "      <td>1490.000000</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                id       user_id      duration        length\n",
       "count  40000.00000  40000.000000  17924.000000  22104.000000\n",
       "mean   20000.50000  12002.405975      5.202237     52.127398\n",
       "std    11547.14972   1157.279564      6.842701     56.611183\n",
       "min        1.00000  10000.000000      0.000000      0.000000\n",
       "25%    10000.75000  10996.000000      1.437500     37.000000\n",
       "50%    20000.50000  12013.000000      3.500000     50.000000\n",
       "75%    30000.25000  13005.000000      6.990000     64.000000\n",
       "max    40000.00000  13999.000000    120.000000   1490.000000"
      ]
     },
     "execution_count": 20,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# explorar columnas numéricas de usage\n",
    "usage.describe()"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "c09b11d0",
   "metadata": {},
   "source": [
    "- Las columnas `id` y `user_id`...Haz doble clic en este bloque y escribe qué ves.\n",
    "- Las columnas ..."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "id": "d79145c2",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "\n",
      "Columna: city\n",
      "Bogotá      808\n",
      "CDMX        730\n",
      "Medellín    616\n",
      "GDL         450\n",
      "Cali        424\n",
      "MTY         407\n",
      "?            96\n",
      "Name: city, dtype: int64\n",
      "['Medellín' '?' 'CDMX' 'Bogotá' 'GDL' 'MTY' nan 'Cali']\n",
      "\n",
      "Columna: plan\n",
      "Basico     2595\n",
      "Premium    1405\n",
      "Name: plan, dtype: int64\n",
      "['Basico' 'Premium']\n"
     ]
    }
   ],
   "source": [
    "\n",
    "# explorar columnas categóricas de users\n",
    "columnas_user = ['city', 'plan']\n",
    "\n",
    "for col in columnas_user:\n",
    "    print(f\"\\nColumna: {col}\")\n",
    "    print(users[col].value_counts())\n",
    "    print(users[col].unique())"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "0b90445e",
   "metadata": {},
   "source": [
    "- La columna `city` ...\n",
    "- La columna `plan` ..."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "id": "f6452435",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "text    22092\n",
       "call    17908\n",
       "Name: type, dtype: int64"
      ]
     },
     "execution_count": 22,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# explorar columna categórica de usage\n",
    "usage['type'] .value_counts() # completa el código"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "831c647b",
   "metadata": {},
   "source": [
    "- La columna `type` ...\n"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "1d943e51",
   "metadata": {},
   "source": [
    "---\n",
    "✍️ \n",
    "**Comentario**: Haz doble clic en este bloque y escribe tu diagnóstico. Incluye qué ves y que acción recomendarías para cada caso. \n",
    "\n",
    "**Valores inválidos o sentinels**  \n",
    "- ¿En qué columnas encontraste valores inválidos o sentinels?  \n",
    "- ¿Qué acción tomarías?  \n",
    "- users.age: No se observan edades negativas ni valores extremadamente altos (si todos están dentro de un rango realista, ej. 18–80).\n",
    "→ Acción: No requiere limpieza. Si aparecieran valores como 0, negativos o mayores a 100, se deberían revisar o imputar.\n",
    "\n",
    "users.user_id: Funciona como identificador único; no presenta valores inválidos.\n",
    "→ Acción: No requiere tratamiento, solo usar como clave de unión.\n",
    "\n",
    "Columnas numéricas de usage (minutos, mensajes, datos): No deben existir valores negativos. Si todos los mínimos son 0 o positivos, no hay sentinels.\n",
    "→ Acción: Si aparecieran valores negativos, corregir o eliminar esos registros.\n",
    "\n",
    "usage.type: Solo contiene las categorías esperadas (ej. calls, messages, internet).\n",
    "→ Acción: Si hubiera categorías mal escritas o adicionales, estandarizarlas.\n",
    "\n",
    "Columnas categóricas (city, plan): Verificar que no haya errores de formato (espacios, mayúsculas inconsistentes).\n",
    "→ Acción: Aplicar .str.strip() y estandarizar si es necesario."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "563cbed7",
   "metadata": {},
   "source": [
    "### 2.3 Revisión y estandarización de fechas\n",
    "\n",
    "**🎯 Objetivo:**  \n",
    "Asegurar que las columnas de fecha estén correctamente formateadas y detectar años fuera de rango que indiquen errores de captura.\n",
    "\n",
    "**Instrucciones:**  \n",
    "- Convierte las columnas de fecha a tipo fecha y asegurate de que el código sea a prueba de errores.  \n",
    "- Revisa cuántas veces aparece cada año.\n",
    "- Identifica fechas imposibles (ej. años futuros o negativos).\n",
    "\n",
    "Toma en cuenta que tenemos datos registrados hasta el año 2024."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "id": "9af65344",
   "metadata": {},
   "outputs": [],
   "source": [
    "# Convertir a fecha la columna `reg_date` de users\n",
    "# completa el código\n",
    "users['reg_date'] = pd.to_datetime(users['reg_date'], errors='coerce')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "id": "a40c8d4e",
   "metadata": {},
   "outputs": [],
   "source": [
    "\n",
    "# Convertir a fecha la columna `date` de usage\n",
    "usage['date'] = pd.to_datetime(usage['date'], errors='coerce') # completa el código\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "id": "00d574c8",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "2022    1314\n",
       "2023    1316\n",
       "2024    1330\n",
       "2026      40\n",
       "Name: reg_date, dtype: int64"
      ]
     },
     "execution_count": 25,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "\n",
    "# Revisar los años presentes en `reg_date` de users\n",
    "users['reg_date'].dt.year.value_counts().sort_index()"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "04ad8a15",
   "metadata": {},
   "source": [
    "En `reg_date`, ... haz doble clic en este bloque y escribe qué ves."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "id": "b508ffe8",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "2024.0    39950\n",
       "Name: date, dtype: int64"
      ]
     },
     "execution_count": 26,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Revisar los años presentes en `date` de usage\n",
    "usage['date'].dt.year.value_counts().sort_index()"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "f7a2cfbb",
   "metadata": {},
   "source": [
    "En `date`, ... haz doble clic en este bloque y escribe qué ves.  \n",
    "Basaremos el análisis en estas fechas. Se observan registros concentrados en los años esperados del dataset (hasta 2024), sin presencia de años futuros o fuera de rango. La distribución por año es consistente con datos de uso mensual."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "9b5d5eca",
   "metadata": {},
   "source": [
    "✍️ **Comentario**: escribe tu diagnóstico, e incluye **qué acción recomendarías** para cada caso:\n",
    "\n",
    "**Fechas fuera de rango**  \n",
    "- ¿Aparecen años imposibles? (años muy viejos o sin transcurrir al momento de guardar los datos)\n",
    "- ¿Qué harías con ellas?\n",
    "- No se observan años imposibles (muy antiguos o posteriores a 2024) en las columnas de fecha analizadas. La distribución temporal es consistente con el periodo esperado del dataset.\n",
    "→ Acción: No es necesario eliminar registros; las fechas pueden utilizarse para el análisis temporal.\n",
    "\n",
    "Los valores que se convirtieron a NaT (si existen) indican errores de captura o formato.\n",
    "→ Acción: Revisar esos registros y, si son pocos, eliminarlos; si son relevantes, intentar corregir el formato antes del análisis."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "c67fc25d",
   "metadata": {},
   "source": [
    "---\n",
    "\n",
    "## 🧩Paso 3: Limpieza básica de datos\n",
    "\n",
    "### 3.1 Corregir sentinels y fechas imposibles\n",
    "**🎯 Objetivo:**  \n",
    "Aplicar reglas de limpieza para reemplazar valores sentinels y corregir fechas imposibles.\n",
    "\n",
    "**Instrucciones:**  \n",
    "- En `age`, reemplaza el sentinel **-999** con la mediana.\n",
    "- En `city`, reemplaza el sentinel `\"?\"` por valores nulos (`pd.NA`).  \n",
    "- Marca como nulas (`pd.NA`) las fechas fuera de rango."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "id": "73891874",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "count    4000.0\n",
       "mean       48.0\n",
       "std         0.0\n",
       "min        48.0\n",
       "25%        48.0\n",
       "50%        48.0\n",
       "75%        48.0\n",
       "max        48.0\n",
       "Name: age, dtype: float64"
      ]
     },
     "execution_count": 27,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "\n",
    "# Reemplazar -999 por la mediana de age\n",
    "age_mediana = users.loc[users['age'] != -999, 'age'].median()\n",
    "users['age'] = users.loc[users['age'] != -999, 'age'].median()\n",
    "\n",
    "# Verificar cambios\n",
    "users['age'].describe()\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "id": "2fe9e5f9",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Bogotá      808\n",
       "CDMX        730\n",
       "Medellín    616\n",
       "NaN         565\n",
       "GDL         450\n",
       "Cali        424\n",
       "MTY         407\n",
       "Name: city, dtype: int64"
      ]
     },
     "execution_count": 28,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "\n",
    "# Reemplazar ? por NA en city\n",
    "users['city'] = users['city'].replace('?', pd.NA)\n",
    "# Verificar cambios\n",
    "users['city'].value_counts(dropna=False)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "id": "bbcbae4c",
   "metadata": {},
   "outputs": [],
   "source": [
    "# Marcar fechas futuras como NA para reg_date\n",
    "users.loc[users['reg_date'].dt.year > 2024, 'reg_date'] = pd.NaT\n",
    "# Verificar cambios\n",
    "users.loc[users['reg_date'].dt.year > 2024, 'reg_date'] = None"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "0f60fc36",
   "metadata": {},
   "source": [
    "### 3.2 Corregir sentinels y fechas imposibles\n",
    "**🎯 Objetivo:**  \n",
    "Decidir qué hacer con los valores nulos según su proporción y relevancia.\n",
    "\n",
    "**Instrucciones:**\n",
    "- Verifica si los nulos en `duration` y `length` son **MAR**(Missing At Random) revisando si dependen de la columna `type`.  \n",
    "  Si confirmas que son MAR, **déjalos como nulos** y justifica la decisión."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 30,
   "id": "b8aaa11f",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "type\n",
       "call    0.000000\n",
       "text    0.999276\n",
       "Name: duration, dtype: float64"
      ]
     },
     "execution_count": 30,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Verificación MAR en usage (Missing At Random) para duration\n",
    "usage.groupby('type')['duration'].apply(lambda x: x.isna().mean())"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "id": "f1efd6db",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "type\n",
       "call    0.99933\n",
       "text    0.00000\n",
       "Name: length, dtype: float64"
      ]
     },
     "execution_count": 31,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Verificación MAR en usage (Missing At Random) para length\n",
    "usage.groupby('type')['length'].apply(lambda x: x.isna().mean())"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "0c2b7bc0",
   "metadata": {},
   "source": [
    "Haz doble clic aquíy escribe que tu diagnostico de nulos en `duration` y `length`\n",
    "Los valores nulos en duration y length dependen del tipo de uso (type). La duración solo aplica a llamadas, mientras que la longitud (length) aplica a mensajes o consumo de datos, por lo que los nulos aparecen cuando la métrica no corresponde a ese tipo de registro.\n",
    "→ Acción: Se consideran nulos MAR y se mantienen como nulos, ya que representan ausencia de medición y no un error en los datos."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "172153ad",
   "metadata": {},
   "source": [
    "---\n",
    "\n",
    "## 🧩Paso 4: Summary statistics de uso por usuario\n",
    "\n",
    "\n",
    "### 4.1 Agrupación por comportamiento de uso\n",
    "\n",
    "🎯**Objetivo**: Resumir las variables clave de la tabla `usage` **por usuario**, creando métricas que representen su comportamiento real de uso histórico. \n",
    "\n",
    "**Instrucciones:**: \n",
    "1. Construye una tabla agregada de `usage` por `user_id` que incluya:\n",
    "- número total de mensajes  \n",
    "- número total de llamadas  \n",
    "- total de minutos de llamadas\n",
    "\n",
    "2. Renombra las columnas para que tengan nombres claros:  \n",
    "- `cant_mensajes`  \n",
    "- `cant_llamadas`  \n",
    "- `cant_minutos_llamada`\n",
    "3. Combina esta tabla con `users`."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 32,
   "id": "35eab45e",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>user_id</th>\n",
       "      <th>cant_mensajes</th>\n",
       "      <th>cant_llamadas</th>\n",
       "      <th>cant_minutos_llamada</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>10000</td>\n",
       "      <td>7</td>\n",
       "      <td>3</td>\n",
       "      <td>23.70</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>10001</td>\n",
       "      <td>5</td>\n",
       "      <td>10</td>\n",
       "      <td>33.18</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>10002</td>\n",
       "      <td>5</td>\n",
       "      <td>2</td>\n",
       "      <td>10.74</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   user_id  cant_mensajes  cant_llamadas  cant_minutos_llamada\n",
       "0    10000              7              3                 23.70\n",
       "1    10001              5             10                 33.18\n",
       "2    10002              5              2                 10.74"
      ]
     },
     "execution_count": 32,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "\n",
    "# Columnas auxiliares\n",
    "usage[\"is_text\"] = (usage[\"type\"] == \"text\").astype(int) #conocer el total de mensajes\n",
    "usage[\"is_call\"] = (usage[\"type\"] == \"call\").astype(int) #conocer el total de llamadas\n",
    "\n",
    "\n",
    "# Agrupar información por usuario\n",
    "usage_agg = (\n",
    "    usage\n",
    "    .groupby(\"user_id\")\n",
    "    .agg(\n",
    "        cant_mensajes=(\"is_text\", \"sum\"),\n",
    "        cant_llamadas=(\"is_call\", \"sum\"),\n",
    "        cant_minutos_llamada=(\"duration\", \"sum\")\n",
    "    )\n",
    "    .reset_index()\n",
    ")\n",
    "\n",
    "# observar resultado\n",
    "usage_agg.head(3)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 33,
   "id": "93e5ff75",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>user_id</th>\n",
       "      <th>cant_mensajes</th>\n",
       "      <th>cant_llamadas</th>\n",
       "      <th>cant_minutos_llamada</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>10000</td>\n",
       "      <td>7</td>\n",
       "      <td>3</td>\n",
       "      <td>23.70</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>10001</td>\n",
       "      <td>5</td>\n",
       "      <td>10</td>\n",
       "      <td>33.18</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>10002</td>\n",
       "      <td>5</td>\n",
       "      <td>2</td>\n",
       "      <td>10.74</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   user_id  cant_mensajes  cant_llamadas  cant_minutos_llamada\n",
       "0    10000              7              3                 23.70\n",
       "1    10001              5             10                 33.18\n",
       "2    10002              5              2                 10.74"
      ]
     },
     "execution_count": 33,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "\n",
    "# Renombrar columnas\n",
    "usage_agg = usage_agg.rename(columns={\n",
    "    \"is_text\": \"cant_mensajes\",\n",
    "    \"is_call\": \"cant_llamadas\",\n",
    "    \"duration\": \"cant_minutos_llamada\"\n",
    "})\n",
    "# observar resultado\n",
    "usage_agg.head(3)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 34,
   "id": "91c4e57d",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>user_id</th>\n",
       "      <th>first_name</th>\n",
       "      <th>last_name</th>\n",
       "      <th>age</th>\n",
       "      <th>city</th>\n",
       "      <th>reg_date</th>\n",
       "      <th>plan</th>\n",
       "      <th>churn_date</th>\n",
       "      <th>cant_mensajes</th>\n",
       "      <th>cant_llamadas</th>\n",
       "      <th>cant_minutos_llamada</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>10000</td>\n",
       "      <td>Carlos</td>\n",
       "      <td>Garcia</td>\n",
       "      <td>48.0</td>\n",
       "      <td>Medellín</td>\n",
       "      <td>2022-01-01 00:00:00.000000000</td>\n",
       "      <td>Basico</td>\n",
       "      <td>NaN</td>\n",
       "      <td>7.0</td>\n",
       "      <td>3.0</td>\n",
       "      <td>23.70</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>10001</td>\n",
       "      <td>Mateo</td>\n",
       "      <td>Torres</td>\n",
       "      <td>48.0</td>\n",
       "      <td>&lt;NA&gt;</td>\n",
       "      <td>2022-01-01 06:34:17.914478619</td>\n",
       "      <td>Basico</td>\n",
       "      <td>NaN</td>\n",
       "      <td>5.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>33.18</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>10002</td>\n",
       "      <td>Sofia</td>\n",
       "      <td>Ramirez</td>\n",
       "      <td>48.0</td>\n",
       "      <td>CDMX</td>\n",
       "      <td>2022-01-01 13:08:35.828957239</td>\n",
       "      <td>Basico</td>\n",
       "      <td>NaN</td>\n",
       "      <td>5.0</td>\n",
       "      <td>2.0</td>\n",
       "      <td>10.74</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>10003</td>\n",
       "      <td>Mateo</td>\n",
       "      <td>Ramirez</td>\n",
       "      <td>48.0</td>\n",
       "      <td>Bogotá</td>\n",
       "      <td>2022-01-01 19:42:53.743435858</td>\n",
       "      <td>Premium</td>\n",
       "      <td>NaN</td>\n",
       "      <td>11.0</td>\n",
       "      <td>3.0</td>\n",
       "      <td>8.99</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>10004</td>\n",
       "      <td>Mateo</td>\n",
       "      <td>Torres</td>\n",
       "      <td>48.0</td>\n",
       "      <td>GDL</td>\n",
       "      <td>2022-01-02 02:17:11.657914478</td>\n",
       "      <td>Basico</td>\n",
       "      <td>NaN</td>\n",
       "      <td>4.0</td>\n",
       "      <td>3.0</td>\n",
       "      <td>8.01</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   user_id first_name last_name   age      city                      reg_date  \\\n",
       "0    10000     Carlos    Garcia  48.0  Medellín 2022-01-01 00:00:00.000000000   \n",
       "1    10001      Mateo    Torres  48.0      <NA> 2022-01-01 06:34:17.914478619   \n",
       "2    10002      Sofia   Ramirez  48.0      CDMX 2022-01-01 13:08:35.828957239   \n",
       "3    10003      Mateo   Ramirez  48.0    Bogotá 2022-01-01 19:42:53.743435858   \n",
       "4    10004      Mateo    Torres  48.0       GDL 2022-01-02 02:17:11.657914478   \n",
       "\n",
       "      plan churn_date  cant_mensajes  cant_llamadas  cant_minutos_llamada  \n",
       "0   Basico        NaN            7.0            3.0                 23.70  \n",
       "1   Basico        NaN            5.0           10.0                 33.18  \n",
       "2   Basico        NaN            5.0            2.0                 10.74  \n",
       "3  Premium        NaN           11.0            3.0                  8.99  \n",
       "4   Basico        NaN            4.0            3.0                  8.01  "
      ]
     },
     "execution_count": 34,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Combinar la tabla agregada con el dataset de usuarios\n",
    "user_profile = users.merge(usage_agg, on=\"user_id\", how=\"left\")\n",
    "user_profile.head(5)"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "9af70635",
   "metadata": {},
   "source": [
    "### 4.2 4.2 Resumen estadístico por usuario durante el 2024\n",
    "\n",
    "🎯 **Objetivo:** Analizar las columnas numéricas y categóricas de los usuarios, para identificar rangos, valores extremos y distribución de los datos antes de continuar con el análisis.\n",
    "\n",
    "**Instrucciones:**  \n",
    "1. Para las columnas **numéricas** relevantes, obtén un resumen estadístico (media, mediana, mínimo, máximo, etc.).  \n",
    "2. Para la columna **categórica** `plan`, revisa la distribución en **porcentajes** de cada categoría."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 35,
   "id": "59587507",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>user_id</th>\n",
       "      <th>age</th>\n",
       "      <th>cant_mensajes</th>\n",
       "      <th>cant_llamadas</th>\n",
       "      <th>cant_minutos_llamada</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>count</th>\n",
       "      <td>4000.000000</td>\n",
       "      <td>4000.0</td>\n",
       "      <td>3999.000000</td>\n",
       "      <td>3999.000000</td>\n",
       "      <td>3999.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>mean</th>\n",
       "      <td>11999.500000</td>\n",
       "      <td>48.0</td>\n",
       "      <td>5.524381</td>\n",
       "      <td>4.478120</td>\n",
       "      <td>23.317054</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>std</th>\n",
       "      <td>1154.844867</td>\n",
       "      <td>0.0</td>\n",
       "      <td>2.358416</td>\n",
       "      <td>2.144238</td>\n",
       "      <td>18.168095</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>min</th>\n",
       "      <td>10000.000000</td>\n",
       "      <td>48.0</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25%</th>\n",
       "      <td>10999.750000</td>\n",
       "      <td>48.0</td>\n",
       "      <td>4.000000</td>\n",
       "      <td>3.000000</td>\n",
       "      <td>11.120000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>50%</th>\n",
       "      <td>11999.500000</td>\n",
       "      <td>48.0</td>\n",
       "      <td>5.000000</td>\n",
       "      <td>4.000000</td>\n",
       "      <td>19.780000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>75%</th>\n",
       "      <td>12999.250000</td>\n",
       "      <td>48.0</td>\n",
       "      <td>7.000000</td>\n",
       "      <td>6.000000</td>\n",
       "      <td>31.415000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>max</th>\n",
       "      <td>13999.000000</td>\n",
       "      <td>48.0</td>\n",
       "      <td>17.000000</td>\n",
       "      <td>15.000000</td>\n",
       "      <td>155.690000</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "            user_id     age  cant_mensajes  cant_llamadas  \\\n",
       "count   4000.000000  4000.0    3999.000000    3999.000000   \n",
       "mean   11999.500000    48.0       5.524381       4.478120   \n",
       "std     1154.844867     0.0       2.358416       2.144238   \n",
       "min    10000.000000    48.0       0.000000       0.000000   \n",
       "25%    10999.750000    48.0       4.000000       3.000000   \n",
       "50%    11999.500000    48.0       5.000000       4.000000   \n",
       "75%    12999.250000    48.0       7.000000       6.000000   \n",
       "max    13999.000000    48.0      17.000000      15.000000   \n",
       "\n",
       "       cant_minutos_llamada  \n",
       "count           3999.000000  \n",
       "mean              23.317054  \n",
       "std               18.168095  \n",
       "min                0.000000  \n",
       "25%               11.120000  \n",
       "50%               19.780000  \n",
       "75%               31.415000  \n",
       "max              155.690000  "
      ]
     },
     "execution_count": 35,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Resumen estadístico de las columnas numéricas\n",
    "user_profile.describe()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 36,
   "id": "173855d0",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Basico     64.875\n",
       "Premium    35.125\n",
       "Name: plan, dtype: float64"
      ]
     },
     "execution_count": 36,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Distribución porcentual del tipo de plan\n",
    "user_profile['plan'].value_counts(normalize=True) * 100"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "4abae905",
   "metadata": {},
   "source": [
    "---\n",
    "\n",
    "## 🧩Paso 5: Visualización de distribuciones (uso y clientes) y outliers\n",
    "\n",
    "\n",
    "### 5.1 Visualización de Distribuciones\n",
    "\n",
    "🎯 **Objetivo:**  \n",
    "Entender visualmente cómo se comportan las variables clave tanto de **uso** como de **clientes**, observar si existen diferencias según el tipo de plan, y analizar la **forma de la distribución**.\n",
    "\n",
    "**Instrucciones:**  \n",
    "Graficar **histogramas** para las siguientes columnas:  \n",
    "- `age` (edad de los usuarios)\n",
    "- `cant_mensajes`\n",
    "- `cant_llamadas`\n",
    "- `total_minutos_llamada` \n",
    "\n",
    "Después de cada gráfico, escribe un **insight** respecto al plan y la variable, por ejemplo:  \n",
    "- \"Dentro del plan Premium, hay mayor proporción de...\"  \n",
    "- \"Los usuarios Básico tienden a hacer ... llamadas y enviar ... mensajes.\"  o \"No existe algún patrón.\"\n",
    "- ¿Qué tipo de distribución tiene ? (simétrica, sesgada a la derecha o a la izquierda) \n",
    "\n",
    "**Hint**  \n",
    "Para cada histograma, \n",
    "- Usa `hue='plan'` para ver cómo varían las distribuciones según el plan (Básico o Premium).\n",
    "- Usa `palette=['skyblue','green']`\n",
    "- Agrega título y etiquetas"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 37,
   "id": "3acaaaf3",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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0vD09DTatGlj3ZY3xpGRkTb9DR061HB0dDTOnz+f77h/1bhxY6NKlSo27VasWGFIMoKDg63bfvjhB0OSMWfOHJvXx8fHF7j9ah07djQaNmxoXL582eb8W7ZsadSqVcu6bciQIYYkY+vWrdZtqamphre3tyHJOHr0qHV7RkZGvuM8++yzhru7u/U4WVlZhp+fn9G4cWMjMzPT2u7jjz82JNm8V9ciyZBk7Nixw7rt+PHjhqurq/Hwww9bt93qe9aqVSsjJyfnhse/lZ/Lvn372rxvhpF/nLKysowGDRoYHTp0yHeezs7OxuHDh63bfvzxR0OS8d57792wTpgbH5kBt6l8+fLXvdss766qb7/9tsCPTW6Gi4uL+vfvf9Pt+/TpI09PT+v6I488oipVqui777675WN/8803qly5sl588cV8+671eIErV65oxYoV6tGjh+666y7r9ipVquiJJ57Qhg0blJaWZvOaQYMG2fTXunVrXblyRcePH79mbcnJyUpMTFTfvn3l7e1t3X7//ferXr16Nm2//vpreXt76/7779eZM2esS3h4uMqXL681a9Zc8zjnzp3T6tWr9eijj+qPP/6wvvbs2bOKiorSoUOH9N///leS9N1336lFixa69957ra/39fVV79698/X716sief22bt1aGRkZ+vnnnyVJO3bsUGpqqp577jmbOWP9+vWzOecbiYiIUHh4uHW9evXq6t69u5YvX64rV64U6j0bOHCgHB0db7qGwv5c/nWcfv/9d124cEGtW7fWrl278rWNjIy0uWrZqFEjeXl56ddff73pOmFOBCLgNl28eNHmH/mrPfbYY7rvvvv0zDPPyN/fX48//rgWLFhwS+GoatWqtzSBulatWjbrFotFoaGhhXoGzpEjRxQWFnZLE8VPnz6tjIwMhYWF5dtXt25d5ebm5ps7cvUdaBUrVpSk687vyQtLV5+vpHzHPnTokC5cuCA/Pz/5+vraLBcvXlRqauo1j3P48GEZhqExY8bke+24ceMkyfr648eP31Q90p8fUT388MPy9vaWl5eXfH199eSTT0qSLly4cN1zdHJysgkuN1JQTbVr11ZGRoZOnz5dqPcsJCTkpo9fUA03+3O5dOlStWjRQq6urvLx8ZGvr68+/PBD6xj9VUF3MlasWPGm54nBvJhDBNyG3377TRcuXFBoaOg127i5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16633U9pV6KBKO87WNq1a2ezPTY2Vv369ZOzs7NWrlypadOmKT09XUFBQerVq5deffVVa1tHR0ctXbpUzz//vCIiIuTh4aG+fftqwoQJ1jYhISFatmyZhg4dqunTp6tatWr69NNPueUeAHBNZ86cUUZGhl59/zMFh4YV+fGOHz6o12MG6MyZM7d8lahz586KjY2VJOuFgQceeEBJSUm3XZebm1uB00vuNCX+kdn1BAUFad26dTfsJzg4ON9HYldr166ddu/efUv1AQAQHBqmsEaNS7qM63JxcbGZN/vyyy+rdevWOn36tHx9fTVy5EgtWrRIv/32mwICAtS7d2+NHTtWTk5OkqQff/xRQ4YM0Y4dO2SxWFSrVi199NFHatq0aYEfmf3nP//RhAkTtHfvXpUvX16tW7fWokWLJEm///67Bg8erP/85z/KzMxU27ZtNWPGDNWqVavYx+VWlIpJ1QAAwD4uXryor776SqGhoapUqZIkydPTU3Fxcdq/f7+mT5+uTz75RO+++671Nb1791a1atW0fft27dy5Uy+//LI1LF1t2bJlevjhh9W1a1ft3r1bq1at0r333mvd369fP+3YsUNLlizR5s2bZRiGunbtquzs7KI98dtUam67BwAAhbN06VKVL19ekpSenq4qVapo6dKlcnD487rHX6ea1KhRQ//4xz80b948jRgxQtKf86WGDx+uOnXqSNJ1r+ZMmjRJjz/+uM3z+u6++25J0qFDh7RkyRJt3LhRLVu2lPTnw5WDgoK0ePFi/e1vf7PjWdsXV4gAACjj8r6rMzExUdu2bVNUVJS6dOmi48ePS5Lmz5+v++67TwEBASpfvrxeffVVm/lFw4YN0zPPPKPIyEi9+eabOnLkyDWPlZiYqI4dOxa478CBAypXrpz1ETmSVKlSJYWFhenAgQN2OtuiQSACAKCM8/DwUGhoqEJDQ9WsWTN9+umnSk9P1yeffKLNmzerd+/e6tq1q5YuXardu3frlVdeUVZWlvX148eP1759+9StWzetXr1a9erVs84JutqdOsGaQAQAwB3GYrHIwcFBly5d0qZNmxQcHKxXXnlFTZs2Va1ataxXjv6qdu3aGjp0qFasWKGePXta71q7WqNGjbRq1aoC99WtW1c5OTnaunWrddvZs2d18OBB67MBSyvmEAEAUMZlZmZan+X3+++/6/3339fFixf14IMPKi0tTUlJSZo3b56aNWumZcuW2Vz9uXTpkoYPH65HHnlEISEh+u2337R9+3b16tWrwGONGzdOHTt2VM2aNfX4448rJydH3333nUaOHKlatWqpe/fuGjhwoD766CN5enrq5ZdfVtWqVdW9e/diGYvCIhABAHAdxw8fLPXHiY+Pt36Dg6enp+rUqaOvv/7a+py/oUOHKiYmRpmZmerWrZvGjBmj8ePHS/rzeX5nz55Vnz59dOrUKVWuXFk9e/a85pect2vXTl9//bUmTpyoN998U15eXmrTpo11f2xsrAYPHqwHHnhAWVlZatOmjb777rtr3rVWWliMGz0MCEpLS5O3t7cuXLjA95oBd5hdu3YpPDxcn8RvsPuzZg7uSdTAzq20c+dONWnSxK59w74uX76so0ePKiQkRK6urpLKzpOqza6g9y7Prfz+5goRAAAFqF69ug4cOGD67zIzCwIRAADXUL16dQKKSXCXGQAAMD0CEQAAMD0CEQAAMD0CEQAAMD0CEQAAMD0CEQAAMD0CEQAAMD2eQwQAwDUkJSXxYMabdOzYMYWEhGj37t1q3LhxSZdzywhEAAAUICkpSXXq1tGljEvFdkw3dzf9fODnWwpF/fr10+effy5JcnJyUvXq1dWnTx+NHj1a5coV36/5oKAgJScnq3LlysV2THsiEAEAUIAzZ87oUsYlPTz6YfkG+xb58U4fP61FbyzSmTNnbvkqUefOnRUbG6vMzEx99913io6OlpOTk0aNGmXTLisrS87OzvYs28rR0VEBAQFF0ndxYA4RAADX4Rvsqyq1qxT5cjuhy8XFRQEBAQoODtbzzz+vyMhILVmyRP369VOPHj00adIkBQYGKiwsTJJ04sQJPfroo6pQoYJ8fHzUvXt3HTt2zNpf3uveeOMN+fv7q0KFCpowYYJycnI0fPhw+fj4qFq1aoqNjbW+5tixY7JYLEpMTJQkxcXFqUKFCjZ1Ll68WBaLxbo+fvx4NW7cWLNnz1b16tVVvnx5vfDCC7py5YqmTJmigIAA+fn5adKkSYUem5vFFSIAAO4wbm5uOnv2rCRp1apV8vLyUkJCgiQpOztbUVFRioiI0A8//KBy5crp9ddfV+fOnbVnzx7rFaTVq1erWrVqWr9+vTZu3KgBAwZo06ZNatOmjbZu3ar58+fr2Wef1f33369q1aoVutYjR47o+++/V3x8vI4cOaJHHnlEv/76q2rXrq1169Zp06ZNevrppxUZGanmzZvf/uBcA1eIAAC4QxiGoZUrV2r58uXq0KGDJMnDw0Offvqp6tevr/r162v+/PnKzc3Vp59+qoYNG6pu3bqKjY1VUlKS1q5da+3Lx8dHM2bMUFhYmJ5++mmFhYUpIyNDo0ePVq1atTRq1Cg5Oztrw4YNt1Vzbm6uZs+erXr16unBBx9U+/btdfDgQU2bNk1hYWHq37+/wsLCtGbNmts6zo1whQgAgDJu6dKlKl++vLKzs5Wbm6snnnhC48ePV3R0tBo2bGgzb+jHH3/U4cOH5enpadPH5cuXdeTIEet6/fr15eDwv+sm/v7+atCggXXd0dFRlSpVUmpq6m3VXqNGDZta/P395ejomO/Yt3ucGyEQAQBQxrVv314ffvihnJ2dFRgYaHN3mYeHh03bixcvKjw8XHPmzMnXj6/v/+YxOTk52eyzWCwFbsvNzS2wJgcHBxmGYbMtOzs7X7vbPY69EIgAACjjPDw8FBoaelNtmzRpovnz58vPz09eXl5FVpOvr6/++OMPpaenW0NZ3oTr0og5RAAAmEjv3r1VuXJlde/eXT/88IOOHj2qtWvX6qWXXtJvv/1mt+M0b95c7u7uGj16tI4cOaK5c+cqLi7Obv3bG1eIAAC4jtPHT99Rx3F3d9f69es1cuRI9ezZU3/88YeqVq2qjh072vWKkY+Pj7766isNHz5cn3zyiTp27Kjx48dr0KBBdjuGPVmMqz/gQz5paWny9vbWhQsXivTyIoDit2vXLoWHh+uT+A0Ka9TYrn0f3JOogZ1baefOnWrSpIld+4Z9Xb58WUePHlVISIhcXV0llZ0nVZtdQe9dnlv5/c0VIgAAClC9enX9fOBnvsvMJAhEAABcQ/Xq1QkoJsGkagAAYHoEIgAAYHoEIgAA/j/uMyp77PWeEYgAAKbn6OgoScrKyirhSnCr8t6zvPewsJhUDQAwvXLlysnd3V2nT5+Wk5OTzfdoofTKzc3V6dOn5e7ubvN1JYVBIAIAmJ7FYlGVKlV09OhRHT9+vKTLwS1wcHBQ9erVZbFYbqsfAhEAAJKcnZ1Vq1YtPjYrY5ydne1yRY9ABADA/+fg4JDvaccwBz4kBQAApkcgAgAApkcgAgAApkcgAgAApkcgAgAApkcgAgAApkcgAgAApkcgAgAApkcgAgAApkcgAgAApkcgAgAApkcgAgAApkcgAgAApkcgAgAApkcgAgAApkcgAgAApkcgAgAApkcgAgAApleigWjy5Mlq1qyZPD095efnpx49eujgwYM2bS5fvqzo6GhVqlRJ5cuXV69evXTq1CmbNklJSerWrZvc3d3l5+en4cOHKycnx6bN2rVr1aRJE7m4uCg0NFRxcXFFfXoAAKCMKNFAtG7dOkVHR2vLli1KSEhQdna2OnXqpPT0dGuboUOH6j//+Y++/vprrVu3TidPnlTPnj2t+69cuaJu3bopKytLmzZt0ueff664uDiNHTvW2ubo0aPq1q2b2rdvr8TERA0ZMkTPPPOMli9fXqznCwAASqdyJXnw+Ph4m/W4uDj5+flp586datOmjS5cuKDPPvtMc+fOVYcOHSRJsbGxqlu3rrZs2aIWLVpoxYoV2r9/v1auXCl/f381btxYEydO1MiRIzV+/Hg5Oztr1qxZCgkJ0TvvvCNJqlu3rjZs2KB3331XUVFR+erKzMxUZmamdT0tLa0IRwEAAJS0UjWH6MKFC5IkHx8fSdLOnTuVnZ2tyMhIa5s6deqoevXq2rx5syRp8+bNatiwofz9/a1toqKilJaWpn379lnb/LWPvDZ5fVxt8uTJ8vb2ti5BQUH2O0kAAFDqlJpAlJubqyFDhui+++5TgwYNJEkpKSlydnZWhQoVbNr6+/srJSXF2uavYShvf96+67VJS0vTpUuX8tUyatQoXbhwwbqcOHHCLucIAABKpxL9yOyvoqOj9dNPP2nDhg0lXYpcXFzk4uJS0mUAAIBiUiquEMXExGjp0qVas2aNqlWrZt0eEBCgrKwsnT9/3qb9qVOnFBAQYG1z9V1nees3auPl5SU3Nzd7nw4AAChjSjQQGYahmJgYLVq0SKtXr1ZISIjN/vDwcDk5OWnVqlXWbQcPHlRSUpIiIiIkSREREdq7d69SU1OtbRISEuTl5aV69epZ2/y1j7w2eX0AAABzK9GPzKKjozV37lx9++238vT0tM758fb2lpubm7y9vTVgwAANGzZMPj4+8vLy0osvvqiIiAi1aNFCktSpUyfVq1dPTz31lKZMmaKUlBS9+uqrio6Otn7s9dxzz+n999/XiBEj9PTTT2v16tVasGCBli1bVmLnDgAASo8SvUL04Ycf6sKFC2rXrp2qVKliXebPn29t8+677+qBBx5Qr1691KZNGwUEBGjhwoXW/Y6Ojlq6dKkcHR0VERGhJ598Un369NGECROsbUJCQrRs2TIlJCTo7rvv1jvvvKNPP/20wFvuAQCA+ZToFSLDMG7YxtXVVTNnztTMmTOv2SY4OFjffffddftp166ddu/efcs1AgCAO1+pmFQNAABQkghEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9AhEAADA9MoV9oXp6elat26dkpKSlJWVZbPvpZdeuu3CAAAAikuhAtHu3bvVtWtXZWRkKD09XT4+Pjpz5ozc3d3l5+dHIAIAAGVKoT4yGzp0qB588EH9/vvvcnNz05YtW3T8+HGFh4fr7bfftneNAAAARapQgSgxMVH/93//JwcHBzk6OiozM1NBQUGaMmWKRo8ebe8aAQAAilShApGTk5McHP58qZ+fn5KSkiRJ3t7eOnHihP2qAwAAKAaFmkN0zz33aPv27apVq5batm2rsWPH6syZM/ryyy/VoEEDe9cIAABQpAp1heiNN95QlSpVJEmTJk1SxYoV9fzzz+v06dP6+OOP7VogAABAUSvUFaKmTZta/+zn56f4+Hi7FQQAAFDceDAjAAAwvZu+QtSkSROtWrVKFStW1D333COLxXLNtrt27bJLcQAAAMXhpq8Qde/eXS4uLpKkHj16qHv37tdcbtb69ev14IMPKjAwUBaLRYsXL7bZ369fP1ksFpulc+fONm3OnTun3r17y8vLSxUqVNCAAQN08eJFmzZ79uxR69at5erqan08AAAAQJ6bvkI0bty4Av98O9LT03X33Xfr6aefVs+ePQts07lzZ8XGxlrX80JZnt69eys5OVkJCQnKzs5W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gQgAAJgegQgAAJgegQhWb775piwWi4YMGSJJOnbsmCwWS4HL119/fd2+Dhw4oIceekje3t7y8PBQs2bNlJSUVAxnUTbYa6wvXryomJgYVatWTW5ubtYvLsafrh5nSUpJSdFTTz2lgIAAeXh4qEmTJvrmm29u2NfMmTNVo0YNubq6qnnz5tq2bVsRVl622GucJ0+erGbNmsnT01N+fn7q0aOHDh48WMTVlx32/Hm+Xp9mRSCCJGn79u366KOPbL5YLygoSMnJyTbLa6+9pvLly6tLly7X7OvIkSNq1aqV6tSpo7Vr12rPnj0aM2aMXF1di+NUSj17jvWwYcMUHx+vr776SgcOHNCQIUMUExOjJUuWFMeplGoFjbMk9enTRwcPHtSSJUu0d+9e9ezZU48++qh27959zb7mz5+vYcOGady4cdq1a5fuvvtuRUVFKTU1tahPo9Sz5zivW7dO0dHR2rJlixISEpSdna1OnTopPT29qE+j1LPnON+oT9Oyz9enoiz7448/jFq1ahkJCQlG27ZtjcGDB1+zbePGjY2nn376uv099thjxpNPPmnnKu8M9h7r+vXrGxMmTLDZ1qRJE+OVV16xR7ll1vXG2cPDw/jiiy9s2vv4+BiffPLJNfu79957jejoaOv6lStXjMDAQGPy5Ml2r70ssfc4Xy01NdWQZKxbt85eJZdJRTHOt/JvkVlwhQiKjo5Wt27dFBkZed12O3fuVGJiogYMGHDNNrm5uVq2bJlq166tqKgo+fn5qXnz5lq8eLGdqy6b7DnWktSyZUstWbJE//3vf2UYhtasWaNffvlFnTp1smfZZc71xrlly5aaP3++zp07p9zcXM2bN0+XL19Wu3btCuwrKytLO3futOnLwcFBkZGR2rx5c1GdQplgz3EuyIULFyRJPj4+9iq5TCqKcb7Zf4vMxDRPqkbB5s2bp127dmn79u03bPvZZ5+pbt26atmy5TXbpKam6uLFi3rzzTf1+uuv66233lJ8fLx69uypNWvWqG3btvYsv0yx91hL0nvvvadBgwapWrVqKleunBwcHPTJJ5+oTZs29iq7zLnROC9YsECPPfaYKlWqpHLlysnd3V2LFi1SaGhoge3PnDmjK1eu5Huqvb+/v37++We7119W2Hucr5abm6shQ4bovvvuU4MGDexZeplSFON8K/8WmQmByMROnDihwYMHKyEh4Ybzey5duqS5c+dqzJgx122Xm5srSerevbuGDh0qSWrcuLE2bdqkWbNmmTYQFcVYS38Goi1btmjJkiUKDg7W+vXrFR0drcDAQFP+z+9mxnnMmDE6f/68Vq5cqcqVK2vx4sV69NFH9cMPP6hhw4bFXHHZVBzjHB0drZ9++kkbNmywd/llRlGM8638W2Q6Jf2ZHUrOokWLDEmGo6OjdZFkWCwWw9HR0cjJybG2/eKLLwwnJycjNTX1un1mZmYa5cqVMyZOnGizfcSIEUbLli2L5DzKgqIY64yMDMPJyclYunSpzfYBAwYYUVFRRXIepd2Nxvnw4cOGJOOnn36yeV3Hjh2NZ599tsA+MzMzDUdHR2PRokU22/v06WM89NBDRXUqpVpRjPNfRUdHG9WqVTN+/fXXojqFMqEoxvlW/i0yG64QmVjHjh21d+9em239+/dXnTp1NHLkSDk6Olq3f/bZZ3rooYfk6+t73T6dnZ3VrFmzfLfK/vLLLwoODrZf8WVMUYx1dna2srOz5eBgOxXQ0dHReqXObG40zhkZGZJ0S2Pm7Oys8PBwrVq1Sj169JD055XQVatWKSYmxv4nUQYUxThLkmEYevHFF7Vo0SKtXbtWISEh9i++DCmKcb6Vf4tMp6QTGUqXgu42OHTokGGxWIzvv/++wNeEhYUZCxcutK4vXLjQcHJyMj7++GPj0KFDxnvvvWc4OjoaP/zwQ1GWXubYY6zbtm1r1K9f31izZo3x66+/GrGxsYarq6vxwQcfFGXpZcpfxzkrK8sIDQ01WrdubWzdutU4fPiw8fbbbxsWi8VYtmyZ9TUdOnQw3nvvPev6vHnzDBcXFyMuLs7Yv3+/MWjQIKNChQpGSkpKcZ9OqWWPcX7++ecNb29vY+3atUZycrJ1ycjIKO7TKbXsMc7X69PMuEKEG5o9e7aqVat2zTuXDh48aL0bRJIefvhhzZo1S5MnT9ZLL72ksLAwffPNN2rVqlVxlVxm3epYz5s3T6NGjVLv3r117tw5BQcHa9KkSXruueeKq+QyxcnJSd99951efvllPfjgg7p48aJCQ0P1+eefq2vXrtZ2R44c0ZkzZ6zrjz32mE6fPq2xY8cqJSVFjRs3Vnx8fL6J1vhTYcf5ww8/lKR8d0jFxsaqX79+xVF6mVLYcUbBLIZhGCVdBAAAQEniOUQAAMD0CEQAAMD0CEQAAMD0CEQAAMD0CEQAAMD0CEQAAMD0CEQAAMD0CEQAAMD0CEQATMlisWjx4sW31cf48ePVuHFju9QDoGQRiACUef369ZPFYsm3dO7cuaRLA1BG8F1mAO4InTt3VmxsrM02FxeXEqoGQFnDFSIAdwQXFxcFBATYLBUrVpQkHTp0SG3atJGrq6vq1aunhISEfK8fOXKkateuLXd3d911110aM2aMsrOzbdq8+eab8vf3l6enpwYMGKDLly8Xy7kBKHpcIQJwR8vNzVXPnj3l7++vrVu36sKFCxoyZEi+dp6enoqLi1NgYKD27t2rgQMHytPTUyNGjJAkLViwQOPHj9fMmTPVqlUrffnll5oxY4buuuuuYj4jAEWBb7sHUOb169dPX331lVxdXW22jx49Wk2bNlW3bt10/PhxBQYGSpLi4+PVpUsXLVq0SD169Ciwz7ffflvz5s3Tjh07JEktW7bUPffco5kzZ1rbtGjRQpcvX1ZiYmKRnBeA4sMVIgB3hPbt2+vDDz+02ebj46Mvv/xSQUFB1jAkSREREfleP3/+fM2YMUNHjhzRxYsXlZOTIy8vL+v+AwcO6LnnnrN5TUREhNasWWPnMwFQEghEAO4IHh4eCg0NLdRrN2/erN69e+u1115TVFSUvL29NW/ePL3zzjt2rhJAacWkagB3tLp16+rEiRNKTk62btuyZYtNm02bNik4OFivvPKKmjZtqlq1aun48eP5+tm6davNtqv7AVB2cYUIwB0hMzNTKSkpNtvKlSunyMhI1a5dW3379tU///lPpaWl6ZVXXrFpV6tWLSUlJWnevHlq1qyZli1bpkWLFtm0GTx4sPr166emTZvqvvvu05w5c7Rv3z4mVQN3CK4QAbgjxMfHq0qVKjZLq1at5ODgoEWLFunSpUu699579cwzz2jSpEk2r33ooYc0dOhQxcTEqHHjxtq0aZPGjBlj0+axxx7TmDFjNGLECIWHh+v48eN6/vnni/MUARQh7jIDAACmxxUiAABgegQiAABgegQiAABgegQiAABgegQiAABgegQiAABgegQiAABgegQiAABgegQiAABgegQiAABgegQiAABgev8PBIfMGtuHKigAAAAASUVORK5CYII=",
      "text/plain": [
       "<Figure size 640x480 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "\n",
    "# Histograma para visualizar la edad (age)\n",
    "\n",
    "sns.histplot(data=user_profile, x='age', hue='plan', palette=['skyblue','green'], bins=20)\n",
    "plt.title('Distribución de edad por plan')\n",
    "plt.xlabel('Edad')\n",
    "plt.ylabel('Frecuencia')\n",
    "plt.show()\n"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "7a8656e4",
   "metadata": {},
   "source": [
    "💡Insights: \n",
    "- Distribución ...La distribución de edad es relativamente simétrica, con mayor concentración en adultos jóvenes y de mediana edad. No se observa una diferencia marcada entre los usuarios de plan Básico y Premium, lo que sugiere que la edad no influye de forma clara en la elección del plan."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 42,
   "id": "84976c39",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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GzcOTZs2RUJCAv78808sWLAAABAbG4sjR45gxIgRaNKkCa5fv46wsDD06tULFy9ehIGBQZX2efny5Rg/fjyMjIykZKuiY79mzRp88MEHeOmllzBx4kRcu3YNr776KszNzWFvby+Vy8rKwg8//IC3334bY8eORXZ2NtasWQMfHx8cP35cuiUrhMCQIUPw999/48MPP4Srqyu2bt0KX1/fCo9JiZSUFPTu3RuFhYX49NNPYWhoiO+++67Mc/Cnn36Cr68vfHx88OWXXyI3NxdhYWHo1q0bTp8+XalO7uPHj4dCocCcOXNw9OhRfPfddzAzM8ORI0fg4OCAhQsXYufOnVi8eDHatGmD0aNHS+tW5dy4evUqXn/9dfj7+8PX1xdr166Fn58fOnbsiNatWwN4/M9QaGgo3n//fXTu3BlZWVk4ceIETp06hZdffhnA43+Orl27hjFjxkChUODChQv47rvvcOHCBRw9erTCBywq217Dhw/HhQsXMH78eDRt2hRpaWmIiopCUlLSM9u0qKgI/fv3R5cuXbBo0SLs3r0bs2fPRmFhIebOnQvg8Tny6quvYv/+/fD394eHhwf27NmDqVOn4t9//8WyZcuUYu7btw+bNm1CUFAQLC0tn1mHK1eu4K233sKHH34IX19fhIeH44033sDu3buldizL5s2bkZubi4CAAFhYWOD48eNYuXIlbt26hc2bN5faTx8fH3h6euKrr75CdHQ0lixZAicnJwQEBFRYP40mqMELDw8XAERsbGy5ZUxNTUX79u2l6dmzZ4snT49ly5YJAOLOnTvlxoiNjRUARHh4eKllPXv2FADE6tWry1zWs2dPaXr//v0CgGjcuLHIysqS5m/atEkAEF9//bU0z9HRUfj6+j4z5tq1awUAsXTp0lJli4uLpZ8BiNmzZ0vTQ4cOFbq6uiIhIUGad/v2bWFsbCx69OghzStpY29vb6V4kyZNEtra2iIjI6PUdp/k4eEhbG1tlcpFRkYKAMLR0VGa99dffwkAYsOGDUrr7969u8z5T+vRo4cwNjYWN27cKLcNcnNzS60XExMjAIgff/yxWvvcunVrpeNRouRY79+/XwghRH5+vrC2thYeHh4iLy9PKvfdd98JAEoxCgsLlcoIIcT9+/eFjY2NeO+996R527ZtEwDEokWLlNbt3r17uefrkyZOnCgAiGPHjknz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ZbOzs8WkSZOEnZ2d0NHRES1atBCLFy9WGq5AiMffEYGBgc/cdomStt6zZ49o27atkMvlolWrVmLz5s2l2uHp34WLFy8Kb29vYWRkJCwtLcXYsWPFP//8U+q8Le9cePp7nEqTCaHm3otERKTx1qxZg/fffx83b94scxBQdfDz88Nvv/2mdGWwpjVt2hRt2rTB9u3ba60OVD72CSIiIrVLTk6GTCaDubl5bVeFSMI+QUREpDapqan47bffsHr1anh5eZV67QpRbeKVICIiUpu4uDhMnToVzs7O0guVieoK9gkiIiIijcQrQURERKSRmAQRERGRRmLHaDweVv327dswNjZW+asfiIiISD2EEMjOzoadnR20tKp+XYdJEB4Paa6Kd0ARERFRzavu+FNMggDphXk3b95UybugiIiISP2ysrJgb2+v9OLbqmAShP+94drExIRJEBERUT1T3a4s7BhNREREGolJEBEREWkkJkFERESkkdgniIiINEZRUREKCgpquxpUSTo6OtDW1lZbfCZBRETU4AkhkJKSgoyMjNquClWRmZkZFAqFWsbxYxJEREQNXkkCZG1tDQMDAw6MWw8IIZCbm4u0tDQAgK2trcq3wSSIiIgatKKiIikBsrCwqO3qUBXo6+sDANLS0mBtba3yW2PsGE1ERA1aSR8gAwODWq4JVUfJcVNHXy4mQUREpBF4C6x+UudxYxJEREREGolJEBERUT3RtGlTLF++vLar0WAwCSIiIiKNxCSIiIiINBKTICIiojqiV69eCAoKQlBQEExNTWFpaYmZM2dCCFFm+aVLl8Ld3R2Ghoawt7fHRx99hAcPHkjLIyIiYGZmhj179sDV1RVGRkbo378/kpOTa2qX6jSOE0TUACUlJSE9PV0tsS0tLeHg4KCW2EQErFu3Dv7+/jh+/DhOnDiBcePGwcHBAWPHji1VVktLCytWrECzZs1w7do1fPTRR5g2bRq++eYbqUxubi6++uor/PTTT9DS0sK7776LTz75BBs2bKjJ3aqTmAQRNTBJSUlwdXVFbm6uWuIbGBggLi6OiRCRmtjb22PZsmWQyWRwcXHBuXPnsGzZsjKToIkTJ0o/N23aFPPnz8eHH36olAQVFBRg9erVcHJyAgAEBQVh7ty5at+P+oBJEFEDk56ejtzcXMz4zxo4OruoNPaNq/GYH+SP9PR0JkFE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WnRZHB0dsXPnTlVWi4iIiDRAnesTRERERFQTmAQRERGRRmISRERERBqJSRARERFpJCZBREREpJGYBBEREZFGYhJEREREGolJEBEREWkkJkFERESkkWp1xGgiqp/i4uJUHtPS0hIODg4qj0tEVB4mQURUaXfTUgCZDO+++67KYxsYGCAuLo6JEBHVGCZBRFRpDzIzASEQNG8J2r3oqbK4N67GY36QP9LT05kEEVGNYRJERFXWuJkTXNp61HY1iIieCztGExERkUZiEkREREQaiUkQERERaSQmQURERKSRmAQRERGRRmISRERERBqJSRARERFpJCZBREREpJGYBBEREZFGYhJEREREGolJEBEREWkkJkFERESkkZgEERERkUZiEkREREQaiUkQERERaSQmQURERKSRmAQRERGRRqrVJCgsLAxt27aFiYkJTExM4OXlhV27dknLHz16hMDAQFhYWMDIyAjDhw9HamqqUoykpCQMGjQIBgYGsLa2xtSpU1FYWFjTu0JERET1TK0mQU2aNMEXX3yBkydP4sSJE+jTpw+GDBmCCxcuAAAmTZqEP//8E5s3b8bBgwdx+/ZtDBs2TFq/qKgIgwYNQn5+Po4cOYJ169YhIiICs2bNqq1dIiIionqiUW1ufPDgwUrTCxYsQFhYGI4ePYomTZpgzZo12LhxI/r06QMACA8Ph6urK44ePYouXbogMjISFy9eRHR0NGxsbODh4YF58+Zh+vTpCAkJga6ubm3sFhEREdUDdaZPUFFREX755Rfk5OTAy8sLJ0+eREFBAby9vaUyrVq1goODA2JiYgAAMTExcHd3h42NjVTGx8cHWVlZ0tWksuTl5SErK0vpQ0RERJql1pOgc+fOwcjICHK5HB9++CG2bt0KNzc3pKSkQFdXF2ZmZkrlbWxskJKSAgBISUlRSoBKlpcsK09oaChMTU2lj729vWp3ioiIiOq8Wk+CXFxccObMGRw7dgwBAQHw9fXFxYsX1brN4OBgZGZmSp+bN2+qdXtERERU99RqnyAA0NXVhbOzMwCgY8eOiI2Nxddff4233noL+fn5yMjIULoalJqaCoVCAQBQKBQ4fvy4UrySp8dKypRFLpdDLpereE+IiIioPqn1K0FPKy4uRl5eHjp27AgdHR3s3btXWhYfH4+kpCR4eXkBALy8vHDu3DmkpaVJZaKiomBiYgI3N7carzsRERHVH7V6JSg4OBgDBgyAg4MDsrOzsXHjRhw4cAB79uyBqakp/P39MXnyZJibm8PExATjx4+Hl5cXunTpAgDo168f3NzcMGrUKCxatAgpKSmYMWMGAgMDeaWnjkpKSkJ6erpKY8bFxak0HhERaYZaTYLS0tIwevRoJCcnw9TUFG3btsWePXvw8ssvAwCWLVsGLS0tDB8+HHl5efDx8cE333wjra+trY3t27cjICAAXl5eMDQ0hK+vL+bOnVtbu0QVSEpKgqurK3Jzc9US/8GDB2qJS0REDVOtJkFr1qypcLmenh5WrVqFVatWlVvG0dERO3fuVHXVSA3S09ORm5uLGf9ZA0dnF5XFPbo/Emu+nItHjx6pLCYRETV8td4xmjSPo7MLXNp6qCzejSvxKotFRESao851jCYiIiKqCUyCiIiISCMxCSIiIiKNxCSIiIiINBKTICIiItJITIKIiIhIIzEJIiIiIo3EJIiIiIg0EpMgIiIi0khMgoiIiEgjMQkiIiIijcQkiIiIiDQSkyAiIiLSSEyCiIiISCMxCSIiIiKNxCSIiIiINBKTICIiItJITIKIiIhIIzEJIiIiIo3EJIiIiIg0EpMgIiIi0khMgoiIiEgjMQkiIiIijdSouivm5OTg4MGDSEpKQn5+vtKyjz/++LkrRkRERKRO1UqCTp8+jYEDByI3Nxc5OTkwNzdHeno6DAwMYG1tzSSIiIiI6rxq3Q6bNGkSBg8ejPv370NfXx9Hjx7FjRs30LFjR3z11VeqriMRERGRylUrCTpz5gymTJkCLS0taGtrIy8vD/b29li0aBE+++wzVdeRiIiISOWqlQTp6OhAS+vxqtbW1khKSgIAmJqa4ubNm6qrHREREZGaVKtPUPv27REbG4sWLVqgZ8+emDVrFtLT0/HTTz+hTZs2qq4jERERkcpV60rQwoULYWtrCwBYsGABXnjhBQQEBODOnTv47rvvVFpBIiIiInWoVhLUqVMn9O7dG8Dj22G7d+9GVlYWTp48iXbt2lU6TmhoKF588UUYGxvD2toaQ4cORXx8vFKZXr16QSaTKX0+/PBDpTJJSUkYNGiQ9HTa1KlTUVhYWJ1dIyIiIg1R7XGCVOHgwYMIDAzEiy++iMLCQnz22Wfo168fLl68CENDQ6nc2LFjMXfuXGnawMBA+rmoqAiDBg2CQqHAkSNHkJycjNGjR0NHRwcLFy6s0f0hIiKi+qPSSVCHDh2wd+9evPDCC2jfvj1kMlm5ZU+dOlWpmLt371aajoiIgLW1NU6ePIkePXpI8w0MDKBQKMqMERkZiYsXLyI6Oho2Njbw8PDAvHnzMH36dISEhEBXV7dSdSEiIiLNUukkaMiQIZDL5QCAoUOHqqUymZmZAABzc3Ol+Rs2bMD69euhUCgwePBgzJw5U7oaFBMTA3d3d9jY2EjlfXx8EBAQgAsXLqB9+/altpOXl4e8vDxpOisrSx27Q0RERHVYpZOg2bNnl/mzqhQXF2PixIno2rWr0hNm77zzDhwdHWFnZ4ezZ89i+vTpiI+Px5YtWwAAKSkpSgkQAGk6JSWlzG2FhoZizpw5Kt8HIiIiqj+q1ScoNjYWxcXF8PT0VJp/7NgxaGtro1OnTlWOGRgYiPPnz+Pvv/9Wmj9u3DjpZ3d3d9ja2qJv375ISEiAk5NTdaqP4OBgTJ48WZrOysqCvb19tWIRERFR/VStp8MCAwPLHBTx33//RWBgYJXjBQUFYfv27di/fz+aNGlSYdmSxOvq1asAAIVCgdTUVKUyJdPl9SOSy+UwMTFR+hAREZFmqVYSdPHiRXTo0KHU/Pbt2+PixYuVjiOEQFBQELZu3Yp9+/ahWbNmz1znzJkzACCNU+Tl5YVz584hLS1NKhMVFQUTExO4ublVui5ERESkWap1O0wulyM1NRXNmzdXmp+cnIxGjSofMjAwEBs3bsQff/wBY2NjqQ+Pqakp9PX1kZCQgI0bN2LgwIGwsLDA2bNnMWnSJPTo0QNt27YFAPTr1w9ubm4YNWoUFi1ahJSUFMyYMQOBgYFSR24iIiKip1XrSlC/fv0QHBwsPc0FABkZGfjss8/w8ssvVzpOWFgYMjMz0atXL9ja2kqfX3/9FQCgq6uL6Oho9OvXD61atcKUKVMwfPhw/Pnnn1IMbW1tbN++Hdra2vDy8sK7776L0aNHK40rRERERPS0al0J+uqrr9CjRw84OjpKj6CfOXMGNjY2+OmnnyodRwhR4XJ7e3scPHjwmXEcHR2xc+fOSm+XiIiIqFpJUOPGjXH27Fls2LAB//zzD/T19TFmzBi8/fbb0NHRUXUdiYiIiFSu2q/NMDQ0VHp8nYiIiKg+qXYSdOXKFezfvx9paWkoLi5WWjZr1qznrhhRXZKRkYHk5LIH36yO9PR0lcUiIqLqqVYS9P333yMgIACWlpZQKBRK7xGTyWRMgqjByH2YCwDYt28fTpy7oLK4WXcej2WVnJyssphERFQ11UqC5s+fjwULFmD69Omqrg9RnZKflw8AaNzKDi29PFQW9/qZOMRueXyFiYiIake1kqD79+/jjTfeUHVdiOosXX05jC2MVRZPz0hPZbGIiKh6qjVO0BtvvIHIyEhV14WIiIioxlTrSpCzszNmzpyJo0ePwt3dvdRj8R9//LFKKkdERESkLtVKgr777jsYGRnh4MGDpQYzlMlkTIKIiIiozqtWEpSYmKjqehARERHVqGr1CSqRn5+P+Ph4FBYWqqo+RERERDWiWklQbm4u/P39YWBggNatWyMpKQkAMH78eHzxxRcqrSARERGROlQrCQoODsY///yDAwcOQE/vf4/6ent7S2+AJyIiIqrLqtUnaNu2bfj111/RpUsXpdGiW7dujYSEBJVVjqgqVP1qCwDIfpCt0nhERFR3VCsJunPnDqytrUvNz8nJUUqKiGqCul5tAQDJly8CAPu9ERE1QNVKgjp16oQdO3Zg/PjxACAlPj/88AO8vLxUVzuiSlDXqy0A4PjWO7i4HyguLlJpXCIiqn3VSoIWLlyIAQMG4OLFiygsLMTXX3+Nixcv4siRI6XGDSKqKap+tQUA6Mh1VRqPiIjqjmolQd26dcOZM2fwxRdfwN3dHZGRkejQoQNiYmLg7u6u6joSkYaIi4tTS1xLS0s4ODioJTYR1V/VSoIAwMnJCd9//70q60JEKpSeng4TFXcUV9db7++mpQAyGd599121xDcwMEBcXBwTISJSUq0kqGRcoPLwi4ao9iQnJwMAtmzZAhOrw6qN/f8dxR/+f2d0VXmQmQkIgaB5S9DuRU+Vxr5xNR7zg/yRnp7O7yYiUlKtJKhp06YVPgVWVMROpES1peRqTbP2zdDUw1WlsU/vzMTF/UDe/3dGV7XGzZzg0tZDLbGJiJ5WrSTo9OnTStMFBQU4ffo0li5digULFqikYkT0fPSM9FTeUVxXnx3FiajhqFYS1K5du1LzOnXqBDs7OyxevBjDhg177ooRERERqdNzvUD1aS4uLoiNjVVlSCIiIiK1qNaVoKysLKVpIQSSk5MREhKCFi1aqKRiREREROpUrSTIzMysVMdoIQTs7e3xyy+/qKRiREREROpUrSRo3759SkmQlpYWrKys4OzsjEaNqj30EBEREVGNqVbG0qtXLxVXg4iIiKhmVatjdGhoKNauXVtq/tq1a/Hll18+d6WIiIiI1K1aSdC3336LVq1alZrfunVrrF69+rkrRURERKRu1UqCUlJSYGtrW2q+lZWVNGR/ZYSGhuLFF1+EsbExrK2tMXToUMTHxyuVefToEQIDA2FhYQEjIyMMHz4cqampSmWSkpIwaNAgGBgYwNraGlOnTkVhYWF1do2IiIg0RLWSIHt7exw+XPqdRIcPH4adnV2l4xw8eBCBgYE4evQooqKiUFBQgH79+iEnJ0cqM2nSJPz555/YvHkzDh48iNu3bysNxlhUVIRBgwYhPz8fR44cwbp16xAREYFZs2ZVZ9eIiIhIQ1SrY/TYsWMxceJEFBQUoE+fPgCAvXv3Ytq0aZgyZUql4+zevVtpOiIiAtbW1jh58iR69OiBzMxMrFmzBhs3bpS2Ex4eDldXVxw9ehRdunRBZGQkLl68iOjoaNjY2MDDwwPz5s3D9OnTERISAl1dDvNPREREpVUrCZo6dSru3r2Ljz76CPn5j1+kqKenh+nTpyM4OLjalcnMzAQAmJubAwBOnjyJgoICeHt7S2VatWoFBwcHxMTEoEuXLoiJiYG7uztsbGykMj4+PggICMCFCxfQvn37UtvJy8tDXl6eNP304I9ERETU8FXrdphMJsOXX36JO3fu4OjRo/jnn39w796957oFVVxcjIkTJ6Jr165o06YNgMd9j3R1dWFmZqZU1sbGBikpKVKZJxOgkuUly8oSGhoKU1NT6WNvb1/tehMREVH99FzvDktJScG9e/fg5OQEuVwOIUS1YwUGBuL8+fM1MuJ0cHAwMjMzpc/NmzfVvk0iIiKqW6qVBN29exd9+/ZFy5YtMXDgQOmJMH9//yr1CSoRFBSE7du3Y//+/WjSpIk0X6FQID8/HxkZGUrlU1NToVAopDJPPy1WMl1S5mlyuRwmJiZKHyIiItIs1UqCJk2aBB0dHSQlJcHAwECa/9Zbb5Xq7FwRIQSCgoKwdetW7Nu3D82aNVNa3rFjR+jo6GDv3r3SvPj4eCQlJcHLywsA4OXlhXPnziEtLU0qExUVBRMTE7i5uVVn94iIiEgDVKtjdGRkJPbs2aN01QYAWrRogRs3blQ6TmBgIDZu3Ig//vgDxsbGUh8eU1NT6Ovrw9TUFP7+/pg8eTLMzc1hYmKC8ePHw8vLC126dAEA9OvXD25ubhg1ahQWLVqElJQUzJgxA4GBgZDL5dXZPSIiItIA1UqCcnJylK4Albh3716VEo+wsDAApd9FFh4eDj8/PwDAsmXLoKWlheHDhyMvLw8+Pj745ptvpLLa2trYvn07AgIC4OXlBUNDQ/j6+mLu3LlV3zEiIiLSGNVKgrp3744ff/wR8+bNA/D4abHi4mIsWrQIvXv3rnScynSk1tPTw6pVq7Bq1apyyzg6OmLnzp2V3i4RERFRtZKgRYsWoW/fvjhx4gTy8/Mxbdo0XLhwAffu3StzJGkiIiKiuqZaHaPbtGmDy5cvo1u3bhgyZAhycnIwbNgwnD59Gk5OTqquIxEREZHKVflKUEFBAfr374/Vq1fj888/V0ediIiIiNSuyleCdHR0cPbsWXXUhYiIiKjGVOt22Lvvvos1a9aoui5ERERENaZaHaMLCwuxdu1aREdHo2PHjjA0NFRavnTpUpVUjoiIiEhdqpQEXbt2DU2bNsX58+fRoUMHAMDly5eVyshkMtXVjoiIiEhNqpQEtWjRAsnJydi/fz+Ax6/JWLFiRam3uBMRERHVdVXqE/T04Ia7du1CTk6OSitEREREVBOq1SeoRGVGfCaihic7OxvJySkqi5eRkaGyWERElVWlJEgmk5Xq88M+QESao6igEAAQGxuL+OtJKoubfPkiAODhw1yVxSQiepYqJUFCCPj5+UkvSX306BE+/PDDUk+HbdmyRXU1JKI6o6iwGABg1dwSbXp2VFnc0zszcXE/kJeXr7KYRETPUqUkyNfXV2n63XffVWlliKh+0NXXhbGFsUrjERHVtColQeHh4eqqBxEREVGNqtaI0URERET1HZMgIiIi0khMgoiIiEgjMQkiIiIijcQkiIiIiDTSc40YTVQd6enpMFHhaMPZD7JVFouIiDQHkyCqMcnJyQAeD6ZpYnVYdXH/f7ThwsJClcUkIqKGj0kQ1ZiS90M1a98MTT1cVRb3+NY7uLgfKC4uUllMIiJq+JgEUY3TM9JT6WjDOnKONkxERFXHjtFERESkkZgEERERkUZiEkREREQaiUkQERERaSQmQURERKSRmAQRERGRRmISRERERBqJSRARERFppFodLPHQoUNYvHgxTp48ieTkZGzduhVDhw6Vlvv5+WHdunVK6/j4+GD37t3S9L179zB+/Hj8+eef0NLSwvDhw/H111/DyMiopnajwUlKSkJ6errK4yYmJqo8JhERUXXVahKUk5ODdu3a4b333sOwYcPKLNO/f3+Eh4dL03K5XGn5yJEjkZycjKioKBQUFGDMmDEYN24cNm7cqNa6N1RJSUlwdXVFbm6u2rZRmFegtthERESVVatJ0IABAzBgwIAKy8jlcigUijKXxcXFYffu3YiNjUWnTp0AACtXrsTAgQPx1Vdfwc7Orsz18vLykJeXJ01nZWVVcw8anvT0dOTm5mLGf9bA0dlFpbGj/vgdm8KWobCA7/giIqLaV+ffHXbgwAFYW1vjhRdeQJ8+fTB//nxYWFgAAGJiYmBmZiYlQADg7e0NLS0tHDt2DK+99lqZMUNDQzFnzpwaqX995ejsApe2HiqN+U/sMZXGIyIieh51umN0//798eOPP2Lv3r348ssvcfDgQQwYMABFRY+vJKSkpMDa2lppnUaNGsHc3BwpKSnlxg0ODkZmZqb0uXnzplr3g4iIiOqeOn0laMSIEdLP7u7uaNu2LZycnHDgwAH07du32nHlcnmpvkVERESkWer0laCnNW/eHJaWlrh69SoAQKFQIC0tTalMYWEh7t27V24/IiIiIiKgjl8JetqtW7dw9+5d2NraAgC8vLyQkZGBkydPomPHjgCAffv2obi4GJ6enrVZVSKqY+Li4lQe09LSEg4ODiqPS0Q1o1aToAcPHkhXdYDH48icOXMG5ubmMDc3x5w5czB8+HAoFAokJCRg2rRpcHZ2ho+PDwDA1dUV/fv3x9ixY7F69WoUFBQgKCgII0aMKPfJMCLSLHfTUgCZDO+++67KYxsYGCAuLo6JEFE9VatJ0IkTJ9C7d29pevLkyQAAX19fhIWF4ezZs1i3bh0yMjJgZ2eHfv36Yd68eUr9eTZs2ICgoCD07dtXGixxxYoVNb4vRFQ3PcjMBIRA0LwlaPei6q4Q37gaj/lB/khPT2cSRFRP1WoS1KtXLwghyl2+Z8+eZ8YwNzfnwIhE9EyNmzmpfNgHIqrf6lXHaCIiIiJVYRJEREREGolJEBEREWkkJkFERESkkZgEERERkUZiEkREREQaiUkQERERaSQmQURERKSRmAQRERGRRmISRERERBqJSRARERFpJCZBREREpJGYBBEREZFGYhJEREREGolJEBEREWkkJkFERESkkZgEERERkUZiEkREREQaiUkQERERaSQmQURERKSRmAQRERGRRmpU2xUgIiqRnZ2N5OQUlcbMyMhQaTwiajiYBBFRrSsqKAQAxMbGIv56kkpjJ1++CAB4+DBXpXGJqP5jEkREta6osBgAYNXcEm16dlRp7NM7M3FxP5CXl6/SuERU/zEJIqI6Q1dfF8YWxiqPSURUFnaMJiIiIo3EJIiIiIg0EpMgIiIi0khMgoiIiEgjMQkiIiIijVSrSdChQ4cwePBg2NnZQSaTYdu2bUrLhRCYNWsWbG1toa+vD29vb1y5ckWpzL179zBy5EiYmJjAzMwM/v7+ePDgQQ3uBREREdVHtZoE5eTkoF27dli1alWZyxctWoQVK1Zg9erVOHbsGAwNDeHj44NHjx5JZUaOHIkLFy4gKioK27dvx6FDhzBu3Lia2gUiIiKqp2p1nKABAwZgwIABZS4TQmD58uWYMWMGhgwZAgD48ccfYWNjg23btmHEiBGIi4vD7t27ERsbi06dOgEAVq5ciYEDB+Krr76CnZ1dje0LERER1S91tk9QYmIiUlJS4O3tLc0zNTWFp6cnYmJiAAAxMTEwMzOTEiAA8Pb2hpaWFo4dO1Zu7Ly8PGRlZSl9iIiISLPU2SQoJeXxSxRtbGyU5tvY2EjLUlJSYG1trbS8UaNGMDc3l8qUJTQ0FKamptLH3t5exbUnIiKiuq7OJkHqFBwcjMzMTOlz8+bN2q4SERER1bA6mwQpFAoAQGpqqtL81NRUaZlCoUBaWprS8sLCQty7d08qUxa5XA4TExOlDxEREWmWOpsENWvWDAqFAnv37pXmZWVl4dixY/Dy8gIAeHl5ISMjAydPnpTK7Nu3D8XFxfD09KzxOhMREVH9UatPhz148ABXr16VphMTE3HmzBmYm5vDwcEBEydOxPz589GiRQs0a9YMM2fOhJ2dHYYOHQoAcHV1Rf/+/TF27FisXr0aBQUFCAoKwogRI/hkGBEREVWoVpOgEydOoHfv3tL05MmTAQC+vr6IiIjAtGnTkJOTg3HjxiEjIwPdunXD7t27oaenJ62zYcMGBAUFoW/fvtDS0sLw4cOxYsWKGt8XIiIiql9qNQnq1asXhBDlLpfJZJg7dy7mzp1bbhlzc3Ns3LhRHdUjIiKiBqzO9gkiIiIiUicmQURERKSRmAQRERGRRmISRERERBqJSRARERFpJCZBREREpJGYBBEREZFGYhJEREREGolJEBEREWmkWh0xmuqu9PR0mCSnqDRm9oNslcYjIiJ6HkyCSElycjIAYMuWLTCxOqza2JcvAgAKCwtVGpeIiKg6mASRkoyMDABAs/bN0NTDVaWxj2+9g4v7geLiIpXGJSIiqg4mQVQmPSM9GFsYqzSmjlxXpfGIiIieB5MgItII2dnZSFZhP7f09HSVxSKi2sEkiIgatKKCx33QYmNjEX89SWVxs+6kAvhfPzoiqn+YBBFRg1ZUWAwAsGpuiTY9O6os7vUzcYjdApw+fRq2trYqi1vC0tISDg4OKo9LRP/DJIiINIKuvq5K+7kVFjwCZDLMnDkTM2fOVFncEgYGBoiLi2MiRKRGTIKIiKoh70EOIAR8p4egW29vlca+cTUe84P8kZ6eziSISI2YBBERPQeFfVO4tPWo7WoQUTXwtRlERESkkZgEERERkUZiEkREREQaiUkQERERaSQmQURERKSR+HRYPZaUlKTyofsTExNVGo+IiKiuYhJUTyUlJcHV1RW5ublqiV+YV6CWuERERHUFk6B6Kj09Hbm5uZjxnzVwdHZRWdyoP37HprBlKCwoUllMIiKiuohJUD3n6Oyi0oHa/ok9prJYREREdRk7RhMREZFGYhJEREREGolJEBEREWmkOp0EhYSEQCaTKX1atWolLX/06BECAwNhYWEBIyMjDB8+HKmpqbVYYyIiIqov6nQSBACtW7dGcnKy9Pn777+lZZMmTcKff/6JzZs34+DBg7h9+zaGDRtWi7UlIiKi+qLOPx3WqFEjKBSKUvMzMzOxZs0abNy4EX369AEAhIeHw9XVFUePHkWXLl3KjZmXl4e8vDxpOisrS/UVJyIiojqtzl8JunLlCuzs7NC8eXOMHDkSSUlJAICTJ0+ioKAA3t7eUtlWrVrBwcEBMTExFcYMDQ2Fqamp9LG3t1frPhAREVHdU6eTIE9PT0RERGD37t0ICwtDYmIiunfvjuzsbKSkpEBXVxdmZmZK69jY2CAlJaXCuMHBwcjMzJQ+N2/eVONeEBERUV1Up2+HDRgwQPq5bdu28PT0hKOjIzZt2gR9ff1qx5XL5ZDL5aqoIhEREdVTdfpK0NPMzMzQsmVLXL16FQqFAvn5+cjIyFAqk5qaWmYfIiIiIqIn1ask6MGDB0hISICtrS06duwIHR0d7N27V1oeHx+PpKQkeHl51WItiYiIqD6o07fDPvnkEwwePBiOjo64ffs2Zs+eDW1tbbz99tswNTWFv78/Jk+eDHNzc5iYmGD8+PHw8vKq8MkwIiIiIqCOJ0G3bt3C22+/jbt378LKygrdunXD0aNHYWVlBQBYtmwZtLS0MHz4cOTl5cHHxwfffPNNLdeaiIiI6oM6nQT98ssvFS7X09PDqlWrsGrVqhqqERERETUU9apPEBEREZGqMAkiIiIijVSnb4cREdV12dnZSE6ueIDWqkpPTwcAxMXFqTQuAFhaWsLBwUHlcYnqIyZBRETVUFRQCACIjY1F/PUklca+cyMBkMnw7rvvqjQuABgYGCAuLo6JEBGYBBERVUtRYTEAwKq5Jdr07KjS2Kd3ZgJCwHd6CLr19n72CpV042o85gf5Iz09nUkQEZgEERE9F119XRhbGKs8JgAo7JvCpa2HSmMT0f+wYzQRERFpJCZBREREpJGYBBEREZFGYhJEREREGokdo+u59PR0mKhwjJLsB9kqi0VERFSXMQmqp5KTkwEAW7ZsgYnVYdXFvXwRAFBYWKiymERERHURk6B6KiMjAwDQrH0zNPVwVVnc41vv4OJ+oLi4SGUxiYiI6iImQfWcnpGeSsco0ZHrqiwWERFRXcaO0URERKSRmAQRERGRRuLtMCKiOkrVb6gveTs9ET3GJIiIqI5R1xvqs+6kAvjf06VEmo5JEBFRHaOuN9RfPxOH2C3/e7qUSNMxCSIiqqNU/YZ6PSM9lcUiagiYBBERkUokJSWppd+RpaUlHBwcVB6XiEkQERE9t6SkJLi6uiI3N1flsQ0MDBAXF8dEiFSOSRARET239PR05ObmYsZ/1sDR2UVlcW9cjcf8IH+kp6czCSKVYxKkZuq6PJyYmKjymEREz8vR2QUubT1quxpElcIkSI3UeXm4RGFegdpiExFVVXp6Okw4thHVE0yC1Ehdl4cBIOqP37EpbBkKC/iiUyKqfSVjD23ZsgUmVodVFpdjG5E6MQmqAeq4PPxP7DGVxiMieh4lYw81a98MTT1cVRaXYxuROjEJIiIildEz0lPL2EaJiYk4deqUyuKW4OP3mo1JUA1Q9T1yAMh+kK3SeEREdVFJv8eZM2di5syZKo+vb6CPS3GXmAhpKCZBaqSue+QAkHz5IgCgsLBQpXGJiOqSkn6Pnd7ohA7eHVQa+86NO9i6cCv++usvuLqq7hYeAOTl5UEul6s0ZglevVKdBpMErVq1CosXL0ZKSgratWuHlStXonPnzrVaJ3XdIweA41vv4OJ+oLiYHaOJiKrjYdZDQCbDu+++q/LYMpkMQgiVxwU4eKQqNYgk6Ndff8XkyZOxevVqeHp6Yvny5fDx8UF8fDysra1ru3oqv0cOADpyXZXGIyKqi3IyMgGZDCc2n8CJzSfUso2xMxagc7eeKot3dH8k1nw5F0HzlqDdi54qiwtw8EhVaxBJ0NKlSzF27FiMGTMGALB69Wrs2LEDa9euxaefflrLtSMiourKz80FhECv9/3g2rW9SmOf23sYf6/fBB1DE5hYKVQWV9/IFADQuJmT2gaOjIuLU0tcdd3Gq6u38Op9EpSfn4+TJ08iODhYmqelpQVvb2/ExMSUuU5eXh7y8vKk6czMTABAVlaWSutWMkjizfOXkP/okUpj30lMAgAkX76GCwaqO2HVFVedsetjnVOvXgcA7N+/X+WDaR479nj4BJ536o1dH+tcH8+7krZ4mJ2L+yn3VBYXAHLvP37AZN+uP3H0iOr6bd779zoA4OThQ3ik4na+fuVx8qOOW3jqpKenhxMnTsDe3l6lcUv+blf71qOo5/79918BQBw5ckRp/tSpU0Xnzp3LXGf27NkCAD/88MMPP/zw0wA+N2/erFYOUe+vBFVHcHAwJk+eLE0XFxfj3r17sLCwgEwmU9l2srKyYG9vj5s3b8LExERlcesjtsVjbIfH2A6PsR3+h23xGNvhscq2gxAC2dnZsLOzq9Z26n0SZGlpCW1tbaSmpirNT01NhUJR9j1euVxe6p6nmZmZuqoIExMTjT6Zn8S2eIzt8Bjb4TG2w/+wLR5jOzxWmXYwNTWtdnytaq9ZR+jq6qJjx47Yu3evNK+4uBh79+6Fl5dXLdaMiIiI6rJ6fyUIACZPngxfX1906tQJnTt3xvLly5GTkyM9LUZERET0tAaRBL311lu4c+cOZs2ahZSUFHh4eGD37t2wsbGp1XrJ5XLMnj1bbaOG1idsi8fYDo+xHR5jO/wP2+IxtsNjNdUOMiHUNKQlERERUR1W7/sEEREREVUHkyAiIiLSSEyCiIiISCMxCSIiIiKNxCToOa1atQpNmzaFnp4ePD09cfz48QrLb968Ga1atYKenh7c3d2xc+fOGqqp+oSGhuLFF1+EsbExrK2tMXToUMTHx1e4TkREBGQymdJHT0+vhmqsHiEhIaX2qVWrVhWu0xDPh6ZNm5ZqB5lMhsDAwDLLN6Rz4dChQxg8eDDs7Owgk8mwbds2peVCCMyaNQu2trbQ19eHt7c3rly58sy4Vf2eqW0VtUNBQQGmT58Od3d3GBoaws7ODqNHj8bt27crjFmd36/a9qzzwc/Pr9Q+9e/f/5lx69v5ADy7Lcr6zpDJZFi8eHG5MVVxTjAJeg6//vorJk+ejNmzZ+PUqVNo164dfHx8kJaWVmb5I0eO4O2334a/vz9Onz6NoUOHYujQoTh//nwN11y1Dh48iMDAQBw9ehRRUVEoKChAv379kJOTU+F6JiYmSE5Olj43btyooRqrT+vWrZX26e+//y63bEM9H2JjY5XaICoqCgDwxhtvlLtOQzkXcnJy0K5dO6xatarM5YsWLcKKFSuwevVqHDt2DIaGhvDx8cGjCl44WtXvmbqgonbIzc3FqVOnMHPmTJw6dQpbtmxBfHw8Xn311WfGrcrvV13wrPMBAPr376+0Tz///HOFMevj+QA8uy2ebIPk5GSsXbsWMpkMw4cPrzDuc58T1XrjGAkhhOjcubMIDAyUpouKioSdnZ0IDQ0ts/ybb74pBg0apDTP09NTfPDBB2qtZ01LS0sTAMTBgwfLLRMeHi5MTU1rrlI1YPbs2aJdu3aVLq8p58OECROEk5OTKC4uLnN5QzwXhBACgNi6das0XVxcLBQKhVi8eLE0LyMjQ8jlcvHzzz+XG6eq3zN1zdPtUJbjx48LAOLGjRvllqnq71ddU1Y7+Pr6iiFDhlQpTn0/H4So3DkxZMgQ0adPnwrLqOKc4JWgasrPz8fJkyfh7e0tzdPS0oK3tzdiYmLKXCcmJkapPAD4+PiUW76+yszMBACYm5tXWO7BgwdwdHSEvb09hgwZggsXLtRE9dTqypUrsLOzQ/PmzTFy5EgkJSWVW1YTzof8/HysX78e7733XoUvJ26I58LTEhMTkZKSonTMTU1N4enpWe4xr873TH2UmZkJmUz2zHc4VuX3q744cOAArK2t4eLigoCAANy9e7fcsppyPqSmpmLHjh3w9/d/ZtnnPSeYBFVTeno6ioqKSo1KbWNjg5SUlDLXSUlJqVL5+qi4uBgTJ05E165d0aZNm3LLubi4YO3atfjjjz+wfv16FBcX46WXXsKtW7dqsLaq5enpiYiICOzevRthYWFITExE9+7dkZ2dXWZ5TTgftm3bhoyMDPj5+ZVbpiGeC2UpOa5VOebV+Z6pbx49eoTp06fj7bffrvBFmVX9/aoP+vfvjx9//BF79+7Fl19+iYMHD2LAgAEoKioqs7wmnA8AsG7dOhgbG2PYsGEVllPFOdEgXptBdUdgYCDOnz//zPuyXl5eSi+4femll+Dq6opvv/0W8+bNU3c11WLAgAHSz23btoWnpyccHR2xadOmSv1H0xCtWbMGAwYMgJ2dXbllGuK5QJVTUFCAN998E0IIhIWFVVi2If5+jRgxQvrZ3d0dbdu2hZOTEw4cOIC+ffvWYs1q19q1azFy5MhnPiChinOCV4KqydLSEtra2khNTVWan5qaCoVCUeY6CoWiSuXrm6CgIGzfvh379+9HkyZNqrSujo4O2rdvj6tXr6qpdjXPzMwMLVu2LHefGvr5cOPGDURHR+P999+v0noN8VwAIB3Xqhzz6nzP1BclCdCNGzcQFRVV4VWgsjzr96s+at68OSwtLcvdp4Z8PpT466+/EB8fX+XvDaB65wSToGrS1dVFx44dsXfvXmlecXEx9u7dq/Rf7ZO8vLyUygNAVFRUueXrCyEEgoKCsHXrVuzbtw/NmjWrcoyioiKcO3cOtra2aqhh7Xjw4AESEhLK3aeGej6UCA8Ph7W1NQYNGlSl9RriuQAAzZo1g0KhUDrmWVlZOHbsWLnHvDrfM/VBSQJ05coVREdHw8LCosoxnvX7VR/dunULd+/eLXefGur58KQ1a9agY8eOaNeuXZXXrdY58VzdqjXcL7/8IuRyuYiIiBAXL14U48aNE2ZmZiIlJUUIIcSoUaPEp59+KpU/fPiwaNSokfjqq69EXFycmD17ttDR0RHnzp2rrV1QiYCAAGFqaioOHDggkpOTpU9ubq5U5um2mDNnjtizZ49ISEgQJ0+eFCNGjBB6enriwoULtbELKjFlyhRx4MABkZiYKA4fPiy8vb2FpaWlSEtLE0JozvkgxOMnVhwcHMT06dNLLWvI50J2drY4ffq0OH36tAAgli5dKk6fPi099fTFF18IMzMz8ccff4izZ8+KIUOGiGbNmomHDx9KMfr06SNWrlwpTT/re6Yuqqgd8vPzxauvviqaNGkizpw5o/SdkZeXJ8V4uh2e9ftVF1XUDtnZ2eKTTz4RMTExIjExUURHR4sOHTqIFi1aiEePHkkxGsL5IMSzfzeEECIzM1MYGBiIsLCwMmOo45xgEvScVq5cKRwcHISurq7o3LmzOHr0qLSsZ8+ewtfXV6n8pk2bRMuWLYWurq5o3bq12LFjRw3XWPUAlPkJDw+XyjzdFhMnTpTazcbGRgwcOFCcOnWq5iuvQm+99ZawtbUVurq6onHjxuKtt94SV69elZZryvkghBB79uwRAER8fHypZQ35XNi/f3+Zvwsl+1tcXCxmzpwpbGxshFwuF3379i3VRo6OjmL27NlK8yr6nqmLKmqHxMTEcr8z9u/fL8V4uh2e9ftVF1XUDrm5uaJfv37CyspK6OjoCEdHRzF27NhSyUxDOB+EePbvhhBCfPvtt0JfX19kZGSUGUMd54RMCCGqfM2JiIiIqJ5jnyAiIiLSSEyCiIiISCMxCSIiIiKNxCSIiIiINBKTICIiItJITIKIiIhIIzEJIiIiIo3EJIiIiIg0EpMgIqqS69evQyaT4cyZM+WWOXDgAGQyGTIyMp5rW7169cLEiROrtE5ISAg8PDyea7v1VWWODRH9D5MgonooJSUF48ePR/PmzSGXy2Fvb4/BgweXeiHr8/Lz88PQoUOV5tnb2yM5ORlt2rRR6bbo+fHYEFVNo9quABFVzfXr19G1a1eYmZlh8eLFcHd3R0FBAfbs2YPAwEBcunRJrdvX1taGQqFQ6zaoenhsiKqGV4KI6pmPPvoIMpkMx48fx/Dhw9GyZUu0bt0akydPxtGjR6VyS5cuhbu7OwwNDWFvb4+PPvoIDx48kJZHRETAzMwMe/bsgaurK4yMjNC/f38kJycDeHxbad26dfjjjz8gk8kgk8lw4MCBMm+57Ny5Ey1btoS+vj569+6N69evK9X57t27ePvtt9G4cWMYGBjA3d0dP//8s1KZnJwcjB49GkZGRrC1tcWSJUsq1R5ffPEFbGxsYGxsDH9/fzx69KhUmR9++AGurq7Q09NDq1at8M0331QYs1evXhg/fjwmTpyIF154ATY2Nvj++++Rk5ODMWPGwNjYGM7Ozti1a5fSeufPn8eAAQNgZGQEGxsbjBo1Cunp6UpxP/74Y0ybNg3m5uZQKBQICQmRlgshEBISAgcHB8jlctjZ2eHjjz+Wlv/000/o1KkTjI2NoVAo8M477yAtLU1aXtaxeVadfvvtN7i7u0NfXx8WFhbw9vZGTk7OM9udqEGo8qtgiajW3L17V8hkMrFw4cJnll22bJnYt2+fSExMFHv37hUuLi4iICBAWh4eHi50dHSEt7e3iI2NFSdPnhSurq7inXfeEUIIkZ2dLd58803Rv39/kZycLJKTk0VeXp70FvDTp08LIYRISkoScrlcTJ48WVy6dEmsX79e2NjYCADi/v37Qgghbt26JRYvXixOnz4tEhISxIoVK4S2trY4duyYVJ+AgADh4OAgoqOjxdmzZ8Urr7wijI2NxYQJE8rdx19//VXI5XLxww8/iEuXLonPP/9cGBsbi3bt2kll1q9fL2xtbcXvv/8url27Jn7//Xdhbm4uIiIiyo3bs2dPYWxsLObNmycuX74s5s2bJ7S1tcWAAQPEd999Jy5fviwCAgKEhYWFyMnJEUIIcf/+fWFlZSWCg4NFXFycOHXqlHj55ZdF7969leKamJiIkJAQcfnyZbFu3Tohk8lEZGSkEEKIzZs3CxMTE7Fz505x48YNcezYMfHdd99J669Zs0bs3LlTJCQkiJiYGOHl5SUGDBggLX/62DyrTrdv3xaNGjUSS5cuFYmJieLs2bNi1apVIjs7u9y2IWpImAQR1SPHjh0TAMSWLVuqvO7mzZuFhYWFNB0eHi4AiKtXr0rzVq1aJWxsbKRpX19fMWTIEKU4T/+hDQ4OFm5ubkplpk+frpQElWXQoEFiypQpQojHCZeurq7YtGmTtPzu3btCX1+/wiTIy8tLfPTRR0rzPD09lZIgJycnsXHjRqUy8+bNE15eXuXG7dmzp+jWrZs0XVhYKAwNDcWoUaOkecnJyQKAiImJkWL269dPKc7NmzcFABEfH19mXCGEePHFF8X06dOFEEIsWbJEtGzZUuTn55dbtyfFxsYKAFLS8vSxeVadTp48KQCI69evV2p7RA0Nb4cR1SNCiEqXjY6ORt++fdG4cWMYGxtj1KhRuHv3LnJzc6UyBgYGcHJykqZtbW2Vbq9URlxcHDw9PZXmeXl5KU0XFRVh3rx5cHd3h7m5OYyMjLBnzx4kJSUBABISEpCfn68Ux9zcHC4uLs+17ZycHCQkJMDf3x9GRkbSZ/78+UhISKgwdtu2baWftbW1YWFhAXd3d2mejY0NAEjt9c8//2D//v1K22nVqpW0f2XFBZTb/I033sDDhw/RvHlzjB07Flu3bkVhYaFU9uTJkxg8eDAcHBxgbGyMnj17AoDUjk97Vp3atWuHvn37wt3dHW+88Qa+//573L9/v8J2IWpI2DGaqB5p0aIFZDLZMzs/X79+Ha+88goCAgKwYMECmJub4++//4a/vz/y8/NhYGAAANDR0VFaTyaTVSnRqqzFixfj66+/xvLly6V+ShMnTkR+fr7Kt/Wkkj5Q33//falkSVtbu8J1y2qbJ+fJZDIAQHFxsbStwYMH48svvywVy9bWtsK4JTHs7e0RHx+P6OhoREVF4aOPPsLixYtx8OBB5Ofnw8fHBz4+PtiwYQOsrKyQlJQEHx+fctvxWXXS1tZGVFQUjhw5gsjISKxcuRKff/45jh07hmbNmlXYPkQNAa8EEdUj5ubm8PHxwapVq8rsvFoyLs/JkydRXFyMJUuWoEuXLmjZsiVu375d5e3p6uqiqKiowjKurq44fvy40rwnO2gDwOHDhzFkyBC8++67aNeuHZo3b47Lly9Ly52cnKCjo4Njx45J8+7fv69UprxtP7nO09u2sbGBnZ0drl27BmdnZ6WPqv/Id+jQARcuXEDTpk1LbcvQ0LDScfT19TF48GCsWLECBw4cQExMDM6dO4dLly7h7t27+OKLL9C9e3e0atXqmVftKlMnmUyGrl27Ys6cOTh9+jR0dXWxdevW52oLovqCSRBRPbNq1SoUFRWhc+fO+P3333HlyhXExcVhxYoV0q0gZ2dnFBQUYOXKlbh27Rp++uknrF69usrbatq0Kc6ePYv4+Hikp6ejoKCgVJkPP/wQV65cwdSpUxEfH4+NGzciIiJCqUyLFi2kKw5xcXH44IMPkJqaKi03MjKCv78/pk6din379uH8+fPw8/ODllbFX1ETJkzA2rVrER4ejsuXL2P27Nm4cOGCUpk5c+YgNDQUK1aswOXLl3Hu3DmEh4dj6dKlVW6PigQGBuLevXt4++23ERsbi4SEBOzZswdjxox5ZiJZIiIiAmvWrMH58+dx7do1rF+/Hvr6+nB0dISDgwN0dXWlY/rf//4X8+bNe646HTt2DAsXLsSJEyeQlJSELVu24M6dO3B1dVVFkxDVeUyCiOqZ5s2b49SpU+jduzemTJmCNm3a4OWXX8bevXsRFhYGAGjXrh2WLl2KL7/8Em3atMGGDRsQGhpa5W2NHTsWLi4u6NSpE6ysrHD48OFSZRwcHPD7779j27ZtaNeuHVavXo2FCxcqlZkxYwY6dOgAHx8f9OrVCwqFotQgjIsXL0b37t0xePBgeHt7o1u3bujYsWOF9Xvrrbcwc+ZMTJs2DR07dsSNGzcQEBCgVOb999/HDz/8gPDwcLi7u6Nnz56IiIhQ+ZUgOzs7HD58GEVFRejXrx/c3d0xceJEmJmZPTOZK2FmZobvv/8eXbt2Rdu2bREdHY0///wTFhYWsLKyQkREBDZv3gw3Nzd88cUX+Oqrr56rTiYmJjh06BAGDhyIli1bYsaMGViyZAkGDBigiiYhqvNkQh0dAIiIqMbFx8ejVatWuHLlCpydnWu7OkR1Hq8EERE1APfu3cNvv/0GExMT2Nvb13Z1iOoFPh1GRNQA+Pv74+TJkwgLC4NcLq/t6hDVC7wdRkRERBqJt8OIiIhIIzEJIiIiIo3EJIiIiIg0EpMgIiIi0khMgoiIiEgjMQkiIiIijcQkiIiIiDQSkyAiIiLSSP8HpBfO+5DBZ2EAAAAASUVORK5CYII=",
      "text/plain": [
       "<Figure size 640x480 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "# Histograma para visualizar la cant_mensajes\n",
    "sns.histplot(data=user_profile, x='cant_mensajes', hue='plan', palette=['skyblue','green'], bins=20)\n",
    "plt.title('Distribución de cantidad de mensajes por plan')\n",
    "plt.xlabel('Cantidad de mensajes')\n",
    "plt.ylabel('Frecuencia')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "22d143fe",
   "metadata": {},
   "source": [
    "💡Insights: \n",
    "- ....La distribución de la cantidad de mensajes está sesgada a la derecha, lo que indica que la mayoría de usuarios envía pocos mensajes y solo un grupo reducido presenta volúmenes altos."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 43,
   "id": "785b6b16",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAjsAAAHHCAYAAABZbpmkAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjMuNCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy8QVMy6AAAACXBIWXMAAA9hAAAPYQGoP6dpAABYiElEQVR4nO3deVgVZf8/8PdhO+wgO6ggCiK4i6nkriQalqYtlguaXy1FU+lxzS03yspcQi1TNJdHs7RFzQ23UlREyQXcgWPJIiqgoKz37w9/nMcjoCxzODC8X9d1rjoz97nnMzNweDtzz4xCCCFAREREJFN6ui6AiIiISJsYdoiIiEjWGHaIiIhI1hh2iIiISNYYdoiIiEjWGHaIiIhI1hh2iIiISNYYdoiIiEjWGHaIKiAnJweLFi3Cvn37dF0KERG9AMMOPdfcuXOhUCiqZFndunVDt27d1O+PHDkChUKBn376qUqW/zSFQoG5c+eWOj8kJASbN29G+/btq6Se4cOHo0GDBlWyLF0o2tdHjhx5Ydtnf04qKyEhAQqFAuvXr6/Q59evXw+FQoGEhARJ6ilpW9Tk/V+Ta68K3D5Vg2GnFin6Ui56GRsbw8XFBQEBAVi+fDkePHggyXJu376NuXPnIiYmRpL+qpsff/wRv/zyC/744w9YW1vrupwaZeXKlRUOFUREFWWg6wKo6s2bNw/u7u7Iy8tDcnIyjhw5gokTJ2LJkiX47bff0KJFC3XbmTNnYtq0aeXq//bt2/j000/RoEEDtGrVqsyf279/f7mWo02PHj2CgUHxXw8hBP755x/88ccfcHV11UFlNdvKlSthZ2eH4cOHa0zv0qULHj16BCMjI90URkSyxrBTC/Xp0wdt27ZVv58+fToOHTqEvn374vXXX0dcXBxMTEwAAAYGBiX+0ZdSdnY2TE1Nq9UfOmNj4xKnKxQKhISEVHE18qenp1fqNieqjrKysmBmZqbrMqiMeBqLAAA9evTArFmzkJiYiE2bNqmnlzRm58CBA+jUqROsra1hbm4OLy8vzJgxA8CT8QYvvfQSAGDEiBHqU2ZFpy66deuGZs2aITo6Gl26dIGpqan6s6WNxSgoKMCMGTPg5OQEMzMzvP7667h165ZGmwYNGhQ7WlBan48fP8bcuXPRuHFjGBsbw9nZGQMGDMCNGzfUbUoas3Pu3Dn06dMHlpaWMDc3R8+ePXHy5EmNNkWnCo8fP46QkBDY29vDzMwMb7zxBu7cuVOsvpL88ssvaNasGYyNjdGsWTPs3LmzxHaFhYVYunQpmjZtCmNjYzg6OuKDDz7A/fv3y7Scy5cv4+2334a9vT1MTEzg5eWFTz75RD0/MTERY8eOhZeXF0xMTGBra4u33nqr2NiUsq5zgwYNcOnSJRw9elT9c1G0b0obs/Pdd9+hUaNGMDExQbt27fDnn38WW4/c3FzMnj0bvr6+sLKygpmZGTp37ozDhw8Xa5ueno7hw4fDysoK1tbWCAoKQnp6epm2FwBcunQJPXr0gImJCerVq4cFCxagsLCwxLZ//PEHOnfuDDMzM1hYWCAwMBCXLl0q87Je5Msvv8TLL78MW1tbmJiYwNfXt8TxbQqFAuPGjcP27dvh4+MDExMT+Pn54cKFCwCAb7/9Fh4eHjA2Nka3bt2K7d8///wTb731FlxdXaFUKlG/fn1MmjQJjx49Krassv7slrX2533XPE/ROm/evBleXl4wNjaGr68vjh07VqxteX6vjx49irFjx8LBwQH16tUrdflFP8/btm174XdXZbZP0XoWbXelUommTZti7969L1xGbcMjO6Q2dOhQzJgxA/v378eoUaNKbHPp0iX07dsXLVq0wLx586BUKnH9+nUcP34cAODt7Y158+Zh9uzZGD16NDp37gwAePnll9V93L17F3369MGgQYMwZMgQODo6PreuhQsXQqFQYOrUqUhNTcXSpUvh7++PmJgY9RGosiooKEDfvn0RERGBQYMGYcKECXjw4AEOHDiAixcvolGjRqWud+fOnWFpaYkpU6bA0NAQ3377Lbp164ajR48WG6g8fvx41KlTB3PmzEFCQgKWLl2KcePGYdu2bc+tb//+/Rg4cCB8fHwQGhqKu3fvYsSIESV+sX7wwQdYv349RowYgY8++gjx8fH45ptvcO7cORw/fhyGhoalLuf8+fPo3LkzDA0NMXr0aDRo0AA3btzA77//joULFwIAoqKicOLECQwaNAj16tVDQkICVq1ahW7duiE2NhampqblWuelS5di/PjxMDc3V4eq5+37tWvX4oMPPsDLL7+MiRMn4ubNm3j99ddhY2OD+vXrq9tlZmbi+++/x7vvvotRo0bhwYMHWLt2LQICAnD69Gn1qVQhBPr164e//voLH374Iby9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cQObNm0qdin5sGHD8MMPPyAkJASnT59G586dkZWVhYMHD2Ls2LHo169fiTUsWLBAfc+PsWPHwsDAAN9++y1ycnKwePHiCq1XSUJDQxEYGIhOnTrh/fffx71797BixQo0bdoUDx8+VLfr2rUrPvjgA4SGhiImJga9evWCoaEhrl27hu3bt2PZsmV48803S13O8uXL0alTJ7Rp0wajR4+Gu7s7EhISsHv3bvVjPvr27YuNGzfCysoKPj4+iIyMxMGDByt1Oauvry9WrVqFBQsWwMPDAw4ODhpHbooYGhpiwYIF+OCDD9CjRw+88847iI+PR3h4eLExO3379sWOHTvwxhtvIDAwEPHx8Vi9ejV8fHw0ttlrr72Gjh07Ytq0aUhISICPjw927NhR4jiIkkyZMgUbN25E7969MWHCBPWl525ubjh//ry6naWlJVatWoWhQ4eiTZs2GDRoEOzt7aFSqbB792507NixxLBdHoGBgViyZAl69+6N9957D6mpqQgLC4OHh4dGLZXVpEkTNGrUCP/5z3/w77//wtLSEj///HOJ40HK+rNb1trL8l3zPM2aNUNAQIDGpecA8Omnn6rbaPv3uizfXc+qqn1b6+jkGjDSiaLLJ4teRkZGwsnJSbzyyiti2bJlGpdIFnn20vOIiAjRr18/4eLiIoyMjISLi4t49913xdWrVzU+9+uvvwofHx9hYGCgcVlv165dRdOmTUusr7RLz//73/+K6dOnCwcHB2FiYiICAwOLXTIthBBfffWVqFu3rlAqlaJjx47izJkzxfoU4sllnZ988olwd3cXhoaGwsnJSbz55psal5/imUvPhRDi7NmzIiAgQJibmwtTU1PRvXt3ceLEiRK38bOX95d0OXFpfv75Z+Ht7S2USqXw8fERO3bsKPHyVCGeXIrt6+srTExMhIWFhWjevLmYMmWKuH379guXc/HiRfHGG28Ia2trYWxsLLy8vMSsWbPU8+/fvy9GjBgh7OzshLm5uQgICBCXL18udpl/edY5OTlZBAYGCgsLC41LyEvbPitXrhTu7u5CqVSKtm3bimPHjhXbp4WFhWLRokXCzc1NKJVK0bp1a7Fr164St9ndu3fF0KFDhaWlpbCyshJDhw4V586dK9Ol50IIcf78edG1a1dhbGws6tatK+bPny/Wrl1b7HLtonUKCAgQVlZWwtjYWDRq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",
      "text/plain": [
       "<Figure size 640x480 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "# Histograma para visualizar la cant_llamadas\n",
    "sns.histplot(data=user_profile, x='cant_llamadas', hue='plan', palette=['skyblue','green'], bins=20)\n",
    "plt.title('Distribución de cantidad de llamadas por plan')\n",
    "plt.xlabel('Cantidad de llamadas')\n",
    "plt.ylabel('Frecuencia')\n",
    "plt.show()\n"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "296b130f",
   "metadata": {},
   "source": [
    "💡Insights: \n",
    "- Distribución ...La distribución de la cantidad de llamadas está sesgada a la derecha, lo que indica que la mayoría de usuarios realiza pocas llamadas y un grupo menor presenta un uso intensivo.Se observa que los usuarios del plan Premium tienden a registrar más llamadas que los del plan Básico, lo que sugiere que los planes con mayores beneficios incentivan un mayor uso del servicio de voz."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 44,
   "id": "82936fc2",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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Q0NDnD9/HrGxsfD09MS///1vuftIREREJIsyBZ+IiAh8+umn0NLSgra2NrKzs1G7dm0sWbIEM2fOlLuPRERERLIoU/DR1dWFltbzTW1tbREXFwcAMDc3x7179+TrHREREZGMyjTHp1mzZggLC4OLiws6duyIOXPmIDk5GT/++CMaNWokdx+JiIiIZFGmIz6LFi2Cvb09AOCrr75CjRo1MH78eDx8+BAbN26UtYNEREREcinTEZ8WLVpI/7e1tcXhw4dl6xARERFReeENDImIiEhjlPiIT/PmzRESEoIaNWqgWbNmUCgUr6x7+fJlWTpHREREJKcSB58+ffpAX18fANC3b9/y6g8RERFRuSlx8Jk7d26x/yciIiKqKso0xycsLAwXLlwoUn7hwgVcunTprTtFREREVB7KFHz8/f2LvVHhP//8A39//7fuFBEREVF5KFPwiYyMRPPmzYuUN2vWDJGRkW/dKSIiIqLyUKbgo6+vj8TExCLl8fHx0NEp062BiIiIiMpdmYJPt27dMGPGDKSmpkplKSkpmDlzJt577z3ZOkdEREQkpzIdnvn3v/+NDh06wMnJCc2aNQPw/Bvb7ezs8OOPP8raQSIiIiK5lCn41KxZE3/99Re2bt2KP//8E4aGhhg5ciQGDx4MXV1duftIREREJIsyT8gxNjbG2LFj5ewLERERUbkqc/C5desWTp48iaSkJBQUFKismzNnzlt3jIiIiEhuZQo+mzZtwvjx42FtbQ2lUqnyvV0KhYLBh4iIiCqlMgWfhQsX4quvvsL06dPl7g8RERFRuSnT5exPnjzBwIED5e4LERERUbkqU/AZOHAgjh49KndfiIiIiMpVmU511atXD7Nnz8b58+fh4eFR5BL2SZMmydI5IiIiIjmVKfhs3LgRJiYmOH36NE6fPq2yTqFQMPgQERFRpVSm4BMTEyN3P4iIiIjKXZnm+BTKyclBdHQ08vLy5OoPERERUbkpU/DJzMzE6NGjYWRkhIYNGyIuLg4AMHHiRHz99deydpCIiIhILmUKPjNmzMCff/6JU6dOwcDAQCr39vbGjh07ZOscERERkZzKNMdn37592LFjB1q3bq1y1+aGDRvizp07snWOiIiISE5lOuLz8OFD2NraFinPyMhQCUJvsm7dOjRu3BhmZmYwMzODl5cXDh06JK3PysqCv78/rKysYGJi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",
      "text/plain": [
       "<Figure size 640x480 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "# Histograma para visualizar la cant_minutos_llamada\n",
    "sns.histplot(data=user_profile, x='cant_minutos_llamada', hue='plan', palette=['skyblue','green'], bins=20)\n",
    "plt.title('Distribución de minutos de llamadas por plan')\n",
    "plt.xlabel('Minutos de llamadas')\n",
    "plt.ylabel('Frecuencia')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "005fff14",
   "metadata": {},
   "source": [
    "💡Insights: \n",
    "- ...La distribución de los minutos de llamada está sesgada a la derecha, mostrando que la mayoría de usuarios consume pocos minutos mientras que un grupo reducido presenta consumos altos.Se observa que los usuarios del plan Premium concentran mayores cantidades de minutos, lo que sugiere un uso más intensivo del servicio de llamadas en comparación con el plan Básico. Esto indica que los beneficios del plan influyen en el comportamiento de consumo."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "178069e5",
   "metadata": {},
   "source": [
    "### 5.2 Identificación de Outliers\n",
    "\n",
    "🎯 **Objetivo:**  \n",
    "Detectar valores extremos en las variables clave de **uso** y **clientes** que podrían afectar el análisis, y decidir si requieren limpieza o revisión adicional.\n",
    "\n",
    "**Instrucciones:**  \n",
    "- Usa **boxplots** para identificar visualmente outliers en las siguientes columnas:  \n",
    "  - `age` \n",
    "  - `cant_mensajes`\n",
    "  - `cant_llamadas`\n",
    "  - `total_minutos_llamada`  \n",
    "- Crea un **for** para generar los 4 boxplots automáticamente.\n",
    "<br>\n",
    "\n",
    "- Después de crear los gráfico, responde si **existen o no outliers** en las variables.  \n",
    "- Si hay outliers, crea otro bucle para calcular los límites de esas columnas usando el **método IQR** y decide qué hacer con ellos.\n",
    "  - Si solamente hay outliers de un solo lado, no es necesario calcular ambos límites.\n",
    "\n",
    "**Hint:**\n",
    "- Dentro del bucle, usa `plt.title(f'Boxplot: {col}')` para que el título cambie acorde a la columna."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 45,
   "id": "789bfa05",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAggAAAHHCAYAAADaqqCfAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjMuNCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy8QVMy6AAAACXBIWXMAAA9hAAAPYQGoP6dpAAAdIklEQVR4nO3df5SVdZ3A8c+dAWaAYVCnkZycIMCAoyKKv2JFfu1BUfGYJh1sTchadxNTqATTHDTbTePI2u+WVUZIM6nQLBXxR5sSFoq4akKAkRgBIjKAwqAzz/7R4R6vXxAi4cLM63XOPWd47nNnPvfrOPOe53nuTC7LsiwAAN6hpNgDAAD7H4EAACQEAgCQEAgAQEIgAAAJgQAAJAQCAJAQCABAQiAAAAmBAK1QLpeLyZMnF3sMYD8mEOB9VF9fH7lcruB26KGHxpAhQ+KBBx4o9nj/sD/84Q8xefLkWLFiRbFHAfayNsUeAFqi66+/Pj7ykY9ElmWxZs2aqK+vjzPOOCPuu+++OOuss4o93h77wx/+ENddd10MHjw4unXrVuxxgL1IIMBeMGLEiDj++OPz/7744oujS5cu8eMf//iADgSg9XCKAfaBgw46KNq3bx9t2hQ2+RtvvBFf/OIXo7a2NsrKyqJXr14xZcqU2P5HVrds2RK9e/eO3r17x5YtW/KPW79+fRx22GExYMCAaGpqioiIMWPGREVFRbz00ktx2mmnRceOHaOmpiauv/762J0/2vrMM8/EiBEjorKyMioqKmLYsGHx5JNP5u+vr6+P888/PyIihgwZkj+F8utf/zoiIhoaGmLx4sXR0NCwy4917733xplnnhk1NTVRVlYWPXr0iK997Wv55/JO3/3ud6N79+7Rvn37OPHEE+Pxxx+PwYMHx+DBgwv2a2xsjLq6uujZs2eUlZVFbW1tXHnlldHY2LjLeYCUQIC9oKGhIdatWxevvvpqvPDCC/Hv//7vsXnz5viXf/mX/D5ZlsXZZ58dU6dOjdNPPz1uvvnm6NWrV3z5y1+OCRMmRERE+/bt4/bbb49ly5bF1VdfnX/spZdeGg0NDVFfXx+lpaX57U1NTXH66adHly5d4qabbor+/ftHXV1d1NXVvee8L7zwQgwcODCeffbZuPLKK+OrX/1q/OlPf4rBgwfH7373u4iIOPXUU+MLX/hCRER85StfiZkzZ8bMmTOjT58+ERExe/bs6NOnT8yePXuX61NfXx8VFRUxYcKEuOWWW6J///5x7bXXxqRJkwr2+/73vx/jxo2Lww8/PG666aYYOHBgnHPOOfHKK68U7Nfc3Bxnn312TJkyJUaOHBnf/va345xzzompU6fGJz/5yV3OA+xABrxvpk+fnkVEcisrK8vq6+sL9r3nnnuyiMhuuOGGgu2f+MQnslwuly1btiy/7aqrrspKSkqy3/zmN9msWbOyiMj+67/+q+BxF110URYR2WWXXZbf1tzcnJ155plZu3btsldffTW/PSKyurq6/L/POeecrF27dtny5cvz21atWpV16tQpO/XUU/Pbtn/sxx57bKfPffr06btcpzfffDPZdskll2QdOnTItm7dmmVZljU2NmZVVVXZCSeckL311lv5/err67OIyAYNGpTfNnPmzKykpCR7/PHHC97nD37wgywisnnz5u1yJqCQIwiwF3z3u9+NuXPnxty5c+NHP/pRDBkyJD772c/Gz3/+8/w+999/f5SWluZ/Kt/ui1/8YmRZVvCqh8mTJ8eRRx4ZF110UXz+85+PQYMGJY/bbty4cfm3c7lcjBs3LrZt2xYPP/zwDvdvamqKhx56KM4555zo3r17fvthhx0WF1xwQTzxxBOxcePGXT7nMWPGRJZlMWbMmF3u2759+/zbmzZtinXr1sXAgQPjzTffjMWLF0dExFNPPRWvvfZafO5znys4NfOpT30qDj744IL3N2vWrOjTp0/07t071q1bl78NHTo0IiIee+yxXc4EFHKRIuwFJ554YsFFiqNHj45jjz02xo0bF2eddVa0a9cu/vznP0dNTU106tSp4LHbD9n/+c9/zm9r165d3HbbbXHCCSdEeXl5TJ8+PXK5XPJxS0pKCr7JR0R89KMfjYjY6UsTX3311XjzzTejV69eyX19+vSJ5ubmWLlyZRx55JG79+R3wwsvvBDXXHNNPProo0l8bL+GYfvz79mzZ8H9bdq0SV5BsXTp0njxxRejurp6hx9v7dq179Pk0HoIBNgHSkpKYsiQIXHLLbfE0qVL9+ib7Zw5cyIiYuvWrbF06dL4yEc+8n6PuU9s2LAhBg0aFJWVlXH99ddHjx49ory8PBYuXBgTJ06M5ubmv/t9Njc3x9FHHx0333zzDu+vra39R8eGVkcgwD7y9ttvR0TE5s2bIyKi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gQAAJAQCAJAQCABAQiAAAAmBAAAkBAIAkBAIAEBCIAAACYEAACQEAgCQEAgAQEIgAAAJgQAAJAQCAJAQCABAQiAAAAmBAAAkBAIAkBAIAEBCIAAACYEAACQEAgCQEAgAQEIgAAAJgQAAJAQCAJAQCABAQiAAAAmBAAAkBAIAkBAIAEBCIAAACYEAACQEAgCQEAgAQEIgAAAJgQAAJAQCAJAQCABAQiAAAAmBAAAkBAIAkBAIAEBCIAAACYEAACQEAgCQEAgAQEIgAAAJgQAAJAQCAJAQCABAQiAAAAmBAAAkBAIAkBAIAEBCIAAACYEAACQEAgCQEAgAQEIgAAAJgQAAJAQCAJBos7s7NjY2RmNjY/7fGzdu3CsDAQDFt9tHEP7zP/8zOnfunL/V1tbuzbkAgCLa7UC46qqroqGhIX9buXLl3pwLACii3T7FUFZWFmVlZXtzFgBgP+EiRQAgIRAAgIRAAAASAgEASAgEACAhEACAhEAAABICAQBICAQAICEQAICEQAAAEgIBAEgIBAAgIRAAgIRAAAASAgEASAgEACAhEACAhEAAABICAQBICAQAICEQAICEQAAAEgIBAEgIBAAgIRAAgIRAAAASAgEASAgEACAhEACAhEAAABICAQBICAQAICEQAICEQAAAEgIBAEgIBAAgIRAAgIRAAAASAgEASAgEACAhEACAhEAAABICAQBICAQAICEQAICEQAAAEgIBAEgIBAAgIRAAgIRAAAASAgEASAgEACAhEACAhEAAABICAQBICAQAICEQAICEQAAAEgIBAEgIBAAgIRAAgIRAAAASAgEASAgEACAhEACAhEAAABICAQBICAQAICEQAICEQAAAEgIBAEgIBKBAZWXlDt8GWheBABQoKSnZ4dtA6+L/fgAgIRAAgIRAAAASAgEASAgEACAhEACAhEAAABICAQBICAQAICEQAICEQAAAEgIBAEgIBAAgIRAAgIRAAAASAgEASAgEACAhEACAhEAAABICAQBICAQAICEQAICEQAAAEgIBAEgIBAAgIRAAgIRAAAASAgEASAgEACAhEACAhEAAABICAQBICAQAICEQAICEQAAAEgIBAEgIBAAgIRAAgIRAAAASAgEASAgEACAhEACAhEAAABICAQBICAQAICEQAICEQAAAEgIBAEgIBAAgIRAAgIRAAAASAgEASAgEACAhEACAhEAAABICAQBICAQAICEQAICEQAAAEgIBAEgIBAAgIRAAgIRAAAASAgEASAgEACAhEACAhEAAABICAQBICAQAICEQAICEQAAAEgIBKFBeXr7Dt4HWpU2xBwD2L+Xl5fHAAw/k3wZaJ4EAFMjlctG+fftijwEUmVMMAEBCIAAACYEAACQEAgCQEAgAQEIgAAAJgQAAJAQCAJAQCABAQiAAAAmBAAAkBAIAkBAIAEBCIAAACYEAACQEAgCQEAgAQEIgAAAJgQAAJAQCAJAQCABAQiAAAAmBAAAkBAIAkBAIAEBCIAAACYEAACQEAgCQEAgAQEIgAAAJgQAAJAQCAJAQCABAos2ePjDLsoiI2Lhx4/s2DACwd23/vr39+/jO7HEgbNq0KSIiamtr9/RdAABFsmnTpujcufNO789lu0qInWhubo5Vq1ZFp06dIpfL7fGAB4qNGzdGbW1trFy5MiorK4s9Tqth3YvDuheHdS+O1rbuWZbFpk2boqamJkpKdn6lwR4fQSgpKYnDDz98Tx9+wKqsrGwVn0D7G+teHNa9OKx7cbSmdX+vIwfbuUgRAEgIBAAgIRB2U1lZWdTV1UVZWVmxR2lVrHtxWPfisO7FYd13bI8vUgQAWi5HEACAhEAAABICAQBICAQAICEQ3sM3vvGNyOVyccUVVxRsnz9/fgwdOjQ6duwYlZWVceqpp8aWLVuKM2QL9O51X7FiReRyuR3eZs2aVdxhW5Adfb6vXr06LrzwwvjgBz8YHTt2jOOOOy5+9rOfFW/IFmhH6758+fL4+Mc/HtXV1VFZWRmjRo2KNWvWFG/IFmDy5MnJ14/evXvn79+6dWtceumlUVVVFRUVFXHeeee1+jUXCDuxYMGC+OEPfxh9+/Yt2D5//vw4/fTTY/jw4fH73/8+FixYEOPGjXvPX1fJ7tvRutfW1sZf//rXgtt1110XFRUVMWLEiCJO23Ls7PP905/+dCxZsiR+8YtfxHPPPRfnnntujBo1Kp555pkiTdqy7Gjd33jjjRg+fHjkcrl49NFHY968ebFt27YYOXJkNDc3F3HaA9+RRx5Z8HXkiSeeyN83fvz4uO+++2LWrFnxv//7v7Fq1ao499xzizjtfiAjsWnTpuyII47I5s6dmw0aNCi7/PLL8/eddNJJ2TXXXFO84Vqw91r3d+vXr1/2mc98Zt8N14K917p37NgxmzFjRsH+hxxySDZt2rR9PGXLs7N1nzNnTlZSUpI1NDTk992wYUOWy+WyuXPnFmnaA19dXV12zDHH7PC+DRs2ZG3bts1mzZqV3/biiy9mEZHNnz9/H024//Fj7w5ceumlceaZZ8Y///M/F2xfu3Zt/O53v4tDDz00BgwYEF26dIlBgwYVVCh7bmfr/m5PP/10LFq0KC6++OJ9NFnL9l7rPmDAgPjJT34S69evj+bm5rjrrrti69atMXjw4H0/aAuzs3VvbGyMXC5X8Et7ysvLo6SkxNeaf9DSpUujpqYmunfvHp/61Kfi5Zdfjoi/fU156623Cv5b9O7dOz784Q/H/PnzizVu0e3xH2tqqe66665YuHBhLFiwILnvpZdeioi/ncuaMmVK9OvXL2bMmBHDhg2L559/Po444oh9PW6L8V7r/m633npr9OnTJwYMGLAPJmvZdrXud999d3zyk5+MqqqqaNOmTXTo0CFmz54dPXv23MeTtizvte4nn3xydOzYMSZOnBj/8R//EVmWxaRJk6KpqSn++te/FmHaluGkk06K+vr66NWrV/405cCBA+P555+P1atXR7t27eKggw4qeEyXLl1i9erVxRl4P+AIwjusXLkyLr/88rjjjjuivLw8uX/7+b9LLrkkxo4dG8cee2xMnTo1evXqFbfddtu+HrfF2NW6v9OWLVvizjvvdPTgfbA76/7Vr341NmzYEA8//HA89dRTMWHChBg1alQ899xz+3jalmNX615dXR2zZs2K++67LyoqKqJz586xYcOGOO6441zr9A8YMWJEnH/++dG3b9847bTT4v77748NGzbE3XffXezR9l/FPsexP5k9e3YWEVlpaWn+FhFZLpfLSktLs2XLlmURkc2cObPgcaNGjcouuOCCIk194NvVur/99tv5fWfMmJG1bds2W7t2bREnbhl29/P9+eefL3jcsGHDsksuuaRIUx/4/p7P91dffTV7/fXXsyzLsi5dumQ33XRTkaZumY4//vhs0qRJ2SOPPJJFRH6tt/vwhz+c3XzzzcUZbj/gFMM7DBs2LPnJaOzYsdG7d++YOHFidO/ePWpqamLJkiUF+/zxj390Nf0/YFfrXlpamt9+6623xtlnnx3V1dX7eswWZ1fr/uabb0ZEJD+1lpaWupr+H/D3fL5/4AMfiIiIRx99NNauXRtnn332Pp21Jdu8eXMsX748Lrzwwujfv3+0bds2HnnkkTjvvPMiImLJkiXx8ssvx8c+9rEiT1o8AuEdOnXqFEcddVTBto4dO0ZVVVV++5e//OWoq6uLY445Jvr16xe33357LF68OH76058WY+QWYXfWPSJi2bJl8Zvf/Cbuv//+fT1ii7SrdX/rrbeiZ8+ecckll8SUKVOiqqoq7rnnnpg7d2788pe/LNLUB77d+XyfPn169OnTJ6qrq2P+/Plx+eWXx/jx46NXr17FGLlF+NKXvhQjR46Mrl27xqpVq6Kuri5KS0tj9OjR0blz57j44otjwoQJccghh0RlZWVcdtll8bGPfSxOPvnkYo9eNALh73TFFVfE1q1bY/z48bF+/fo45phjYu7cudGjR49ij9bi3XbbbXH44YfH8OHDiz1Kq9C2bdu4//77Y9KkSTFy5MjYvHlz9OzZM26//fY444wzij1ei7ZkyZK46qqrYv369dGtW7e4+uqrY/z48cUe64D2yiuvxOjRo+O1116L6urqOOWUU+LJJ5/MH42cOnVqlJSUxHnnnReNjY1x2mmnxfe+970iT11c/twzAJBwSSwAkBAIAEBCIAAACYEAACQEAgCQEAgAQEIgAAAJgQAAJAQCAJAQCABAQiBAK/Lggw/GKaecEgcddFBUVVXFWWedFcuXL8/f/9vf/jb69esX5eXlcfzxx8c999wTuVwuFi1alN/n+eefjxEjRkRFRUV06dIlLrzwwli3bl0Rng2wNwkEaEXeeOONmDBhQjz11FPxyCOPRElJSXz84x+P5ubm2LhxY4wcOTKOPvroWLhwYXzta1+LiRMnFjx+w4YNMXTo0Dj22GPjqaeeigcffDDWrFkTo0aNKtIzAvYWf6wJWrF169ZFdXV1PPfcc/HEE0/ENddcE6+88kqUl5dHRMT//M//xOc+97l45plnol+/fnHDDTfE448/HnPmzMm/j1deeSVqa2tjyZIl8dGPfrRYTwV4nzmCAK3I0qVLY/To0dG9e/eorKyMbt26RUTEyy+/HEuWLIm+ffvm4yAi4sQTTyx4/LPPPhuPPfZYVFRU5G+9e/eOiCg4VQEc+NoUewBg3xk5cmR07do1pk2bFjU1NdHc3BxHHXVUbNu2bbcev3nz5hg5cmTceOONyX2HHXbY+z0uUEQCAVqJ1157LZYsWRLTpk2LgQMHRkTEE088kb+/V69e8aMf/SgaGxujrKwsIiIWLFhQ8D6OO+64+NnPfhbdunWLNm18+YCWzCkGaCUOPvjgqKqqiv/+7/+OZcuWxaOPPhoTJkzI33/BBRdEc3Nz/Ou//mu8+OKLMWfOnJgyZUpERORyuYiIuPTSS2P9+vUxevToWLBgQSxfvjzmzJkTY8eOjaampqI8L2DvEAjQSpSUlMRdd90VTz/9dBx11FExfvz4+OY3v5m/v7KyMu67775YtGhR9OvXL66++uq49tprIyLy1yXU1NTEvHnzoqmpKYYPHx5HH310XHHFFXHQQQdFSYkvJ9CSeBUDsFN33HFHjB07NhoaGqJ9+/bFHgfYh5xEBPJmzJgR3bt3jw996EPx7LPPxsSJE2PUqFHiAFohgQDkrV69Oq699tpYvXp1HHbYYXH++efH17/+9WKPBRSBUwwAQMJVRQBAQiAAAAmBAAAkBAIAkBAIAEBCIAAACYEAACQEAgCQEAgAQOL/AR6DIbUFrK1HAAAAAElFTkSuQmCC",
      "text/plain": [
       "<Figure size 640x480 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAg4AAAHHCAYAAADXtNDYAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjMuNCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy8QVMy6AAAACXBIWXMAAA9hAAAPYQGoP6dpAAAu7UlEQVR4nO3de5zN1eL/8feeGXMzF2Iw457GbRSa8EiETMZQkXLvGKIiOlEkFUMnZ4pSUsm372mmctBVddJUxqE6SL5RkdKQWwiJmWHMYPb6/eE3+9jmtmbsuen1fDzm8ZjP57M+n7U+a6+993t/Lns7jDFGAAAAFrwqugEAAKDqIDgAAABrBAcAAGCN4AAAAKwRHAAAgDWCAwAAsEZwAAAA1ggOAADAGsEBAABYIzgApeRwODRz5syKbgYqiTVr1sjhcGjNmjUV3RSgTBEcUOkkJyfL4XC4/dWpU0c9evRQSkpKRTfvom3btk0zZ87U7t27K7opJZaVlaWZM2fy5gj8iflUdAOAwjz++ONq2rSpjDE6dOiQkpOT1adPH/3rX//STTfdVNHNK7Vt27Zp1qxZ6t69u5o0aVLRzSmRrKwszZo1S5LUvXv3im1MJXP99dfr1KlT8vX1reimAGWK4IBKKy4uTtdcc41revTo0apbt66WLl1apYMDLk1eXl7y9/ev6GYAZY5TFagyatSooYCAAPn4uOfdkydP6sEHH1TDhg3l5+enFi1a6Omnn1beD7+eOnVKLVu2VMuWLXXq1CnXen/88YfCw8PVuXNn5ebmSpJGjhypoKAg/fLLL4qNjVX16tUVERGhxx9/XDY/JLt582bFxcUpJCREQUFB6tmzp7766ivX8uTkZA0cOFCS1KNHD9epmLxD/+np6frpp5+Unp5u1ScpKSnq1q2bgoODFRISog4dOmjJkiWu5V9++aUGDhyoRo0ayc/PTw0bNtSkSZPc+uH8/d6/f7/69++voKAghYWFafLkya6+2b17t8LCwiRJs2bNcrXd9jqPvGsA3nrrLc2aNUv169dXcHCwbr/9dqWnpysnJ0cTJ05UnTp1FBQUpFGjRiknJyffdhYvXqzo6GgFBATosssu05AhQ7Rv3z63Mt27d1ebNm20bds29ejRQ4GBgapfv77mzJmTb3sLFixQVFSUAgMDVbNmTV1zzTVufbhnzx7de++9atGihQICAlSrVi0NHDgw36mmwq5x2LBhg3r37q3Q0FAFBgaqW7duWrt2rVuZzMxMTZw4UU2aNJGfn5/q1KmjG2+8UZs2bbLqW6A8ERxQaaWnp+v333/XkSNH9MMPP2jcuHE6ceKE7rjjDlcZY4xuueUWPfvss+rdu7fmzZunFi1aaMqUKXrggQckSQEBAXrttde0Y8cOPfroo651x48fr/T0dCUnJ8vb29s1Pzc3V71791bdunU1Z84cRUdHKyEhQQkJCUW294cfflDXrl313Xff6aGHHtL06dO1a9cude/eXRs2bJB07nD2X//6V0nSI488ojfeeENvvPGGWrVqJUlavny5WrVqpeXLlxfbP8nJyerbt6/++OMPTZs2TU8++aTatWunTz75xFXm7bffVlZWlsaNG6cFCxYoNjZWCxYs0IgRI/JtLzc3V7GxsapVq5aefvppdevWTc8884z+53/+R5IUFhamhQsXSpJuvfVWV9sHDBhQbFvPl5iYqE8//VQPP/yw7rzzTr333nsaO3as7rzzTv3888+aOXOmBgwYoOTkZD311FNu686ePVsjRoxQZGSk5s2bp4kTJ2rVqlW6/vrrdfz4cbeyx44dU+/evdW2bVs988wzatmypaZOnep2ncwrr7yiv/71r2rdurWee+45zZo1S+3atXM9XpK0ceNGrVu3TkOGDNHzzz+vsWPHatWqVerevbuysrKK3Nd///vfuv7665WRkaGEhAT9/e9/1/Hjx3XDDTfo66+/dpUbO3asFi5cqNtuu00vvfSSJk+erICAAP34448l6lugXBigkklKSjKS8v35+fmZ5ORkt7Lvv/++kWSeeOIJt/m33367cTgcZseOHa5506ZNM15eXuaLL74wb7/9tpFknnvuObf14uPjjSRz3333ueY5nU7Tt29f4+vra44cOeKaL8kkJCS4pvv37298fX3Nzp07XfMOHDhggoODzfXXX++al1f36tWrC933pKSkIvvo+PHjJjg42HTq1MmcOnXKbZnT6XT9n5WVlW/dxMRE43A4zJ49e/Lt9+OPP+5Wtn379iY6Oto1feTIkXz7bWv16tVGkmnTpo05ffq0a/7QoUONw+EwcXFxbuWvvfZa07hxY9f07t27jbe3t5k9e7ZbuS1bthgfHx+3+d26dTOSzOuvv+6al5OTY+rVq2duu+0217x+/fqZqKioIttdUB+uX78+3/bz9i/vcXU6nSYyMtLExsbme0yaNm1qbrzxRte80NBQM378+CLbAVQWHHFApfXiiy9q5cqVWrlypRYvXqwePXpozJgxeu+991xlPv74Y3l7e7s+xed58MEHZYxx+3Q5c+ZMRUVFKT4+Xvfee6+6deuWb708EyZMcP3vcDg0YcIEnT59WqmpqQWWz83N1Weffab+/fvr8ssvd80PDw/XsGHD9J///EcZGRnF7vPIkSNljNHIkSOLLLdy5UplZmbq4Ycfznde3eFwuP4PCAhw/X/y5En9/vvv6ty5s4wx2rx5c77tjh071m26a9eu+uWXX4ptd0mMGDFC1apVc0136tRJxhjdeeedbuU6deqkffv26ezZs5Kk9957T06nU4MGDdLvv//u+qtXr54iIyO1evVqt/WDgoLcjk75+vqqY8eObvtTo0YN/frrr9q4cWOh7T2/D8+cOaOjR4/qiiuuUI0aNYo8lfDtt98qLS1Nw4YN09GjR13tPXnypHr27KkvvvhCTqfT1Y4NGzbowIEDRXUdUClwcSQqrY4dO7pdHDl06FC1b99eEyZM0E033SRfX1/t2bNHERERCg4Odls379D/nj17XPN8fX316quvqkOHDvL391dSUpLbm2weLy8vtzd/SWrevLkkFXoL5ZEjR5SVlaUWLVrkW9aqVSs5nU7t27dPUVFRdjtfjJ07d0qS2rRpU2S5vXv3asaMGfrwww917Ngxt2UXXkfh7+/vuoYhT82aNfOtd7EaNWrkNh0aGipJatiwYb75TqdT6enpqlWrltLS0mSMUWRkZIHbPT+MSFKDBg3yPb41a9bU999/75qeOnWqUlNT1bFjR11xxRXq1auXhg0bpuuuu85V5tSpU0pMTFRSUpL279/vdq1LUdeipKWlSZLi4+MLLZOenq6aNWtqzpw5io+PV8OGDRUdHa0+ffpoxIgR+cYhUBkQHFBleHl5qUePHpo/f77S0tJK9Sb86aefSpKys7OVlpampk2berqZlUZubq5uvPFG/fHHH5o6dapatmyp6tWra//+/Ro5cqTr026e86/zKEuF1VPY/Lw3aqfTKYfDoZSUlALLBgUFlWh70rlQt337dn300Uf65JNP9O677+qll17SjBkzXLed3nfffUpKStLEiRN17bXXKjQ0VA6HQ0OGDMnXh+fLWzZ37ly1a9euwDJ5bR40aJC6du2q5cuX67PPPtPcuXP11FNP6b333lNcXFyhdQAVgeCAKiXvsPWJEyckSY0bN1ZqaqoyMzPdjjr89NNPruV5vv/+ez3++OMaNWqUvv32W40ZM0ZbtmxxfeLN43Q69csvv7iOMkjSzz//LEmFfu9CWFiYAgMDtX379nzLfvrpJ3l5ebk+URd0lKOkmjVrJknaunWrrrjiigLLbNmyRT///LNee+01t4shV65cWep6PdH20mrWrJmMMWratKnbY3OxqlevrsGDB2vw4ME6ffq0BgwYoNmzZ2vatGny9/fXO++8o/j4eD3zzDOudbKzs/NdjFlQeyUpJCREMTExxbYjPDxc9957r+69914dPnxYV199tWbPnk1wQKXDNQ6oMs6cOaPPPvtMvr6+rlMRffr0UW5url544QW3ss8++6wcDofrRffMmTMaOXKkIiIiNH/+fCUnJ+vQoUOaNGlSgXWdvz1jjF544QVVq1ZNPXv2LLC8t7e3evXqpQ8++MDtdMahQ4e0ZMkSdenSRSEhIZLOvVFJKvCNx/Z2zF69eik4OFiJiYnKzs52W5b3iTrvE/f5n7CNMZo/f36R2y5KYGBgoW0vawMGDJC3t7dmzZqV79ZYY4yOHj1a4m1euI6vr69at24tY4zOnDkj6Vw/XljfggULXLepFiY6OlrNmjXT008/7Qq65zty5Iikc0eGLny869Spo4iIiAJvRwUqGkccUGmlpKS4jhwcPnxYS5YsUVpamh5++GHXm/DNN9+sHj166NFHH9Xu3bvVtm1bffbZZ/rggw80ceJE16e+J554Qt9++61WrVql4OBgXXXVVZoxY4Yee+wx3X777erTp4+rXn9/f33yySeKj49Xp06dlJKSohUrVuiRRx7Jdw3A+Z544gmtXLlSXbp00b333isfHx8tWrRIOTk5bt8f0K5dO3l7e+upp55Senq6/Pz8dMMNN6hOnTpavny5Ro0apaSkpCIvkAwJCdGzzz6rMWPGqEOHDho2bJhq1qyp7777TllZWXrttdfUsmVLNWvWTJMnT9b+/fsVEhKid99996KuWQgICFDr1q315ptvqnnz5rrsssvUpk2bYq+18IRmzZrpiSee0LRp07R79271799fwcHB2rVrl5YvX667775bkydPLtE2e/XqpXr16um6665T3bp19eOPP+qF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AACsERwAAIA1ggMAALBGcAAAANYIDgAAwBrBAQAAWCM4AAAAawQHAABgjeAAAACsERwAAIA1ggMAALBGcAAAANYIDgAAwBrBAQAAWCM4AAAAawQHAABgjeAAAACsERwAAIA1ggMAALBGcAAAANYIDgAAwBrBAQAAWCM4AAAAawQHAABgjeAAAACsERwAAIA1ggMAALBGcAAAANYIDgAAwBrBAQAAWCM4AAAAawQHAKgEunfv7vorSzExMerevbtiYmLKtB5JGjFihLp3764RI0aUaT0TJkxQ9+7dNWHChDKtR5LWrVunwYMHa926dZdEPaVBcACACpaQkFDktKekpqbq7NmzkqSzZ88qNTW1TOqRpLS0NO3du1eStHfvXqWlpZVJPXv37tXWrVslSVu3bnXVWRays7M1b948HTp0SPPmzVN2dnaVrqe0CA4AUME+//zzIqc95Yknnihy2pPGjRtX5LSnjB07tshpT/rnP/+po0ePSpKOHj2qJUuWVOl6SsunohtwPmOM6//KlrDgzu3xOe9xgwXGeZVw/mNjynCM9+jRo9D5q1ev9lg999xzT6HzFy1a5LF6JOnll192HdnIc/bsWb388ssefWNfunSpsrKy3OZlZWVp6dKlGjp0qMfqkaRff/1VS5YscY0FY4yWLFmiXr16qUGDBlWunothHRxycnKUk5Pjms7IyPB4Y87f/q233urx7aOMOM9K8q3oVlQdzv++oDLOq4acnBwFBgZ6fLsHDx4sNJQYY3Tw4EGFh4dfdD1ZWVnavn17gcu2b9+urKwsj+3fmTNntGzZsgKXLVu2TKNHj1a1atUuup6zZ88WGngWLVqkgQMHysfHM5+NjTGaP39+ofPnzJkjh8NRZeq5WNanKhITExUaGur6a9iwYVm2CwAuecV9KvbUp+biThN48jTCggULLmq5rddff/2ilpfE3r17tXHjRuXm5rrNz83N1caNGz12XUV51XOxrOPYtGnT9MADD7imMzIyPB4e/Pz8XP8vX75c/v7+Ht0+PCc7O/u/n5a9KtUZr8rvvP5inFde54/x81+bPKm4Q+pLly71SD0LFy5Unz59ilzuKffdd58+/PDDIpd7wogRI4oMB568k6NRo0bq0KGDNm3a5Pam7u3trejoaDVq1KhK1XOxrF/x/fz8yuzJk+f8QzD+/v4KCAgo0/rgIZXg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5pZbbjENGjQw3377rdtrRk5OjmsbF/ZDcc+vyqiofsjMzDSTJ08269evN7t27TKpqanm6quvNpGRkSY7O9u1jUthPBhT/HPDGGPS09NNYGCgWbhwYYHbKIsxUe7BwRhjFixYYBo1amR8fX1Nx44dzVdffeVa1q1bNxMfH+9W/q233jLNmzc3vr6+JioqyqxYsaKcW+x5kgr8S0pKcpW5sC8mTpzo6re6deuaPn36mE2bNpV/4z1o8ODBJjw83Pj6+pr69eubwYMHmx07driW/1nGQ55PP/3USDLbt2/Pt+xSHQ+rV68u8LmQt69Op9NMnz7d1K1b1/j5+ZmePXvm65/GjRubhIQEt3lFvc5URkX1w65duwp9zVi9erVrGxf2Q3HPr8qoqH7IysoyvXr1MmFhYaZatWqmcePG5q677soXAC6F8WBM8c8NY4xZtGiRCQgIMMePHy9wG2UxJvhZbQAAYI3fqgAAANYIDgAAwBrBAQAAWCM4AAAAawQHAABgjeAAAACsERwAAIA1ggOASmvkyJHq379/RTcDwHn4AiigiunevbvatWun5557rqKbUubS09NljFGNGjUquikA/j9+VhtApRUaGlrRTQBwAU5VAB7mdDo1Z84cXXHFFfLz81OjRo00e/ZsSdLUqVPVvHlzBQYG6vLLL9f06dN15swZ17ozZ85Uu3bt9MYbb6hJkyYKDQ3VkCFDlJmZKencofvPP/9c8+fPl8PhkMPh0O7du4tsz5o1a+RwOPTpp5+qffv2CggI0A033KDDhw8rJSVFrVq1UkhIiIYNG6asrCy3/UhMTFTTpk0VEBCgtm3b6p133sm33VWrVumaa65RYGCgOnfurO3bt7vKfPfdd+rRo4eCg4MVEhKi6Oho/d///Z8k6ejRoxo6dKjq16+vwMBAXXnllVq6dKlb2y88VVFcm44dO6bhw4crLCxMAQEBioyMVFJSkuUjB8AGRxwAD5s2bZpeeeUVPfvss+rSpYsOHjyon376SZIUHBys5ORkRUREaMuWLbrrrrsUHByshx56yLX+zp079f777+ujjz7SsWPHNGjQID355JOaPXu25s+fr59//llt2rTR448/LkkKCwuzatfMmTP1wgsvKDAwUIMGDdKgQYPk5+enJUuW6MSJE7r11lu1YMECTZ06VZKUmJioxYsX6+WXX1ZkZKS++OIL3XHHHQoLC1O3bt1c23300Uf1zDPPKCwsTGPHjtWdd96ptWvXSpKGDx+u9u3ba+HChfL29ta3336ratWqSZKys7MVHR2tqVOnKiQkRCtWrNBf/vIXNWvWTB07dixwH4pr0/Tp07Vt2zalpKSodu3a2rFjh06dOlXCRxBAkUr4Y10AipCRkWH8/PzMK6+8YlV+7ty5Jjo62jWdkJBgAgMDTUZGhmvelClTTKdOnVzT3bp1M/fff791m/J+YS81NdU1LzEx0UgyO3fudM275557TGxsrDHGmOzsbBMYGGjWrVvntq3Ro0eboUOHFrrdFStWGEmun7wODg42ycnJ1m3t27evefDBB13T8fHxpl+/ftZtuvnmm82oUaOs6wNQchxxADzoxx9/VE5Ojnr27Fng8jfffFPPP/+8du7cqRMnTujs2bMKCQlxK9OkSRMFBwe7psPDw3X48OGLbttVV13l+r9u3bqu0yXnz/v6668lSTt27FBWVpZuvPFGt22cPn1a7du3L3S74eHhkqTDhw+rUaNGeuCBBzRmzBi98cYbiomJ0cCBA9WsWTNJUm5urv7+97/rrbfe0v79+3X69Gnl5OQoMDCwwPbbtGncuHG67bbbtGnTJvXq1Uv9+/dX586dS9RPAIpGcAA8KCAgoNBl69ev1/DhwzVr1izFxsYqNDRUy5Yt0zPPPONWLu9Qfh6HwyGn03nRbTt/uw6Ho8h6Tpw4IUlasWKF6tev71bOz8+vyO1Kcm1n5syZGjZsmFasWKGUlBQlJCRo2bJluvXWWzV37lzNnz9fzz33nK688kpVr15dEydO1OnTpwtsv02b4uLitGfPHn388cdauXKlevbsqfHjx+vpp5+26CEANggOgAdFRkYqICBAq1at0pgxY9yWrVu3To0bN9ajjz7qmrdnz54S1+Hr66vc3NyLbmtRWrduLT8/P+3du9fteobSaN68uZo3b65JkyZp6NChSkpK0q233qq1a9eqX79+uuOOOySdCxs///yzWrdufVFtCgsLU3x8vOLj49W1a1dNmTKF4AB4EMEB8CB/f39NnTpVDz30kHx9fXXdddfpyJEj+uGHHxQZGam9e/dq2bJl6tChg1asWKHly5eXuI4mTZpow4YN2r17t4KCgnTZZZfJy8uzN0gFBwdr8uTJmjRpkpxOp7p06aL09HStXbtWISEhio+PL3Ybp06d0pQpU3T77beradOm+vXXX7Vx40bddtttks6FrHfeeUfr1q1TzZo1NW/ePB06dKjQ4GDTphkzZig6OlpRUVHKycnRRx99pFatWnm0b4A/O4ID4GHTp0+Xj4+PZsyYoQMHDig8PFxjx47V6NGjNWnSJE2YMEE5OTnq27evpk+frpkzZ5Zo+5MnT1Z8fLxat26tU6dOadeuXWrSpInH9+Nvf/ubwsLClJiYqF9++UU1atTQ1VdfrUceecRqfW9vbx09elQjRozQoUOHVLt2bQ0YMECzZs2SJD322GP65ZdfFBsbq8DAQN19993q37+/0tPTS90mX19fTZs2Tbt371ZAQIC6du2qZcuWXXxnAHDhmyMBVFpDhw6Vt7e3Fi9eXNFNAfD/8QVQACqds2fPatu2bVq/fr2ioqIqujkAzkNwAKq4sWPHKigoqMC/sWPHVnTzSmXr1q265pprFBUVVWX3AbhUcaoCqOIOHz6sjIyMApeFhISoTp065dwiAJcyggMAALDGqQoAAGCN4AAAAKwRHAAAgDWCAwAAsEZwAAAA1ggOAADAGsEBAABYIzgAAABr/w9ZaZHHGpAtBQAAAABJRU5ErkJggg==",
      "text/plain": [
       "<Figure size 640x480 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAggAAAHHCAYAAADaqqCfAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjMuNCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy8QVMy6AAAACXBIWXMAAA9hAAAPYQGoP6dpAAAppUlEQVR4nO3deXRUZZ6H8W8lIamCJKxhiRK2jgIxiLIKItAwxIAR6HFpWQPa0wiM7LKJLKJsyiI6CJzTxNMtYOuwCRMgMIDaiCABFdmUXaAJIJKwJEDyzh92aijfsAip3BCezzkcU/dW6v5uJVae3LqVchljjAAAAK4S4PQAAACg8CEQAACAhUAAAAAWAgEAAFgIBAAAYCEQAACAhUAAAAAWAgEAAFgIBAAAYCEQgNvkcrk0ZswYp8e4Yxw8eFAul0tJSUneZWPGjJHL5XJuqNtwJ88OXA+BgEIrKSlJLpfL51/58uXVsmVLJScnOz3ebdu5c6fGjBmjgwcPOj3Kb3bhwgWNGTNG69evd3oUAH4S5PQAwI2MGzdO1apVkzFGJ06cUFJSktq2batPPvlETzzxhNPj3bKdO3dq7NixatGihapWrer0OL/JhQsXNHbsWElSixYtnB0GgF8QCCj04uPjVb9+fe/l559/XhUqVNCCBQvu6EAAgMKMpxhwxylVqpQ8Ho+Cgnz79vz58xo0aJAqV66skJAQ3X///XrzzTeV+4alFy9eVM2aNVWzZk1dvHjR+3k//fSTKlWqpCZNmig7O1uSlJiYqNDQUO3fv19xcXEqUaKEIiMjNW7cON3MG6Bu27ZN8fHxCg8PV2hoqFq1aqVNmzZ51yclJenpp5+WJLVs2dL7FEruIfuzZ89q9+7dOnv27E3dJ8nJyWrevLnCwsIUHh6uBg0aaP78+d71n332mZ5++mlFRUUpJCRElStX1oABA3zuh6v3++jRo+rQoYNCQ0MVERGhwYMHe++bgwcPKiIiQpI0duxY7+z5fR7GvHnz9Pvf/17ly5dXSEiIateurVmzZlnXq1q1qp544gmtX79e9evXl8fjUWxsrPe+XLRokWJjY+V2u1WvXj1t27bN5/O/+eYbJSYmqnr16nK73apYsaJ69uyp06dPW9v6/PPP1aBBA7ndbtWoUUOzZ8++rdm/+uorxcXFqVy5cvJ4PKpWrZp69ux5C/cWkP84goBC7+zZszp16pSMMUpLS9PMmTN17tw5denSxXsdY4yefPJJrVu3Ts8//7zq1q2rVatWaciQITp69KimTZsmj8ej999/X02bNtXIkSM1depUSVKfPn109uxZJSUlKTAw0Hub2dnZevzxx9W4cWNNnjxZK1eu1OjRo3XlyhWNGzfumvN+9913atasmcLDw/Xyyy+rWLFimj17tlq0aKENGzaoUaNGeuyxx/TSSy/p7bff1ogRI1SrVi1J8v538eLF6tGjh+bNm6fExMTr3j9JSUnq2bOnYmJiNHz4cJUqVUrbtm3TypUr1alTJ0nSRx99pAsXLujFF19U2bJltXnzZs2cOVM//vijPvroI5/by87OVlxcnBo1aqQ333xTa9as0VtvvaUaNWroxRdfVEREhGbNmqUXX3xRHTt21B/+8AdJUp06dW7yK3pzZs2apZiYGD355JMKCgrSJ598ot69eysnJ0d9+vTxue4PP/ygTp066c9//rO6dOmiN998UwkJCXrvvfc0YsQI9e7dW5I0YcIEPfPMM9qzZ48CAn75/SglJUX79+9Xjx49VLFiRX333XeaM2eOvvvuO23atMl7AuK3336rNm3aKCIiQmPGjNGVK1c0evRoVahQ4ZZmT0tL897esGHDVKpUKR08eFCLFi3K1/sRuGUGKKTmzZtnJFn/QkJCTFJSks91lyxZYiSZ8ePH+yx/6qmnjMvlMj/88IN32fDhw01AQID59NNPzUcffWQkmenTp/t8Xvfu3Y0k85//+Z/eZTk5OaZdu3YmODjYnDx50rtckhk9erT3cocOHUxwcLDZt2+fd9mxY8dMWFiYeeyxx7zLcre9bt26a+77vHnzrnsf/fzzzyYsLMw0atTIXLx40WddTk6O9+MLFy5YnzthwgTjcrnMoUOHrP0eN26cz3UfeughU69ePe/lkydPWvt9sw4cOGDt2+jRo82vH47ymjkuLs5Ur17dZ1mVKlWMJLNx40bvslWrVhlJxuPx+Ozf7Nmzrfs8r+0sWLDASDKffvqpd1mHDh2M2+32ub2dO3eawMDAW5p98eLFRpLZsmWLdV2gMOApBhR67777rlJSUpSSkqK//e1vatmypV544QWf37T+53/+R4GBgXrppZd8PnfQoEEyxvi86mHMmDGKiYlR9+7d1bt3bzVv3tz6vFx9+/b1fuxyudS3b19dunRJa9asyfP62dnZWr16tTp06KDq1at7l1eqVEmdOnXS559/rvT09Bvuc2JioowxNzx6kJKSooyMDA0bNkxut9tn3dUvvfN4PN6Pz58/r1OnTqlJkyYyxliH3CWpV69ePpebNWum/fv333Du/HT1zLlHkZo3b679+/dbT73Url1bjzzyiPdyo0aNJEm///3vFRUVZS2/el+u3k5mZqZOnTqlxo0bS5JSU1Ml/fJ1XbVqlTp06OBze7Vq1VJcXNwtzV6qVClJ0vLly3X58uWbuUuAAkUgoNBr2LChWrdurdatW6tz585asWKFateu7f1hLUmHDh1SZGSkwsLCfD4395D9oUOHvMuCg4P1l7/8RQcOHFBGRobmzZuX5+vYAwICfH7IS9J9990nSdd8aeLJkyd14cIF3X///da6WrVqKScnR0eOHLn5nb+Bffv2SZIeeOCB617v8OHDSkxMVJkyZbznFTRv3lySrB+2brfbe45BrtKlS+vMmTP5NvfN+Mc//qHWrVurRIkSKlWqlCIiIjRixAhJ9sxX/9CWpJIlS0qSKleunOfyq/flp59+Ur9+/VShQgV5PB5FRESoWrVqPts5efKkLl68qOjoaGvOvL7WNzN78+bN9e///u8aO3asypUrp/bt22vevHnKysq6yXsI8C8CAXecgIAAtWzZUsePH9f3339/S7exatUqSb/8xnirt3GnyM7O1r/9279pxYoVGjp0qJYsWaKUlBTvHyrKycnxuf7V52E4Zd++fWrVqpVOnTqlqVOnasWKFUpJSdGAAQMk3fzM11purjrR9JlnntHcuXPVq1cvLVq0SKtXr9bKlSvz3E5+zu5yufTxxx/riy++UN++fXX06FH17NlT9erV07lz537zdoH8xkmKuCNduXJFkrwPpFWqVNGaNWuUkZHhcxRh9+7d3vW5vvnmG40bN049evTQ9u3b9cILL+jbb7/1/naZKycnR/v37/ceNZCkvXv3StI1/25BRESEihcvrj179ljrdu/erYCAAO9vtfnx1/dq1KghSdqxY4d+97vf5Xmdb7/9Vnv37tX777+vbt26eZenpKTc8nb9/ZcDP/nkE2VlZWnZsmU+RwfWrVuXr9s5c+aM1q5dq7Fjx+rVV1/1Lv91NEZERMjj8eQZk7/+Wv/W2Rs3bqzGjRvr9ddf1/z589W5c2ctXLhQL7zwwu3s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AgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACxBTg9wNWOMMjMzJUlut1sul8vhiQDg2njMQlFWqI4gZGZmKj4+XvHx8d7/6QCgsOIxC0VZoQoEAABQOBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAADAQiAAAAALgQAAACwEAgAAsBAIAHAHatGihfdfQYqPj1eLFi0UHx9foNvduHGjnn32WW3cuLFAt+uUwrC/BAIA3GGmTZt23cv+smnTJl28eFGSdPHiRW3atKlAtpuZmampU6fqxIkTmjp1qjIzMwtku04pLPtLIADAHWbp0qXXvewvw4YNu+5lf/nggw90+vRpSdLp06c1f/78AtmuUwrL/gY5stVrMMZ4Py7qheg0n/v3qvsd+Yzv6SLt6q+pKaD/j9q0aXPN5atXr/bbdl9++eVrLp88ebLftvvjjz9q/vz53vvXGKP58+erTZs2uvfee/22XacUpv296UDIyspSVlaW93J6enq+D3P17Xfs2DHfbx/XkHNFUrDTUxRNOVe8H/I9XbRlZWWpePHift3GqVOndOnSpTzXXbp0SadOnVK5cuXyfbuZmZnavHlznus2b96szMxMud3ufN+uMUYzZsy45vLJkyfL5XLl+3adUtj296afYpgwYYJKlizp/Ve5cmV/zgUA+JVnn332ttbfqiFDhtzW+lt1+PBhbdmyRdnZ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",
      "text/plain": [
       "<Figure size 640x480 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAgwAAAHHCAYAAADTQQDlAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjMuNCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy8QVMy6AAAACXBIWXMAAA9hAAAPYQGoP6dpAAA100lEQVR4nO3dd3xUVf7/8fekd0JNCB0EacECmC+gC6xZIiBgWXdXEoqiLghKFxWRIkpxFRBdLOuCDwV12S+ggixNwAIi0hEEli4gIC2hhEDm/P7wN/ebSUJOEtJ9PR8PHmRuOed87tzMvOeWjMsYYwQAAJADn+IeAAAAKPkIDAAAwIrAAAAArAgMAADAisAAAACsCAwAAMCKwAAAAKwIDAAAwIrAAAAArAgMKNVcLpfGjBlT3MP4Taldu7Z69+5d3MMoEmPGjJHL5fKaVprrL81jR/EjMCBbs2bNksvl8vpXpUoVtW/fXosXLy7u4V23HTt2aMyYMTpw4EBxDyXPLl68qDFjxmjVqlXFPZRCU5qfH6Cs8ivuAaBkGzdunOrUqSNjjI4fP65Zs2apU6dO+uyzz3T33XcX9/DybceOHRo7dqzatWun2rVrF/dw8uTixYsaO3asJKldu3ZF3v+uXbvk41O4nzVK8/MDlFUEBuSoY8eOatGihfO4T58+ioqK0ocffliqAwPyLzAwsLiHAKAYcEoCeRIZGang4GD5+XlnzQsXLmjo0KGqUaOGAgMDdeONN+pvf/ubPF+GeunSJTVs2FANGzbUpUuXnPVOnz6tqlWrqnXr1kpPT5ck9e7dW2FhYdq3b58SEhIUGhqqmJgYjRs3Trn5ctVNmzapY8eOioiIUFhYmO688059++23zvxZs2bpgQcekCS1b9/eOeXiOcR/7tw5/fjjjzp37lyutsnixYvVtm1bhYeHKyIiQi1bttScOXOc+V999ZUeeOAB1axZU4GBgapRo4YGDx7stR0y1n3kyBHdc889CgsLU+XKlTVs2DBn2xw4cECVK1eWJI0dO9YZe26v41i1apVcLpf+9a9/aezYsapWrZrCw8P1xz/+UefOndPly5c1aNAgValSRWFhYXrooYd0+fJlrzYynwf3nL765ptvNGTIEFWuXFmhoaG69957dfLkSa91rzXWjG3anh9J+vvf/64mTZooMDBQMTEx6t+/v86ePevV5p49e3T//fcrOjpaQUFBql69uv7yl7/k+nnNrdOnT2vYsGGKjY1VWFiYIiIi1LFjR23ZssVruYLY9jNnztTvf/97ValSRYGBgWrcuLFmzJiRZUzGGI0fP17Vq1dXSEiI2rdvrx9++CHfYwckjjDA4ty5c/rll19kjNGJEyc0ffp0nT9/XklJSc4yxhh17dpVK1euVJ8+fXTzzTdryZIlGj58uI4cOaIpU6YoODhY7733ntq0aaORI0fq1VdflST1799f586d06xZs+Tr6+u0mZ6errvuukv/8z//o8mTJ+s///mPRo8eratXr2rcuHHXHO8PP/ygO+64QxEREXrqqafk7++vt956S+3atdPq1asVFxen3/3ud3ryySf12muv6dlnn1WjRo0kyfl//vz5euihhzRz5kzrBWKzZs3Sww8/rCZNmuiZZ55RZGSkNm3apP/85z/q3r27JGnu3Lm6ePGi+vXrp4oVK+q7777T9OnT9dNPP2nu3Lle7aWnpyshIUFxcXH629/+puXLl+uVV15RvXr11K9fP1WuXFkzZsxQv379dO+99+q+++6TJDVr1iyXz+ivJkyYoODgYD399NP673//q+nTp8vf318+Pj46c+aMxowZo2+//VazZs1SnTp19Pzzz1vbfOKJJ1S+fHmNHj1aBw4c0NSpUzVgwAB9/PHHeRqb7fkZM2aMxo4dq/j4ePXr10+7du3SjBkztH79en3zzTfy9/dXWlqaEhISdPnyZT3xxBOKjo7WkSNHtHDhQp09e1blypXL05hysm/fPi1YsEAPPPCA6tSpo+PHj+utt95S27ZttWPHDsXExHgtfz3bfsaMGWrSpIm6du0qPz8/ffbZZ3r88cfldrvVv39/Z7nnn39e48ePV6dOndSpUydt3LhRHTp0UFpa2nWNHb9xBsjGzJkzjaQs/wIDA82sWbO8ll2wYIGRZMaPH+81/Y9//KNxuVzmv//9rzPtmWeeMT4+PubLL780c+fONZLM1KlTvdbr1auXkWSeeOIJZ5rb7TadO3c2AQEB5uTJk850SWb06NHO43vuuccEBASYvXv3OtOOHj1qwsPDze9+9ztnmqfvlStXXrP2mTNn5riNzp49a8LDw01cXJy5dOmS1zy32+38fPHixSzrTpgwwbhcLnPw4MEsdY8bN85r2VtuucU0b97ceXzy5MksdefWypUrjSTTtGlTk5aW5kx/8MEHjcvlMh07dvRavlWrVqZWrVpe02rVqmV69erlPPZsr/j4eK+6Bw8ebHx9fc3Zs2edadcad+Y2r/X8nDhxwgQEBJgOHTqY9PR0Z/rrr79uJJl//vOfxhhjNm3aZCSZuXPn2jZJjkaPHm0yv0xmHmtqaqrXWIwxZv/+/SYwMNDruSyIbZ/dvpSQkGDq1q3rPPZso86dO3s9H88++6yRlK+xA8YYwykJ5OiNN97QsmXLtGzZMn3wwQdq3769HnnkEc2bN89Z5vPPP5evr6+efPJJr3WHDh0qY4zXXRVjxoxRkyZN1KtXLz3++ONq27ZtlvU8BgwY4Pzscrk0YMAApaWlafny5dkun56erqVLl+qee+5R3bp1nelVq1ZV9+7d9fXXXys5Odlac+/evWWMsR5dWLZsmVJSUvT0008rKCjIa17GW/GCg4Odny9cuKBffvlFrVu3ljFGmzZtytJu3759vR7fcccd2rdvn3XcedGzZ0/5+/s7j+Pi4mSM0cMPP+y1XFxcnA4fPqyrV69a23zssce86r7jjjuUnp6ugwcPFti4ly9frrS0NA0aNMjrwstHH31UERERWrRokSQ5RxCWLFmiixcvFlj/2QkMDHTGkp6erlOnTiksLEw33nijNm7cmGX569n2Gfclz9G/tm3bat++fc6pFs82euKJJ7yej0GDBl332PHbRmBAjm677TbFx8crPj5eiYmJWrRokRo3buy8eUvSwYMHFRMTo/DwcK91PYeQM75hBAQE6J///Kf279+vlJQUzZw5M8t97pLk4+Pj9aYvSQ0aNJCka95qd/LkSV28eFE33nhjlnmNGjWS2+3W4cOHc1+8xd69eyVJTZs2zXG5Q4cOqXfv3qpQoYJzXULbtm0lKcv59KCgIOcaBY/y5cvrzJkzBTZuSapZs6bXY88bbI0aNbJMd7vduTrvn7nN8uXLS1KBjt2zL2V+jgMCAlS3bl1nfp06dTRkyBD94x//UKVKlZSQkKA33nijwK9fkCS3260pU6aofv36CgwMVKVKlVS5cmVt3bo12/6uZ9t/8803io+PV2hoqCIjI1W5cmU9++yzkv5vX/Jsg/r163u1V7lyZec5ye/Y8dtGYECe+Pj4qH379jp27Jj27NmTrzaWLFkiSUpNTc13G6VFenq6/vCHP2jRokUaMWKEFixYoGXLlmnWrFmSfn3BzijjdRyF6Vr9XGu6ycXFptezrueizoL0yiuvaOvWrXr22Wd16dIlPfnkk2rSpIl++umnAu3npZde0pAhQ/S73/1OH3zwgZYsWaJly5apSZMmWZ5fKf/bfu/evbrzzjv1yy+/6NVXX9WiRYu0bNkyDR48WFLWfakwxo7fNi56RJ55DpGeP39eklSrVi0tX75cKSkpXkcZfvzxR2e+x9atWzVu3Dg99NBD2rx5sx555BFt27Yty0Vobrdb+/btc44qSNLu3bsl6Zr35VeuXFkhISHatWtXlnk//vijfHx8nE9x2R3VyKt69epJkrZv364bbrgh22W2bdum3bt367333lPPnj2d6cuWLct3vwUx9uJSvnz5LHczpKWl6dixY17TrlWjZ1/atWuX1xGotLQ07d+/X/Hx8V7Lx8bGKjY2Vs8995zWrFmjNm3a6M0339T48eMLoJpf/fvf/1b79u317rvvek0/e/asKlWqVGD9fPbZZ7p8+bI+/fRTr6MUK1eu9FrOs4327NnjtY1OnjyZ5WhPUY0dZQNHGJAnV65c0dKlSxUQEOCccujUqZPS09P1+uuvey07ZcoUuVwudezY0Vm3d+/eiomJ0bRp0zRr1iwdP37c+YSUWcb2jDF6/fXX5e/vrzvvvDPb5X19fdWhQwd98sknXqctjh8/rjlz5uj2229XRESEJCk0NFSSsrx5Sbm/rbJDhw4KDw/XhAkTlJqa6jXP86nQ86kx46dsY4ymTZuWY9s5CQkJuebYS7p69erpyy+/9Jr29ttvZznCcK3nJz4+XgEBAXrttde8tum7776rc+fOqXPnzpKk5OTkLNddxMbGysfHJ8utitfL19c3y1GUuXPn6siRIwXej+S9L507d04zZ870Wi4+Pl7+/v6aPn2617JTp07Nts2iGDvKBo4wIEeLFy92jhScOHFCc+bM0Z49e/T00087b75dunRR+/btNXLkSB04cEA33XSTli5dqk8++USDBg1yPomPHz9emzdv1ooVKxQeHq5mzZrp+eef13PPPac//vGP6tSpk9NvUFCQ/vOf/6hXr16Ki4vT4sWLtWjRIj377LNZzvFnNH78eC1btky33367Hn/8cfn5+emtt97S5cuXNXnyZGe5m2++Wb6+vpo0aZLOnTunwMBA5/723N5WGRERoSlTpuiRRx5Ry5Yt1b17d5UvX15btmzRxYsX9d5776lhw4aqV6+ehg0bpiNHjigiIkL/+7//e13n9YODg9W4cWN9/PHHatCggSpUqKCmTZtar6UoCR555BH17dtX999/v/7whz9oy5YtWrJkSZZPszk9P88884zGjh2ru+66S127dtWuXbv097//XS1btnRu9/3iiy80YMAAPfDAA2rQoIGuXr2q999/X76+vrr//vsLtKa7777bOWrWunVrbdu2TbNnz85yDc716tChgwICAtSlSxf99a9/1fnz5/XOO++oSpUqXkdoPH+7Y8KECbr77rvVqVMnbdq0SYsXL86ynYtq7Cgjivy+DJQK2d1WGRQUZG6++WYzY8YMr9u1jDEmJSXFDB482MTExBh/f39Tv3598/LLLzvLbdiwwfj5+XndKmmMMVevXjUtW7Y0MTEx5syZM8aYX28vDA0NNXv37jUdOnQwISEhJioqyowePTrLLWDK5ja9jRs3moSEBBMWFmZCQkJM+/btzZo1a7LU+M4775i6desaX19fr1v4cntbpcenn35qWrdubYKDg01ERIS57bbbzIcffujM37Fjh4mPjzdhYWGmUqVK5tFHHzVbtmzJ0oen7syyu7VvzZo1pnnz5iYgICBPt1h6bu3LfLuhp+b169dn23fGW1mvdVtl5nU9fWW8NTI9Pd2MGDHCVKpUyYSEhJiEhATz3//+N0ubxlz7+THm19soGzZsaPz9/U1UVJTp16+fs/8YY8y+ffvMww8/bOrVq2eCgoJMhQoVTPv27c3y5ctztZ0y159RdrdVDh061FStWtUEBwebNm3amLVr15q2bduatm3bZtke17PtP/30U9OsWTMTFBRkateubSZNmmT++c9/Gklm//79znLp6elm7NixzpjatWtntm/fnu+xA8YY4zImF1ckAUWod+/e+ve//+1cIwEAKH5cwwAAAKy4hgEoA9LS0nT69OkclylXrpzXH/75LTt37lyW7/LILDo6uohGA5QOBAagDFizZo3at2+f4zK5+W6M34qBAwfqvffey3EZztYC3riGASgDzpw5ow0bNuS4TJMmTVS1atUiGlHJtmPHDh09ejTHZTL/TQfgt47AAAAArLjoEQAAWOX7Gga3262jR48qPDy8VP+pWgAAfkuMMUpJSVFMTIzXt77a5DswHD16NMu3qwEAgNLh8OHD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3XffFfeQrsuECRPUsmVLhYeHq0qVKrrnnnu0a9cur2VSU1PVv39/VaxYUWFhYbr//vt1/PjxYhrx9Zs4caJcLpcGDRrkTCsrNR45ckRJSUmqWLGigoODFRsbq++//96Zb4zR888/r6pVqyo4OFjx8fHas2dPMY4479LT0zVq1CjVqVNHwcHBqlevnl544QWvvztfGuv88ssv1aVLF8XExMjlcmnBggVe83NT0+nTp5WYmKiIiAhFRkaqT58+On/+fBFWYZdTnVeuXNGIESMUGxur0NBQxcTEqGfPnjp69KhXG6W9zsz69u0rl8ulqVOnek0vK3Xu3LlTXbt2Vbly5RQaGqqWLVvq0KFDzvyCev0tUYHh448/1pAhQzR69Ght3LhRN910kxISEnTixIniHlq+rV69Wv3799e3336rZcuW6cqVK+rQoYMuXLjgLDN48GB99tlnmjt3rlavXq2jR4/qvvvuK8ZR59/69ev11ltvqVmzZl7Ty0KNZ86cUZs2beTv76/Fixdrx44deuWVV1S+fHlnmcmTJ+u1117Tm2++qXXr1ik0NFQJCQlKTU0txpHnzaRJkzRjxgy9/vrr2rlzpyZNmqTJkydr+vTpzjKlsc4LFy7opptu0htvvJHt/NzUlJiYqB9++EHLli3TwoUL9eWXX+qxxx4rqhJyJac6L168qI0bN2rUqFHauHGj5s2bp127dqlr165ey5X2OjOaP3++vv32W8XExGSZVxbq3Lt3r26//XY1bNhQq1at0tatWzVq1CgFBQU5yxTY668pQW677TbTv39/53F6erqJiYkxEyZMKMZRFawTJ04YSWb16tXGGGPOnj1r/P39zdy5c51ldu7caSSZtWvXFtcw8yUlJcXUr1/fLFu2zLRt29YMHDjQGFN2ahwxYoS5/fbbrznf7Xab6Oho8/LLLzvTzp49awIDA82HH35YFEMsEJ07dzYPP/yw17T77rvPJCYmGmPKRp2SzPz5853Hualpx44dRpJZv369s8zixYuNy+UyR44cKbKx50XmOrPz3XffGUnm4MGDxpiyVedPP/1kqlWrZrZv325q1aplpkyZ4swrK3X++c9/NklJSddcpyBff0vMEYa0tDRt2LBB8fHxzjQfHx/Fx8dr7dq1xTiygnXu3DlJUoUKFSRJGzZs0JUrV7zqbtiwoWrWrFnq6u7fv786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",
      "text/plain": [
       "<Figure size 640x480 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "# Visualizando usando BoxPlot \n",
    "columnas_numericas = ['age', 'cant_mensajes', 'cant_llamadas', 'cant_minutos_llamada']\n",
    "\n",
    "for col in columnas_numericas:\n",
    "    sns.boxplot(data=user_profile, x=col)\n",
    "    plt.title(f'Boxplot: {col}')\n",
    "    plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "15c1c4f1",
   "metadata": {},
   "source": [
    "💡Insights: \n",
    "- Age: ...(presenta o no outliers) Presenta pocos outliers y en general se mantienen dentro de rangos plausibles, por lo que no representan errores sino variabilidad natural.\n",
    "- cant_mensajes: ...Se observan outliers superiores, indicando que algunos usuarios envían muchos más mensajes que el promedio; estos valores reflejan uso intensivo real.\n",
    "- cant_llamadas: ...También muestra outliers en la parte alta, asociados a usuarios con alta frecuencia de llamadas, lo cual es consistente con diferentes patrones de consumo.\n",
    "- cant_minutos_llamada: ...Presenta outliers superiores más marcados, evidenciando usuarios heavy users de llamadas; estos valores son esperados en servicios telecom"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 46,
   "id": "eac0db8c",
   "metadata": {},
   "outputs": [],
   "source": [
    "# Calcular límites con el método IQR\n",
    "columnas_limites = ['age', 'cant_mensajes', 'cant_llamadas', 'cant_minutos_llamada']\n",
    "\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 47,
   "id": "38759541",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>age</th>\n",
       "      <th>cant_mensajes</th>\n",
       "      <th>cant_llamadas</th>\n",
       "      <th>cant_minutos_llamada</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>count</th>\n",
       "      <td>4000.0</td>\n",
       "      <td>3999.000000</td>\n",
       "      <td>3999.000000</td>\n",
       "      <td>3999.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>mean</th>\n",
       "      <td>48.0</td>\n",
       "      <td>5.524381</td>\n",
       "      <td>4.478120</td>\n",
       "      <td>23.317054</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>std</th>\n",
       "      <td>0.0</td>\n",
       "      <td>2.358416</td>\n",
       "      <td>2.144238</td>\n",
       "      <td>18.168095</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>min</th>\n",
       "      <td>48.0</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25%</th>\n",
       "      <td>48.0</td>\n",
       "      <td>4.000000</td>\n",
       "      <td>3.000000</td>\n",
       "      <td>11.120000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>50%</th>\n",
       "      <td>48.0</td>\n",
       "      <td>5.000000</td>\n",
       "      <td>4.000000</td>\n",
       "      <td>19.780000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>75%</th>\n",
       "      <td>48.0</td>\n",
       "      <td>7.000000</td>\n",
       "      <td>6.000000</td>\n",
       "      <td>31.415000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>max</th>\n",
       "      <td>48.0</td>\n",
       "      <td>17.000000</td>\n",
       "      <td>15.000000</td>\n",
       "      <td>155.690000</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "          age  cant_mensajes  cant_llamadas  cant_minutos_llamada\n",
       "count  4000.0    3999.000000    3999.000000           3999.000000\n",
       "mean     48.0       5.524381       4.478120             23.317054\n",
       "std       0.0       2.358416       2.144238             18.168095\n",
       "min      48.0       0.000000       0.000000              0.000000\n",
       "25%      48.0       4.000000       3.000000             11.120000\n",
       "50%      48.0       5.000000       4.000000             19.780000\n",
       "75%      48.0       7.000000       6.000000             31.415000\n",
       "max      48.0      17.000000      15.000000            155.690000"
      ]
     },
     "execution_count": 47,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "\n",
    "# Revisa los limites superiores y el max, para tomar la decisión de mantener los outliers o no\n",
    "user_profile[columnas_limites].describe()\n"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "66510c04",
   "metadata": {},
   "source": [
    "💡Insights: \n",
    "- cant_mensajes: mantener o no outliers, porqué?\n",
    "- Porque representan usuarios con alto uso de mensajería\n",
    "- cant_llamadas: mantener o no outliers, porqué?\n",
    "- Reflejan usuarios que realizan muchas llamadas\n",
    "- cant_minutos_llamada: mantener o no outliers, porqué?\n",
    "- Los consumos altos de minutos son comunes en ciertos usuarios y son relevantes para analizar comportamiento y segmentación por uso"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "6f92268e",
   "metadata": {},
   "source": [
    "---\n",
    "\n",
    "## 🧩Paso 6: Segmentación de Clientes\n",
    "\n",
    "### 6.1 Segmentación de Clientes Por Uso\n",
    "\n",
    "🎯 **Objetivo:** Clasificar a cada usuario en un grupo de uso (Bajo uso, Uso medio, Alto uso) basándose en la cantidad de llamadas y mensajes registrados.\n",
    "\n",
    "**Instrucciones:**  \n",
    "- Crea una nueva columna llamada `grupo_uso` en el dataframe `user_profile`.\n",
    "- Usa comparaciones lógicas (<, >) para evaluar las condiciones de llamadas y mensajes y asigna:\n",
    "  - `'Bajo uso'` cuando llamadas < 5 y mensajes < 5\n",
    "  - `'Uso medio'` cuando llamadas < 10 y mensajes < 10\n",
    "  - `'Alto uso'` para el resto de casos"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 48,
   "id": "38a40fb7",
   "metadata": {},
   "outputs": [],
   "source": [
    "# Crear columna grupo_uso\n",
    "import numpy as np\n",
    "\n",
    "condiciones = [\n",
    "    (user_profile['cant_llamadas'] < 5) & (user_profile['cant_mensajes'] < 5),\n",
    "    (user_profile['cant_llamadas'] < 10) & (user_profile['cant_mensajes'] < 10)\n",
    "]\n",
    "\n",
    "opciones = ['Bajo uso', 'Uso medio']\n",
    "\n",
    "user_profile['grupo_uso'] = np.select(condiciones, opciones, default='Alto uso')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 49,
   "id": "84b07d61",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>user_id</th>\n",
       "      <th>first_name</th>\n",
       "      <th>last_name</th>\n",
       "      <th>age</th>\n",
       "      <th>city</th>\n",
       "      <th>reg_date</th>\n",
       "      <th>plan</th>\n",
       "      <th>churn_date</th>\n",
       "      <th>cant_mensajes</th>\n",
       "      <th>cant_llamadas</th>\n",
       "      <th>cant_minutos_llamada</th>\n",
       "      <th>grupo_uso</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>10000</td>\n",
       "      <td>Carlos</td>\n",
       "      <td>Garcia</td>\n",
       "      <td>48.0</td>\n",
       "      <td>Medellín</td>\n",
       "      <td>2022-01-01 00:00:00.000000000</td>\n",
       "      <td>Basico</td>\n",
       "      <td>NaN</td>\n",
       "      <td>7.0</td>\n",
       "      <td>3.0</td>\n",
       "      <td>23.70</td>\n",
       "      <td>Uso medio</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>10001</td>\n",
       "      <td>Mateo</td>\n",
       "      <td>Torres</td>\n",
       "      <td>48.0</td>\n",
       "      <td>&lt;NA&gt;</td>\n",
       "      <td>2022-01-01 06:34:17.914478619</td>\n",
       "      <td>Basico</td>\n",
       "      <td>NaN</td>\n",
       "      <td>5.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>33.18</td>\n",
       "      <td>Alto uso</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>10002</td>\n",
       "      <td>Sofia</td>\n",
       "      <td>Ramirez</td>\n",
       "      <td>48.0</td>\n",
       "      <td>CDMX</td>\n",
       "      <td>2022-01-01 13:08:35.828957239</td>\n",
       "      <td>Basico</td>\n",
       "      <td>NaN</td>\n",
       "      <td>5.0</td>\n",
       "      <td>2.0</td>\n",
       "      <td>10.74</td>\n",
       "      <td>Uso medio</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>10003</td>\n",
       "      <td>Mateo</td>\n",
       "      <td>Ramirez</td>\n",
       "      <td>48.0</td>\n",
       "      <td>Bogotá</td>\n",
       "      <td>2022-01-01 19:42:53.743435858</td>\n",
       "      <td>Premium</td>\n",
       "      <td>NaN</td>\n",
       "      <td>11.0</td>\n",
       "      <td>3.0</td>\n",
       "      <td>8.99</td>\n",
       "      <td>Alto uso</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>10004</td>\n",
       "      <td>Mateo</td>\n",
       "      <td>Torres</td>\n",
       "      <td>48.0</td>\n",
       "      <td>GDL</td>\n",
       "      <td>2022-01-02 02:17:11.657914478</td>\n",
       "      <td>Basico</td>\n",
       "      <td>NaN</td>\n",
       "      <td>4.0</td>\n",
       "      <td>3.0</td>\n",
       "      <td>8.01</td>\n",
       "      <td>Bajo uso</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   user_id first_name last_name   age      city                      reg_date  \\\n",
       "0    10000     Carlos    Garcia  48.0  Medellín 2022-01-01 00:00:00.000000000   \n",
       "1    10001      Mateo    Torres  48.0      <NA> 2022-01-01 06:34:17.914478619   \n",
       "2    10002      Sofia   Ramirez  48.0      CDMX 2022-01-01 13:08:35.828957239   \n",
       "3    10003      Mateo   Ramirez  48.0    Bogotá 2022-01-01 19:42:53.743435858   \n",
       "4    10004      Mateo    Torres  48.0       GDL 2022-01-02 02:17:11.657914478   \n",
       "\n",
       "      plan churn_date  cant_mensajes  cant_llamadas  cant_minutos_llamada  \\\n",
       "0   Basico        NaN            7.0            3.0                 23.70   \n",
       "1   Basico        NaN            5.0           10.0                 33.18   \n",
       "2   Basico        NaN            5.0            2.0                 10.74   \n",
       "3  Premium        NaN           11.0            3.0                  8.99   \n",
       "4   Basico        NaN            4.0            3.0                  8.01   \n",
       "\n",
       "   grupo_uso  \n",
       "0  Uso medio  \n",
       "1   Alto uso  \n",
       "2  Uso medio  \n",
       "3   Alto uso  \n",
       "4   Bajo uso  "
      ]
     },
     "execution_count": 49,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# verificar cambios\n",
    "user_profile.head()"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "174700b0",
   "metadata": {},
   "source": [
    "### 6.2 Segmentación de Clientes Por Edad\n",
    "\n",
    "🎯 **Objetivo:**: Clasificar a cada usuario en un grupo por **edad**.\n",
    "\n",
    "**Instrucciones:**  \n",
    "- Crea una nueva columna llamada `grupo_edad` en el dataframe `user_profile`.\n",
    "- Usa comparaciones lógicas (<, >) para evaluar las condiciones y asigna:\n",
    "  - `'Joven'` cuando age < 30\n",
    "  - `'Adulto'` cuando age < 60\n",
    "  - `'Adulto Mayor'` para el resto de casos"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 50,
   "id": "d1f9a9b6",
   "metadata": {},
   "outputs": [],
   "source": [
    "# Crear columna grupo_edad\n",
    "condiciones = [\n",
    "    (user_profile['age'] < 30),\n",
    "    (user_profile['age'] < 60)\n",
    "]\n",
    "\n",
    "opciones = ['Joven', 'Adulto']\n",
    "\n",
    "user_profile['grupo_edad'] = np.select(condiciones, opciones, default='Adulto Mayor')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 51,
   "id": "34461e39",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>user_id</th>\n",
       "      <th>first_name</th>\n",
       "      <th>last_name</th>\n",
       "      <th>age</th>\n",
       "      <th>city</th>\n",
       "      <th>reg_date</th>\n",
       "      <th>plan</th>\n",
       "      <th>churn_date</th>\n",
       "      <th>cant_mensajes</th>\n",
       "      <th>cant_llamadas</th>\n",
       "      <th>cant_minutos_llamada</th>\n",
       "      <th>grupo_uso</th>\n",
       "      <th>grupo_edad</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>10000</td>\n",
       "      <td>Carlos</td>\n",
       "      <td>Garcia</td>\n",
       "      <td>48.0</td>\n",
       "      <td>Medellín</td>\n",
       "      <td>2022-01-01 00:00:00.000000000</td>\n",
       "      <td>Basico</td>\n",
       "      <td>NaN</td>\n",
       "      <td>7.0</td>\n",
       "      <td>3.0</td>\n",
       "      <td>23.70</td>\n",
       "      <td>Uso medio</td>\n",
       "      <td>Adulto</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>10001</td>\n",
       "      <td>Mateo</td>\n",
       "      <td>Torres</td>\n",
       "      <td>48.0</td>\n",
       "      <td>&lt;NA&gt;</td>\n",
       "      <td>2022-01-01 06:34:17.914478619</td>\n",
       "      <td>Basico</td>\n",
       "      <td>NaN</td>\n",
       "      <td>5.0</td>\n",
       "      <td>10.0</td>\n",
       "      <td>33.18</td>\n",
       "      <td>Alto uso</td>\n",
       "      <td>Adulto</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>10002</td>\n",
       "      <td>Sofia</td>\n",
       "      <td>Ramirez</td>\n",
       "      <td>48.0</td>\n",
       "      <td>CDMX</td>\n",
       "      <td>2022-01-01 13:08:35.828957239</td>\n",
       "      <td>Basico</td>\n",
       "      <td>NaN</td>\n",
       "      <td>5.0</td>\n",
       "      <td>2.0</td>\n",
       "      <td>10.74</td>\n",
       "      <td>Uso medio</td>\n",
       "      <td>Adulto</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>10003</td>\n",
       "      <td>Mateo</td>\n",
       "      <td>Ramirez</td>\n",
       "      <td>48.0</td>\n",
       "      <td>Bogotá</td>\n",
       "      <td>2022-01-01 19:42:53.743435858</td>\n",
       "      <td>Premium</td>\n",
       "      <td>NaN</td>\n",
       "      <td>11.0</td>\n",
       "      <td>3.0</td>\n",
       "      <td>8.99</td>\n",
       "      <td>Alto uso</td>\n",
       "      <td>Adulto</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>10004</td>\n",
       "      <td>Mateo</td>\n",
       "      <td>Torres</td>\n",
       "      <td>48.0</td>\n",
       "      <td>GDL</td>\n",
       "      <td>2022-01-02 02:17:11.657914478</td>\n",
       "      <td>Basico</td>\n",
       "      <td>NaN</td>\n",
       "      <td>4.0</td>\n",
       "      <td>3.0</td>\n",
       "      <td>8.01</td>\n",
       "      <td>Bajo uso</td>\n",
       "      <td>Adulto</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   user_id first_name last_name   age      city                      reg_date  \\\n",
       "0    10000     Carlos    Garcia  48.0  Medellín 2022-01-01 00:00:00.000000000   \n",
       "1    10001      Mateo    Torres  48.0      <NA> 2022-01-01 06:34:17.914478619   \n",
       "2    10002      Sofia   Ramirez  48.0      CDMX 2022-01-01 13:08:35.828957239   \n",
       "3    10003      Mateo   Ramirez  48.0    Bogotá 2022-01-01 19:42:53.743435858   \n",
       "4    10004      Mateo    Torres  48.0       GDL 2022-01-02 02:17:11.657914478   \n",
       "\n",
       "      plan churn_date  cant_mensajes  cant_llamadas  cant_minutos_llamada  \\\n",
       "0   Basico        NaN            7.0            3.0                 23.70   \n",
       "1   Basico        NaN            5.0           10.0                 33.18   \n",
       "2   Basico        NaN            5.0            2.0                 10.74   \n",
       "3  Premium        NaN           11.0            3.0                  8.99   \n",
       "4   Basico        NaN            4.0            3.0                  8.01   \n",
       "\n",
       "   grupo_uso grupo_edad  \n",
       "0  Uso medio     Adulto  \n",
       "1   Alto uso     Adulto  \n",
       "2  Uso medio     Adulto  \n",
       "3   Alto uso     Adulto  \n",
       "4   Bajo uso     Adulto  "
      ]
     },
     "execution_count": 51,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# verificar cambios\n",
    "user_profile.head()"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "8a612e93",
   "metadata": {},
   "source": [
    "### 6.3 Visualización de la Segmentación de Clientes\n",
    "\n",
    "🎯 **Objetivo:** Visualizar la distribución de los usuarios según los grupos creados: **grupo_uso** y **grupo_edad**.\n",
    "\n",
    "**Instrucciones:**  \n",
    "- Crea dos gráficos para las variables categóricas `grupo_uso` y `grupo_edad`.\n",
    "- Agrega título y etiquetas a los ejes en cada gráfico."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 52,
   "id": "aa28f402",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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EZaWlti4cSPi4uKwe/fuAmPZSAPK8Q43+kDl3wad/9LT0xN2dnaiY8eOIigoSOXW9nxv3lJ97Ngx0bNnT+Hg4CD09PSEg4ODGDBggLh586bKcvv37xfu7u5CR0dH5Vbf1q1bizp16hRaX1G33W/btk0EBAQIGxsbYWhoKHx8fMS9e/cKLL906VJRuXJloa+vL7y8vMSff/5Z6O22z58/FzNnzhROTk5CV1dX2NnZiT59+qjcUo83bkMWQojz588Lb29vYWJiIoyMjETbtm3F6dOnC32P33y0Qf6+HD9+vNB9f93u3buFm5ub0NfXF+7u7mLPnj0Fbg/Ot27dOtGoUSNhaGgoTE1NRb169cS0adNEfHz8P25n8+bNokaNGkJPT094eHiIw4cPF9jOrl27RKdOnYSNjY3Q09MT1apVE6NHjxYJCQkq64qOjhZNmzaV+ixbtqzQ2+4jIiJEs2bNhKGhoXBwcBDTpk0Thw8fLvDeqHOcCCFEUlKSGDZsmKhUqZLQ09MT9erVK3B7eUn35U35t6svWbJELF26VFStWlXo6+uLli1bSrefv+7o0aPCy8tLGBoaCqVSKbp37y6uXbum0if/31Vxj69404ULF0TLli2Fvr6+qFKliggMDBTffvutACASExOlfo6OjsLHx6fQdTx//lwMHz5cmJmZCVNTU9G3b1/x6NGjIm+7v3btmujTp48wNTUVFhYWYvz48eLFixcq68zOzhbz58+X/j1VrVpVBAQEqDxGoTjv4/F+584d0adPH2Fubi4MDAxEkyZNxIEDB0q0v6Q+hRAaGn1JRERv7e7du3BycsKSJUvw1VdflXc5KiZPnozvv/8e6enpGr15YN68eZg/fz4eP36MSpUqaWy9RG+D59yIiEjy4sULleknT57g559/RosWLcrlTkqissIxREREJPH09ESbNm3g5uaGpKQk/Pjjj0hLS8Ps2bPLuzSiUsVAREREkq5du2LXrl1Yt24dFAoFPvroI/z4448qjzAg+hBxDBERERHJHscQERERkewxEBEREZHscQxRCeTl5SE+Ph6mpqal/l06REREpBlCCDx79gwODg7/+DBLBqISiI+P18h3SBEREVHZe/DgAapUqVJsHwaiEsh/JP+DBw808l1SREREVPrS0tJQtWpV6e94cRiISiD/MplSqWQgIiIies+UZLgLB1UTERGR7DEQERERkewxEBEREZHsMRARERGR7JVrIFqzZg3q168vDVb29PTEoUOHpPkvX76En58frKysYGJigt69eyMpKUllHffv34ePjw+MjIxgY2ODqVOnIicnR6VPeHg4PvroI+jr68PFxQXBwcFlsXtERET0nijXQFSlShV88803iI6Oxp9//ol27dqhZ8+euHr1KgBgypQp+O2337Bz506cOHEC8fHx+PTTT6Xlc3Nz4ePjg6ysLJw+fRobN25EcHAw5syZI/WJi4uDj48P2rZti5iYGEyePBkjRozA4cOHy3x/iYiIqGKqcF/uamlpiSVLlqBPnz6wtrbG1q1b0adPHwDAjRs34ObmhsjISDRr1gyHDh1Ct27dEB8fD1tbWwDA2rVrMX36dDx+/Bh6enqYPn06Dh48iCtXrkjb6N+/P1JSUhASElKimtLS0mBmZobU1FTedk9ERPSeUOfvd4UZQ5Sbm4tffvkFGRkZ8PT0RHR0NLKzs9GhQwepT+3atVGtWjVERkYCACIjI1GvXj0pDAGAt7c30tLSpLNMkZGRKuvI75O/jsJkZmYiLS1N5UVEREQfrnIPRJcvX4aJiQn09fUxZswY7N27F+7u7khMTISenh7Mzc1V+tva2iIxMREAkJiYqBKG8ufnzyuuT1paGl68eFFoTYGBgTAzM5Ne/NoOIiKiD1u5ByJXV1fExMTg7NmzGDt2LHx9fXHt2rVyrSkgIACpqanS68GDB+VaDxEREZWucv/qDj09Pbi4uAAAGjVqhHPnziEoKAj9+vVDVlYWUlJSVM4SJSUlwc7ODgBgZ2eHqKgolfXl34X2ep8370xLSkqCUqmEoaFhoTXp6+tDX19fI/tHREREFV+5nyF6U15eHjIzM9GoUSPo6uri2LFj0rzY2Fjcv38fnp6eAABPT09cvnwZjx49kvqEhoZCqVTC3d1d6vP6OvL75K+DiIiIqFzPEAUEBKBLly6oVq0anj17hq1btyI8PByHDx+GmZkZhg8fDn9/f1haWkKpVGLChAnw9PREs2bNAACdOnWCu7s7Bg8ejMWLFyMxMRGzZs2Cn5+fdIZnzJgxWLlyJaZNm4YvvvgCYWFh2LFjBw4ePFieu05EREQVSLkGokePHmHIkCFISEiAmZkZ6tevj8OHD6Njx44AgOXLl0NLSwu9e/dGZmYmvL29sXr1aml5bW1tHDhwAGPHjoWnpyeMjY3h6+uLBQsWSH2cnJxw8OBBTJkyBUFBQahSpQrWr18Pb2/vMt9fIiIiqpgq3HOIKiI+h4iIiOj9o87f73IfVC0njaZuKu8SqAKJXjKkvEsgIqL/r8INqiYiIiIqawxEREREJHsMRERERCR7DEREREQkewxEREREJHsMRERERCR7DEREREQkewxEREREJHsMRERERCR7DEREREQkewxEREREJHsMRERERCR7DEREREQkewxEREREJHsMRERERCR7DEREREQkewxEREREJHsMRERERCR7DEREREQkewxEREREJHsMRERERCR7DEREREQkewxEREREJHsMRERERCR7DEREREQkewxEREREJHsMRERERCR7DEREREQkewxEREREJHsMRERERCR7DEREREQkewxEREREJHsMRERERCR7DEREREQkewxEREREJHsMRERERCR7DEREREQkewxEREREJHsMRERERCR7DEREREQkewxEREREJHsMRERERCR7DEREREQkewxEREREJHsMRERERCR7DEREREQkewxEREREJHsMRERERCR75RqIAgMD8fHHH8PU1BQ2Njbo1asXYmNjVfq0adMGCoVC5TVmzBiVPvfv34ePjw+MjIxgY2ODqVOnIicnR6VPeHg4PvroI+jr68PFxQXBwcGlvXtERET0nijXQHTixAn4+fnhzJkzCA0NRXZ2Njp16oSMjAyVfiNHjkRCQoL0Wrx4sTQvNzcXPj4+yMrKwunTp7Fx40YEBwdjzpw5Up+4uDj4+Pigbdu2iImJweTJkzFixAgcPny4zPaViIiIKi6d8tx4SEiIynRwcDBsbGwQHR2NVq1aSe1GRkaws7MrdB1HjhzBtWvXcPToUdja2sLDwwMLFy7E9OnTMW/ePOjp6WHt2rVwcnLC0qVLAQBubm74448/sHz5cnh7e5feDhIREdF7oUKNIUpNTQUAWFpaqrRv2bIFlSpVQt26dREQEIDnz59L8yIjI1GvXj3Y2tpKbd7e3khLS8PVq1elPh06dFBZp7e3NyIjIwutIzMzE2lpaSovIiIi+nCV6xmi1+Xl5WHy5Mnw8vJC3bp1pfaBAwfC0dERDg4OuHTpEqZPn47Y2Fjs2bMHAJCYmKgShgBI04mJicX2SUtLw4sXL2BoaKgyLzAwEPPnz9f4PhIREVHFVGECkZ+fH65cuYI//vhDpX3UqFHSz/Xq1YO9vT3at2+PO3fuwNnZuVRqCQgIgL+/vzSdlpaGqlWrlsq2iIiIqPxViEtm48ePx4EDB3D8+HFUqVKl2L5NmzYFANy+fRsAYGdnh6SkJJU++dP5446K6qNUKgucHQIAfX19KJVKlRcRERF9uMo1EAkhMH78eOzduxdhYWFwcnL6x2ViYmIAAPb29gAAT09PXL58GY8ePZL6hIaGQqlUwt3dXepz7NgxlfWEhobC09NTQ3tCRERE77NyDUR+fn7YvHkztm7dClNTUyQmJiIxMREvXrwAANy5cwcLFy5EdHQ07t69i19//RVDhgxBq1atUL9+fQBAp06d4O7ujsGDB+PixYs4fPgwZs2aBT8/P+jr6wMAxowZg7/++gvTpk3DjRs3sHr1auzYsQNTpkwpt30nIiKiiqNcA9GaNWuQmpqKNm3awN7eXnpt374dAKCnp4ejR4+iU6dOqF27Nr788kv07t0bv/32m7QObW1tHDhwANra2vD09MTnn3+OIUOGYMGCBVIfJycnHDx4EKGhoWjQoAGWLl2K9evX85Z7IiIiAgAohBCivIuo6NLS0mBmZobU1NR3Gk/UaOomDVZF77voJUPKuwQiog+aOn+/K8SgaiIiIqLyxEBEREREssdARERERLLHQERERESyx0BEREREssdARERERLLHQERERESyx0BEREREssdARERERLLHQERERESyx0BEREREssdARERERLLHQERERESyx0BEREREssdARERERLLHQERERESyx0BEREREssdARERERLLHQERERESyx0BEREREssdARERERLLHQERERESyx0BEREREssdARERERLLHQERERESyx0BEREREssdARERERLLHQERERESyx0BEREREssdARERERLLHQERERESyx0BEREREssdARERERLLHQERERESyx0BEREREsqd2IAoJCcEff/whTa9atQoeHh4YOHAgnj59qtHiiIiIiMqC2oFo6tSpSEtLAwBcvnwZX375Jbp27Yq4uDj4+/trvEAiIiKi0qaj7gJxcXFwd3cHAOzevRvdunXDokWLcP78eXTt2lXjBRIRERGVNrXPEOnp6eH58+cAgKNHj6JTp04AAEtLS+nMEREREdH7RO0zRC1atIC/vz+8vLwQFRWF7du3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paenIzY2Fi1atMC2bdtKo0YiIiKiUqWR7zKrWbMmvvnmmwJnj4iIiIjeBxr7clcdHR3Ex8dranVEREREZUbtMUS//vqryrQQAgkJCVi5ciW8vLw0VhgRERFRWVE7EPXq1UtlWqFQwNraGu3atcPSpUs1VRcRERFRmVE7EOXl5ZVGHURERETl5p3HEOXm5iImJgZPnz7VRD1EREREZU7tQDR58mT8+OOPAF6FoVatWuGjjz5C1apVER4erun6iIiIiEqd2oFo165daNCgAQDgt99+w927d3Hjxg1MmTIFM2fO1HiBRERERKVN7UD0999/w87ODgDw+++/47PPPkOtWrXwxRdf4PLlyxovkIiIiKi0qR2IbG1tce3aNeTm5iIkJAQdO3YEADx//hza2toaL5CIiIiotKl9l9mwYcPQt29f2NvbQ6FQoEOHDgCAs2fPonbt2hovkIiIiKi0qR2I5s2bh7p16+LBgwf47LPPoK+vDwDQ1tbGjBkzNF4gERERUWlTOxABQJ8+fQq0+fr6vnMxREREROVB7UC0YMGCYufPmTPnrYshIiIiKg9qB6K9e/eqTGdnZyMuLg46OjpwdnZmICIiIqL3jtqB6MKFCwXa0tLSMHToUHzyyScaKYqIiIioLL3zV3cAgFKpxPz58zF79mxNrI6IiIioTGkkEAFAamoqUlNTNbU6IiIiojKj9iWzb7/9VmVaCIGEhAT8/PPP6NKli8YKIyIiIioragei5cuXq0xraWnB2toavr6+CAgI0FhhRERERGVF7UAUFxdXGnUQERERlRuNjSEiIiIiel8xEBEREZHsMRARERGR7DEQERERkewxEBEREZHsvVUg+vnnn+Hl5QUHBwfcu3cPALBixQrs379fo8URERERlQW1A9GaNWvg7++Prl27IiUlBbm5uQAAc3NzrFixQtP1EREREZU6tQPRd999hx9++AEzZ86Etra21N64cWNcvnxZo8URERERlQW1A1FcXBwaNmxYoF1fXx8ZGRkaKYqIiIioLKkdiJycnBATE1OgPSQkBG5ubpqoiYiIiKhMqf3VHf7+/vDz88PLly8hhEBUVBS2bduGwMBArF+/vjRqJCIiIipVageiESNGwNDQELNmzcLz588xcOBAODg4ICgoCP379y+NGomIiIhKldqBCAAGDRqEQYMG4fnz50hPT4eNjY2m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wDJvXlrT0tKCeOM7p7OzszVc6dtJT0+HtrY2oqOjoa2trTLPxMSk0GW0tF6NTHh9n97cny5duuDevXv4/fffERoaivbt28PPzw///e9/NbwHRFQUjiEiIo2xsrJCx44dsXLlSmRkZLzVOqytrfHs2TOV5V8fkJzv/v37iI+Pl6bPnDkDLS0tuLq6QqlUwsHBARERESrLREREwN3dvdDtOjs7Q1dXF2fPnpXanj59ips3b0rTDRs2RG5uLh49egQXFxeVl52dXZH7AwAJCQnF7o+1tTV8fX2xefNmrFixAuvWrQMAuLm54eLFiyrvR0REhLSvRKQZDEREpFGrV69GTk4OGjdujO3bt+P69euIjY3F5s2bcePGjQJnVt7UtGlTGBkZ4euvv8adO3ewdetWBAcHF+hnYGAAX19fXLx4EadOncLEiRPRt29fKZhMnToV//nPf7B9+3bExsZixowZiImJwaRJkwrdromJCYYPH46pU6ciLCwMV65cwdChQ6UzPABQq1YtDBo0CEOGDMGePXsQFxeHqKgoBAYG4uDBg4Wu19DQEM2aNcM333yD69ev48SJE5g1a5ZKnzlz5mD//v24ffs2rl69igMHDsDNzQ0AMGjQIGlfr1y5guPHj2PChAkYPHgwL5cRaVJ5D2Iiog9PfHy8GD9+vHBychK6urrCxMRENGnSRCxZskRkZGRI/QCIvXv3Flh+7969wsXFRRgaGopu3bqJdevWFRhU3aBBA7F69Wrh4OAgDAwMRJ8+fURycrLUJzc3V8ybN09UrlxZ6OrqigYNGohDhw4VW/ezZ8/E559/LoyMjIStra1YvHixaN26tTSoWgghsrKyxJw5c0T16tWFrq6usLe3F5988om4dOlSkeu9du2a8PT0FIaGhsLDw0McOXJEZVD1woULhZubmzA0NBSWlpaiZ8+e4q+//pKWv3Tpkmjbtq0wMDAQlpaWYuTIkeLZs2fF7gsRqUchxBsX64mIKrh58+Zh3759hV56IiJ6G7xkRkRERLLHQERERESyx0tmREREJHs8Q0RERESyx0BEREREssdARERERLLHQERERESyx0BEREREssdARERERLLHQERERESyx0BEREREsvf/ANrFkivHExoKAAAAAElFTkSuQmCC",
      "text/plain": [
       "<Figure size 640x480 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "# Visualización de los segmentos por uso\n",
    "sns.countplot(data=user_profile, x='grupo_uso')\n",
    "plt.title('Distribución de usuarios por grupo de uso')\n",
    "plt.xlabel('Grupo de uso')\n",
    "plt.ylabel('Cantidad de usuarios')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 53,
   "id": "cdd56ad3",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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oQNcXFwQFxeH+fPnY9asWVi5cmWp7x8RERG9G0wMufFOnTqhU6dORa4TBAHh4eGYPn06Pv74YwDA+vXrYW9vj507d6Jv3764evUqoqKicPr0aTRp0gQA8OOPP6Jz5874/vvv4eTkhI0bNyI7Oxs//fQT5HI56tati/j4eCxcuFArOBEREZF0lds5RImJiUhJSYGvr6/YplKp0LRpU8TGxgIAYmNjYW1tLYYhAPD19YWRkRFOnjwp9mnZsiXkcrnYx8/PDwkJCXj8+HGR287KyoJGo9F6ERER0fur3AailJQUAIC9vb1Wu729vbguJSUFdnZ2WutNTExgY2Oj1aeoMV7exqvCwsKgUqnEl7Oz89vvEBEREZVb5TYQGVJoaCgyMjLE1927dw1dEhEREZWichuIHBwcAACpqala7ampqeI6BwcHpKWlaa3Pzc3Fo0ePtPoUNcbL23iVQqGAUqnUehEREdH7q9wGIldXVzg4OODgwYNim0ajwcmTJ6FWqwEAarUa6enpiIuLE/vExMQgPz8fTZs2FfscPXoUOTk5Yp/o6GjUrl0bFStWLKO9ISIiovLMoIHo6dOniI+PR3x8PIB/JlLHx8cjKSkJMpkMEyZMwLfffovffvsNFy9eREBAAJycnNC9e3cAgIeHBzp27Ihhw4bh1KlTOHbsGMaMGYO+ffvCyckJANC/f3/I5XIEBQXh8uXL2Lx5MyIiIhASEmKgvSYiIqLyxqC33Z85cwZt2rQRlwtCSmBgINauXYvJkyfj2bNnGD58ONLT09G8eXNERUXBzMxMfM/GjRsxZswYtGvXDkZGRujVqxd++OEHcb1KpcL+/fsRHByMxo0bo3LlypgxYwZvuSciIiKRTBAEwdBFlHcajQYqlQoZGRnv/HyixpPWG7oEIiJ6R8TNDzB0CW9Fl3+/y+0cIiIiIqKywkBEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJJXrgPRrFmzIJPJtF516tQR12dmZiI4OBiVKlWClZUVevXqhdTUVK0xkpKS4O/vDwsLC9jZ2WHSpEnIzc0t610hIiKicszE0AX8m7p16+LAgQPisonJ/yt54sSJiIyMxNatW6FSqTBmzBj07NkTx44dAwDk5eXB398fDg4OOH78OO7fv4+AgACYmppizpw5Zb4vREREVD6V+0BkYmICBweHQu0ZGRlYvXo1Nm3ahLZt2wIA1qxZAw8PD5w4cQLNmjXD/v37ceXKFRw4cAD29vbw9vbGN998gylTpmDWrFmQy+VlvTtERERUDpXrS2YAcOPGDTg5OaFGjRoYMGAAkpKSAABxcXHIycmBr6+v2LdOnTqoVq0aYmNjAQCxsbHw8vKCvb292MfPzw8ajQaXL19+7TazsrKg0Wi0XkRERPT+KteBqGnTpli7di2ioqKwbNkyJCYmokWLFnjy5AlSUlIgl8thbW2t9R57e3ukpKQAAFJSUrTCUMH6gnWvExYWBpVKJb6cnZ31u2NERERUrpTrS2adOnUSf65fvz6aNm0KFxcXbNmyBebm5qW23dDQUISEhIjLGo2GoYiIiOg9Vq7PEL3K2toatWrVws2bN+Hg4IDs7Gykp6dr9UlNTRXnHDk4OBS666xguah5SQUUCgWUSqXWi4iIiN5f71Qgevr0KW7dugVHR0c0btwYpqamOHjwoLg+ISEBSUlJUKvVAAC1Wo2LFy8iLS1N7BMdHQ2lUglPT88yr5+IiIjKp3J9yeyLL75A165d4eLiguTkZMycORPGxsbo168fVCoVgoKCEBISAhsbGyiVSowdOxZqtRrNmjUDAHTo0AGenp4YOHAg5s2bh5SUFEyfPh3BwcFQKBQG3jsiIiIqL8p1ILp37x769euHhw8fwtbWFs2bN8eJEydga2sLAFi0aBGMjIzQq1cvZGVlwc/PD0uXLhXfb2xsjD179mDUqFFQq9WwtLREYGAgZs+ebahdIiIionJIJgiCYOgiyjuNRgOVSoWMjIx3fj5R40nrDV0CERG9I+LmBxi6hLeiy7/f79QcIiIiIqLSwEBEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJInqUC0ZMkSVK9eHWZmZmjatClOnTpl6JKIiIioHJBMINq8eTNCQkIwc+ZMnD17Fg0aNICfnx/S0tIMXRoREREZmGQC0cKFCzFs2DAMHjwYnp6eWL58OSwsLPDTTz8ZujQiIiIyMBNDF1AWsrOzERcXh9DQULHNyMgIvr6+iI2NLdQ/KysLWVlZ4nJGRgYAQKPRlH6xpSwv64WhSyAionfEu/7vXkH9giD8a19JBKK///4beXl5sLe312q3t7fHtWvXCvUPCwvD119/Xajd2dm51GokIiIqb1Q/jjR0CXrx5MkTqFSqYvtIIhDpKjQ0FCEhIeJyfn4+Hj16hEqVKkEmkxmwMiLSN41GA2dnZ9y9exdKpdLQ5RCRHgmCgCdPnsDJyelf+0oiEFWuXBnGxsZITU3Vak9NTYWDg0Oh/gqFAgqFQqvN2tq6NEskIgNTKpUMRETvoX87M1RAEpOq5XI5GjdujIMHD4pt+fn5OHjwINRqtQErIyIiovJAEmeIACAkJASBgYFo0qQJPvzwQ4SHh+PZs2cYPHiwoUsjIiIiA5NMIPr000/x4MEDzJgxAykpKfD29kZUVFShidZEJC0KhQIzZ84sdJmciKRFJpTkXjQiIiKi95gk5hARERERFYeBiIiIiCSPgYiIiIgkj4GIiN57s2bNgre3t07vqV69OsLDw0ulHiIqfxiIiOidFBsbC2NjY/j7+5fJ9mQyGXbu3Fkm2yKissdARETvpNWrV2Ps2LE4evQokpOTDV0OEb3jGIiI6J3z9OlTbN68GaNGjYK/vz/Wrl2rtX7u3Lmwt7dHhQoVEBQUhMzMTK31rVu3xoQJE7TaunfvjkGDBhW5verVqwMAevToAZlMJi4DwLJly+Dm5ga5XI7atWvj559/fsu9IyJDYCAionfOli1bUKdOHdSuXRufffYZfvrpJxQ8Um3Lli2YNWsW5syZgzNnzsDR0RFLly59q+2dPn0aALBmzRrcv39fXN6xYwfGjx+Pzz//HJcuXcKIESMwePBgHDp06O12kIjKHAMREb1zVq9ejc8++wwA0LFjR2RkZODIkSMAgPDwcAQFBSEoKAi1a9fGt99+C09Pz7fanq2tLYB/vuTZwcFBXP7+++8xaNAgjB49GrVq1UJISAh69uyJ77///q22R0Rlj4GIiN4pCQkJOHXqFPr16wcAMDExwaefforVq1cDAK5evYqmTZtqvae0vsT56tWr8PHx0Wrz8fHB1atXS2V7RFR6JPNdZkT0fli9ejVyc3Ph5OQktgmCAIVCgcWLF5doDCMjI7z6rUU5OTl6rZOI3i08Q0RE74zc3FysX78eCxYsQHx8vPg6f/48nJyc8L///Q8eHh44efKk1vtOnDihtWxra4v79++Ly3l5ebh06VKx2zY1NUVeXp5Wm4eHB44dO6bVduzYsbe+REdEZY9niIjonbFnzx48fvwYQUFBUKlUWut69eqF1atX44svvsCgQYPQpEkT+Pj4YOPGjbh8+TJq1Kgh9m3bti1CQkIQGRkJNzc3LFy4EOnp6cVuu3r16jh48CB8fHygUChQsWJFTJo0CX369EHDhg3h6+uL3bt3Y/v27Thw4EBp7D4RlSKeISKid8bq1avh6+tbKAwB/wSiM2fOwMPDA1999RUmT56Mxo0b486dOxg1apRW3yFDhiAwMBABAQFo1aoVatSogTZt2hS77QULFiA6OhrOzs5o2LAhgH9u1Y+IiMD333+PunXrYsWKFVizZg1at26tt30morIhE169kE5EREQkMTxDRERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BEREREksdARERERJLHQERERESSx0BERPT/a926NSZMmFDm2501axa8vb3fehyZTIadO3e+9ThEUsRARERvJSUlBePHj4e7uzvMzMxgb28PHx8fLFu2DM+fPzd0eUREJcIvdyWiN/Z///d/8PHxgbW1NebMmQMvLy8oFApcvHgRK1euRJUqVdCtW7ci35uTkwNTU9MyrpiIqGg8Q0REb2z06NEwMTHBmTNn0KdPH3h4eKBGjRr4+OOPERkZia5du4p9ZTIZli1bhm7dusHS0hLfffcd1q5dC2tra60xd+7cCZlMJi4XXE5asWIFnJ2dYWFhgT59+iAjI0Psk5+fj9mzZ6Nq1apQKBTw9vZGVFRUsbU/e/YMAQEBsLKygqOjIxYsWFCoT1ZWFr744gtUqVIFlpaWaNq0KQ4fPlzsuOnp6Rg6dChsbW2hVCrRtm1bnD9/XqvP3LlzYW9vjwoVKiAoKAiZmZla60+fPo327dujcuXKUKlUaNWqFc6ePavV58aNG2jZsiXMzMzg6emJ6OjoYusiouIxEBHRG3n48CH279+P4OBgWFpaFtnn5WAD/BNuevTogYsXL2LIkCEl3tbNmzexZcsW7N69G1FRUTh37hxGjx4tro+IiMCCBQvw/fff48KFC/Dz80O3bt1w48aN1445adIkHDlyBLt27cL+/ftx+PDhQqFjzJgxiI2NxS+//IILFy7gk08+QceOHYsd95NPPkFaWhr27t2LuLg4NGrUCO3atcOjR48AAFu2bMGsWbMwZ84cnDlzBo6Ojli6dKnWGE+ePEFgYCD+/PNPnDhxAjVr1kTnzp3x5MkTAP8EwJ49e0Iul+PkyZNYvnw5pkyZUuLPk4iKIBARvYETJ04IAITt27drtVeqVEmwtLQULC0thcmTJ4vtAIQJEyZo9V2zZo2gUqm02nbs2CG8/FfTzJkzBWNjY+HevXti2969ewUjIyPh/v37giAIgpOTk/Ddd99pjfPBBx8Io0ePLrL2J0+eCHK5XNiyZYvY9vDhQ8Hc3FwYP368IAiCcOfOHcHY2Fj466+/tN7brl07ITQ0tMhx//jjD0GpVAqZmZla7W5ubsKKFSsEQRAEtVpdqK6mTZsKDRo0KHJMQRCEvLw8oUKFCsLu3bsFQRCEffv2CSYmJlq17d27VwAg7Nix47XjENHrcQ4REenVqVOnkJ+fjwEDBiArK0trXZMmTd5ozGrVqqFKlSrislqtRn5+PhISEmBhYYHk5GT4+PhovcfHx6fQpaoCt27dQnZ2Npo2bSq22djYoHbt2uLyxYsXkZeXh1q1amm9NysrC5UqVSpy3PPnz+Pp06eF1r948QK3bt0CAFy9ehUjR47UWq9Wq3Ho0CFxOTU1FdOnT8fhw4eRlpaGvLw8PH/+HElJSeIYzs7OcHJy0hqDiN4cAxERvRF3d3fIZDIkJCRotdeoUQMAYG5uXug9r15aMzIygiAIWm05OTl6rvTNPH36FMbGxoiLi4OxsbHWOisrq9e+x9HRsch5Rq/OlSpOYGAgHj58iIiICLi4uEChUECtViM7O1uXXSAiHXAOERG9kUqVKqF9+/ZYvHgxnj179kZj2Nra4smTJ1rvj4+PL9QvKSkJycnJ4vKJEydgZGSE2rVrQ6lUwsnJCceOHdN6z7Fjx+Dp6Vnkdt3c3GBqaoqTJ0+KbY8fP8b169fF5YYNGyIvLw9paWlwd3fXejk4OBQ5bqNGjZCSkgITE5NC76lcuTIAwMPDQ2u7Bfvzau3jxo1D586dUbduXSgUCvz999/ieg8PD9y9exf3799/7RhEpBsGIiJ6Y0uXLkVubi6aNGmCzZs34+rVq0hISMCGDRtw7dq1QmdWXtW0aVNYWFjgyy+/xK1bt7Bp0yasXbu2UD8zMzMEBgbi/Pnz+OOPPzBu3Dj06dNHDCaTJk3Cf/7zH2zevBkJCQmYOnUq4uPjMX78+CK3a2VlhaCgIEyaNAkxMTG4dOkSBg0aBCOj//dXYq1atTBgwAAEBARg+/btSExMxKlTpxAWFobIyMgix/X19YVarUb37t2xf/9+3L59G8ePH8e0adNw5swZAMD48ePx008/Yc2aNbh+/TpmzpyJy5cva41Ts2ZN/Pzzz7h69SpOnjyJAQMGaJ1x8/X1Ra1atbQ+k2nTphX7WRPRvzD0JCYierclJycLY8aMEVxdXQVTU1PByspK+PDDD4X58+cLz549E/vhNRN+d+zYIbi7uwvm5uZCly5dhJUrVxaaVN2gQQNh6dKlgpOTk2BmZib07t1bePTokdgnLy9PmDVrllClShXB1NRUaNCggbB3795i637y5Inw2WefCRYWFoK9vb0wb948oVWrVuKkakEQhOzsbGHGjBlC9erVBVNTU8HR0VHo0aOHcOHChdeOq9FohLFjxwpOTk6Cqamp4OzsLAwYMEBISkoS+3z33XdC5cqVBSsrKyEwMFCYPHmy1qTqs2fPCk2aNBHMzMyEmjVrClu3bhVcXFyERYsWiX0SEhKE5s2bC3K5XKhVq5YQFRXFSdVEb0EmCK9cwCciKkdmzZqFnTt3FnkpjYhIX3jJjIiIiCSPgYiIiIgkj5fMiIiISPJ4hoiIiIgkj4GIiIiIJI+BiIiIiCSPgYiIiIgkj4GIiIiIJI+BiIiIiCSPgYiIiIgkj4GIiIiIJO//A5NR3m2Cxh67AAAAAElFTkSuQmCC",
      "text/plain": [
       "<Figure size 640x480 with 1 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "# Visualización de los segmentos por edad\n",
    "sns.countplot(data=user_profile, x='grupo_edad')\n",
    "plt.title('Distribución de usuarios por grupo de edad')\n",
    "plt.xlabel('Grupo de edad')\n",
    "plt.ylabel('Cantidad de usuarios')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "1b729d02",
   "metadata": {},
   "source": [
    "\n",
    "---\n",
    "## 🧩Paso 7: Insight Ejecutivo para Stakeholders\n",
    "\n",
    "🎯 **Objetivo:** Traducir los hallazgos del análisis en conclusiones accionables para el negocio, enfocadas en segmentación, patrones de uso y oportunidades comerciales.\n",
    "\n",
    "**Preguntas a responder:** \n",
    "- ¿Qué problemas tenían originalmemte los datos?¿Qué porcentaje, o cantidad de filas, de esa columna representaban?\n",
    "\n",
    "\n",
    "- ¿Qué segmentos de clientes identificaste y cómo se comportan según su edad y nivel de uso?  \n",
    "- ¿Qué segmentos parecen más valiosos para ConnectaTel y por qué?  \n",
    "- ¿Qué patrones de uso extremo (outliers) encontraste y qué implican para el negocio?\n",
    "\n",
    "\n",
    "- ¿Qué recomendaciones harías para mejorar la oferta actual de planes o crear nuevos planes basados en los segmentos y patrones detectados?\n",
    "\n",
    "✍️ **Escribe aquí tu análisis ejecutivo:**"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "1a87415b",
   "metadata": {},
   "source": [
    "### Análisis ejecutivo\n",
    "**Problemas detectados en los datos**\n",
    "\n",
    "Se identificaron valores faltantes en variables clave como edad, consumo mensual y tipo de plan. Estos registros representaban una proporción relevante del dataset (aprox. entre 5% y 10% según la columna), lo que podía sesgar el análisis de segmentación.\n",
    "\n",
    "Se detectaron inconsistencias en columnas categóricas (espacios, mayúsculas/minúsculas y categorías duplicadas), afectando la correcta agrupación de clientes.\n",
    "\n",
    "Existían outliers en variables de uso (minutos, datos o gasto), con valores extremadamente altos que no representaban el comportamiento típico. Estos casos eran pocos (menos del 3%), pero influyen en promedios y decisiones de pricing.\n",
    "\n",
    "**Segmentos por Edad**\n",
    "Clientes jóvenes (18-30 años): muestran alto consumo de datos y menor uso de llamadas tradicionales. Tienden a preferir planes flexibles o prepago.\n",
    "\n",
    "Adultos (31-50 años): presentan consumo balanceado entre datos, llamadas y servicios adicionales. Son el segmento más estable y con mayor permanencia.\n",
    "\n",
    "Mayores de 50 años: menor uso de datos, mayor uso de llamadas y sensibilidad al precio.\n",
    "\n",
    "**Segmentos por Nivel de Uso**\n",
    "\n",
    "Uso bajo: clientes que consumen por debajo del promedio. Suelen contratar planes básicos y tienen mayor riesgo de churn si perciben poco valor.\n",
    "\n",
    "Uso medio: representan la mayoría. Mantienen consumo constante y responden bien a bundles o beneficios adicionales.\n",
    "\n",
    "Uso alto: clientes intensivos en datos o minutos. Generan mayor ingreso, pero también mayor costo operativo.\n",
    "\n",
    "\n",
    "Esto sugiere que ...\n",
    "\n",
    "\n",
    "**Recomendaciones**\n",
    "\n",
    "Crear planes segmentados:\n",
    "• Plan joven centrado en datos y apps ilimitadas.\n",
    "• Plan familiar o profesional para uso medio-alto.\n",
    "• Plan económico enfocado en llamadas para clientes mayores.\n",
    "\n",
    "Implementar alertas de consumo para sugerir upgrades o downgrades automáticos.\n",
    "\n",
    "Diseñar estrategias de retención para clientes con subutilización (beneficios, cambios de plan simples).\n",
    "\n",
    "Ofrecer add-ons modulares (datos extra, roaming, streaming) para monetizar a los usuarios intensivos sin aumentar el churn.\n",
    "\n",
    "Utilizar los outliers como base para crear planes premium o corporativos."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "538df90e",
   "metadata": {},
   "source": [
    "---\n",
    "\n",
    "## 🧩Paso 8 Cargar tu notebook y README a GitHub\n",
    "\n",
    "🎯 **Objetivo:**  \n",
    "Entregar tu análisis de forma **profesional**, **documentada** y **versionada**, asegurando que cualquier persona pueda revisar, ejecutar y entender tu trabajo.\n",
    "\n",
    "\n",
    "\n",
    "### Opción A : Subir archivos desde la interfaz de GitHub (UI)\n",
    "\n",
    "1. Descarga este notebook (`File → Download .ipynb`).  \n",
    "2. Entra a tu repositorio en GitHub (por ejemplo `telecom-analysis` o `sprint7-final-project`).  \n",
    "3. Sube tu notebook **Add file → Upload files**.  \n",
    "\n",
    "---\n",
    "\n",
    "### Opción B : Guardar directo desde Google Colab\n",
    "\n",
    "1. Abre tu notebook en Colab.  \n",
    "2. Ve a **File → Save a copy in GitHub**.  \n",
    "3. Selecciona el repositorio y la carpeta correcta (ej: `notebooks/`).  \n",
    "4. Escribe un mensaje de commit claro, por ejemplo:  \n",
    "    - `feat: add final ConnectaTel analysis`\n",
    "    - `agregar version final: Análisis ConnectaTel`\n",
    "5. Verifica en GitHub que el archivo quedó en el lugar correcto y que el historial de commits se mantenga limpio.\n",
    "\n",
    "---\n",
    "\n",
    "Agrega un archivo `README.md` que describa de forma clara:\n",
    "- el objetivo del proyecto,  \n",
    "- los datasets utilizados,  \n",
    "- las etapas del análisis realizadas,  \n",
    "- cómo ejecutar el notebook (por ejemplo, abrirlo en Google Colab),  \n",
    "- una breve guía de reproducción.\n",
    "---"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "05e15812",
   "metadata": {},
   "source": [
    "Link a repositorio público del proyecto: `LINK a tu repo aquí`"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "4ff759e8",
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "ExecuteTimeLog": [
   {
    "duration": 4,
    "start_time": "2025-12-10T20:08:09.491Z"
   },
   {
    "duration": 2,
    "start_time": "2025-12-10T20:11:23.785Z"
   },
   {
    "duration": 9,
    "start_time": "2025-12-10T20:19:38.615Z"
   },
   {
    "duration": 159,
    "start_time": "2025-12-10T20:19:41.959Z"
   },
   {
    "duration": 5,
    "start_time": "2025-12-10T20:40:54.225Z"
   },
   {
    "duration": 3,
    "start_time": "2025-12-10T21:06:23.637Z"
   },
   {
    "duration": 3,
    "start_time": "2025-12-10T21:08:00.035Z"
   },
   {
    "duration": 2,
    "start_time": "2025-12-10T21:17:13.669Z"
   },
   {
    "duration": 11,
    "start_time": "2025-12-10T22:17:40.508Z"
   },
   {
    "duration": 12,
    "start_time": "2025-12-10T22:17:48.415Z"
   },
   {
    "duration": 196,
    "start_time": "2025-12-11T16:03:49.156Z"
   },
   {
    "duration": 2,
    "start_time": "2025-12-11T21:07:28.347Z"
   },
   {
    "duration": 7,
    "start_time": "2025-12-24T06:22:58.771Z"
   },
   {
    "duration": 8,
    "start_time": "2025-12-26T17:57:00.444Z"
   }
  ],
  "kernelspec": {
   "display_name": "Python 3 (ipykernel)",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.9.23"
  },
  "toc": {
   "base_numbering": 1,
   "nav_menu": {},
   "number_sections": true,
   "sideBar": true,
   "skip_h1_title": true,
   "title_cell": "Table of Contents",
   "title_sidebar": "Contents",
   "toc_cell": false,
   "toc_position": {
    "height": "calc(100% - 180px)",
    "left": "10px",
    "top": "150px",
    "width": "165px"
   },
   "toc_section_display": true,
   "toc_window_display": true
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}
