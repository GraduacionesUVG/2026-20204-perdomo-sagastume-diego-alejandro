# Reproducibilidad

La carpeta `src/` contiene una exportación de `thesis-v1.0` más los dos checkpoints del seguimiento híbrido, verificados contra el plan original. Los datos de sesiones humanas y los registros históricos de ejecución no se redistribuyen. El conjunto principal de entrenamiento tampoco se incluye en esta entrega.

Para comprobar la integridad de los archivos exportados, ejecutar `cd src` y `sha256sum -c SOURCE_SHA256SUMS`. El manifiesto cubre los archivos exportados y los checkpoints, no los archivos de configuración de entrega añadidos después.

El repositorio original conserva el historial Git necesario para `python freeze_experiment.py verify`. Clonar https://github.com/DiegoPerdomo0805/AdaptiveDifficultyRogue con `--branch thesis-v1.0`, instalar las dependencias y ejecutar la verificación allí. No volver a registrar el congelamiento en el repositorio de entrega.

El seguimiento híbrido se ejecutó en el commit indicado en `experiments/20261022_hybrid_v1/plan.json`; el script original exige esa procedencia exacta y se niega a sobrescribir resultados existentes. Los resultados y la selección sellada se conservan; no se requiere repetir la evaluación final. El juego interactivo ofrece los controladores fijo, heurístico y cGAN, no el híbrido experimental. El entorno original está documentado en `entorno_original.txt` (Python 3.12.3, PyTorch 2.14.1+cu130).
