# Ajuste dinámico de dificultad mediante generación condicional de configuraciones de combate en un videojuego roguelike

Trabajo de graduación — Universidad del Valle de Guatemala, 2026.

## Descripción

Este proyecto desarrolla un entorno experimental para investigar el ajuste dinámico de dificultad (Dynamic Difficulty Adjustment, DDA) en videojuegos de generación procedural mediante técnicas de inteligencia artificial.

Se implementó un videojuego roguelike en Python y Pygame que registra telemetría de combate, representa el desempeño reciente del jugador mediante un vector continuo y permite modificar los parámetros de los enemigos entre habitaciones.

La investigación evalúa una red generativa antagónica condicional (cGAN), orientada por resultados de combate, frente a configuraciones fijas y un controlador heurístico. Asimismo, estudia un controlador híbrido que combina las propuestas del modelo generativo con las del sistema heurístico.

El proyecto constituye una plataforma experimental de investigación y no un videojuego comercial.

## Tecnologías Utilizadas

- Python 3.12.3: lenguaje de programación.
- Pygame 2.6.1: implementación del videojuego.
- PyTorch 2.14.1: entrenamiento e inferencia de modelos de inteligencia artificial.
- NumPy 2.5.3: procesamiento numérico.
- Matplotlib 3.11.2: visualización de resultados experimentales.
- Pytest 9.1.1: pruebas automatizadas.
- Git y GitHub: control de versiones y distribución.
- Bash: automatización de los procedimientos experimentales.

## Requisitos Previos

- Python 3.12; versión verificada: 3.12.3.
- Git.
- Acceso a Internet para instalar las dependencias.
- Entorno gráfico compatible con Pygame.
- Linux o Windows con WSL2 y soporte gráfico.

No se requiere una GPU NVIDIA para ejecutar el videojuego, cargar los modelos o realizar las pruebas automatizadas. La instalación con PyTorch para CPU fue validada en un entorno independiente.

El entrenamiento original utilizó una GPU NVIDIA GeForce RTX 5070 Laptop y PyTorch con CUDA 13.0.

## Instalación

### 1. Clonar el repositorio

```bash
git clone https://github.com/DiegoPerdomo0805/2026-20204-perdomo-sagastume-diego-alejandro.git
cd 2026-20204-perdomo-sagastume-diego-alejandro
cd src
```

### 2. Crear y activar un entorno virtual

```bash
python3.12 -m venv .venv
source .venv/bin/activate
```

### 3. Instalar las dependencias

```bash
python -m pip install --upgrade pip

python -m pip install torch==2.14.1 \
  --index-url https://download.pytorch.org/whl/cpu

python -m pip install -r requirements.txt
python -m pip check
```

La instalación anterior utiliza PyTorch para CPU y evita la descarga innecesaria de bibliotecas CUDA.

### 4. Configurar las variables de entorno

El proyecto no requiere bases de datos, credenciales, servicios externos ni claves API.

El archivo `src/.env.example` documenta las variables disponibles para configurar los controladores.

Las variables se proporcionan directamente al ejecutar el programa. No es necesario crear un archivo `.env`.

## Ejecución del Proyecto

Ejecutar los siguientes comandos desde la carpeta `src`, con el entorno virtual activado.

### Modo heurístico

```bash
ROGUE_CONTROLLER=heuristic \
ROGUE_REWARD_POLICY=heuristic \
python rogue.py
```

El controlador heurístico modifica los parámetros de los encuentros según el desempeño reciente del jugador.

### Modo de dificultad fija

```bash
ROGUE_CONTROLLER=fixed \
ROGUE_REWARD_POLICY=heuristic \
python rogue.py
```

Este modo utiliza una configuración constante para los encuentros ordinarios de combate.

### Funcionalidades principales

- Generación procedural de mapas y habitaciones.
- Combate en tiempo real mediante ataques cuerpo a cuerpo y magia.
- Movimiento defensivo y desplazamiento rápido.
- Selección y equipamiento de recompensas.
- Ajuste de vida, daño, velocidad y cantidad de enemigos entre habitaciones.
- Registro de telemetría de combate y resultados.
- Evaluación automatizada de controladores de dificultad.

El controlador híbrido forma parte del procedimiento experimental automatizado. No corresponde a un modo seleccionable directamente desde el cliente gráfico.

## Capturas

Las capturas del videojuego en funcionamiento se encuentran en la carpeta [`capturas/`](capturas/).

Se incluyen 25 imágenes que documentan el funcionamiento del entorno experimental.

## Demo

El video demostrativo del proyecto está disponible en:

[`demo/demo.mp4`](demo/demo.mp4)

El video presenta el funcionamiento del videojuego, sus mecánicas principales, el sistema de ajuste dinámico de dificultad y los resultados de la investigación.

## Resultados Experimentales

El conjunto principal de datos se obtuvo mediante 9,000 partidas automatizadas, con 36,075 habitaciones ordinarias de combate.

La investigación determinó que el predictor condicionado por el estado y la acción, Q(x,a), aportó información predictiva respecto al modelo condicionado únicamente por el estado, Q(x).

Sin embargo, ninguno de los 36 puntos de control de la cGAN evaluados satisfizo todos los criterios establecidos para su selección. Este resultado se conserva como evidencia experimental negativa.

En un seguimiento exploratorio independiente se seleccionó un controlador híbrido con un factor de mezcla de 0.50 entre las configuraciones generadas por el modelo y las propuestas heurísticas.

La evaluación final utilizó 400 semillas reservadas, con 393 semillas comparables para el análisis pareado.

| Controlador | Distancia media de balance por partida (B_r) |
|---|---:|
| Dificultad fija | 0.26001 |
| Heurístico | 0.16887 |
| Híbrido | 0.11123 |

Los valores menores representan una mayor proximidad al intervalo de balance definido.

| Comparación pareada | Diferencia media | IC bootstrap del 95 % |
|---|---:|---|
| Híbrido - fijo | -0.148779 | [-0.162513, -0.135445] |
| Híbrido - heurístico | -0.057633 | [-0.072029, -0.043545] |

Los resultados muestran que el controlador híbrido presentó una distancia media de balance inferior a ambas referencias en el entorno automatizado.

Estos hallazgos no demuestran una mejora generalizada de la experiencia subjetiva de jugadores humanos.

## Validación y Reproducibilidad

El código fuente entregado corresponde a la versión `thesis-v1.0` del repositorio de investigación AdaptiveDifficultyRogue.

Commit de la versión entregada:

`f861e22d50c8a45fc5f4f133011da99b52d0a5df`

El contrato experimental original `exp_v3.2` permanece congelado, sin modificaciones a sus parámetros, métricas, umbrales ni particiones de semillas.

La entrega incluye los dos puntos de control utilizados durante el seguimiento híbrido, junto con sus registros de selección, validación y evaluación final.

### Verificar la integridad

Desde la carpeta `src`:

```bash
sha256sum -c SOURCE_SHA256SUMS
```

### Ejecutar las pruebas automatizadas

```bash
python -m pytest -q
```

Resultado obtenido durante la validación en un entorno independiente con PyTorch para CPU: **128 pruebas aprobadas**.

La comprobación del contrato experimental mediante `freeze_experiment.py verify` debe realizarse en el repositorio original, puesto que depende de su historial de Git.

### Evidencia adicional

- [Repositorio original de investigación](https://github.com/DiegoPerdomo0805/AdaptiveDifficultyRogue/tree/thesis-v1.0)
- [Documentación técnica](src/README.md)
- [Resultados experimentales finales](src/docs/RESULTADOS_FINALES.md)
- [Plan experimental híbrido](src/experiments/20261022_hybrid_v1/plan.json)
- [Registro de selección](src/experiments/20261022_hybrid_v1/selection.lock.json)
- [Reporte de evaluación final](src/experiments/20261022_hybrid_v1/final/final_report.json)
- [Reproducibilidad de la entrega](docs/reproducibilidad.md)
- [Entorno original](docs/entorno_original.txt)
- [Entorno de validación para CPU](docs/entorno_validacion_cpu.txt)

La entrega conserva el código, los modelos seleccionados y los resultados experimentales. No incluye el conjunto completo de datos de entrenamiento ni los registros individuales de participantes humanos.

## Documentación

- [Informe final del trabajo de graduación](docs/informe_final.pdf).
- [Consentimiento de uso de foto y video firmado](docs/consentimiento_firmado.pdf).

## Autor

Diego Alejandro Perdomo Sagastume — Carné 20204.

Universidad del Valle de Guatemala.

Ingeniería en Ciencias de la Computación y Tecnologías de la Información.

2026.

## Licencia

Código fuente entregado para fines académicos y de evaluación del trabajo de graduación. Los derechos de autor y las condiciones de reutilización se mantienen sujetos a las disposiciones aplicables. Esta entrega no establece una licencia de código abierto adicional.
