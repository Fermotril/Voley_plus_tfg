# 🏐 Voley+ | Análisis Táctico de Voleibol con Inteligencia Artificial

![Universidad de Granada](https://img.shields.io/badge/Universidad_de_Granada-Grado_en_Ingeniería_Informática-red)
![Calificación](https://img.shields.io/badge/Calificación-10_%2F_10-brightgreen)
![Python](https://img.shields.io/badge/Python-3.9+-blue?logo=python&logoColor=white)
![YOLOv8](https://img.shields.io/badge/YOLOv8-Ultralytics-purple)
![FastAPI](https://img.shields.io/badge/FastAPI-Backend-009688?logo=fastapi&logoColor=white)
![Flutter](https://img.shields.io/badge/Flutter-Multiplataforma-02569B?logo=flutter&logoColor=white)

> **Trabajo de Fin de Grado** — Escuela Técnica Superior de Ingenierías Informática y de Telecomunicación (ETSIIT), Universidad de Granada (2026).  
> **Autor:** Fernando Rodríguez Sánchez
> **Directores:** Dr. José Manuel Benítez Sánchez y Dr. Aurelio Ureña Espa (Asesoramiento táctico deportivo).

---

## 📄 Documentación del Proyecto

* 📘 **[Leer / Descargar Memoria Completa (PDF)](./Memoria_TFG_Fernando_Rodriguez.pdf)** *(128 páginas — Arquitectura, entrenamiento, evaluación experimental y anexos).*
* 📊 **[Ver Presentación de Defensa (PDF/PPTX)](./Presentacion_Defensa_TFG.pdf)** *(Diapositivas presentadas ante el tribunal).*

---

## 🎯 Descripción General

El análisis estadístico en el voleibol se realiza mayoritariamente de forma manual (costoso en tiempo y propenso a errores) o mediante infraestructuras multicámara de élite económicamente inaccesibles para la mayoría de clubes.

**Voley+** es una plataforma *End-to-End* que democratiza la analítica deportiva automatizando la extracción de métricas físicas y tácticas a partir de **una única toma de vídeo frontal**, sin requerir sensores corporales (GPS/RFID) ni hardware especializado.

### ✨ Funcionalidades Principales
* **Detección y Seguimiento Multiobjeto:** Localización continua de jugadores y balón en alta resolución mediante una red **YOLOv8** entrenada a medida y **ByteTrack**.
* **Identificación de Dorsales:** Pipeline de preprocesamiento morfológico (recorte de torso, interpolación Lanczos4, ecualización CLAHE e inversión binaria) acoplado a **EasyOCR** y a una máquina de estados finita.
* **Rectificación Espacial (Homografía 2D):** Calibración interactiva de los 4 vértices de la pista para proyectar las coordenadas de cámara a un plano métrico real de **$9 \times 9\text{ m}$**.
* **Detección Autónoma de Eventos:** Identificación automática de saques, actualización del marcador, seguimiento del vector de rotación (P1–P6) y detección de sustituciones en pista.
* **Dashboard Táctico y Exportación:** Segmentación interactiva de jugadas (*rallies*) mediante atajos de teclado, generación de **mapas de calor topológicos**, cálculo de distancias recorridas y exportación a **CSV**.

---

## 📊 Resultados Experimentales Destacados

Evaluado sobre partidos de nivel olímpico (*Juegos Olímpicos de París 2024 y Tokio 2020*) y un conjunto de test independiente:

| Módulo / Métrica | Resultado | Detalles Técnicos |
| :--- | :---: | :--- |
| **Detector de Entidades (YOLOv8s)** | **95,6 %** `mAP@0.5` | Dataset propio de 1.910 fotogramas anotados en Roboflow (`player` y `ball`). |
| **Detección del Balón** | **96,3 %** `mAP@0.5` | Inferencia hipersensible (`imgsz=1280`, `conf=0.20`) para mitigar *motion blur*. |
| **Detección de Saques** | **98,2 %** Precisión / Recall | Validado sobre 55 saques reales mediante lógica espacial y *cooldown* anti-duplicados. |
| **Identificación del Sacador** | **96,4 %** Lecturas válidas | Votación temporal de 7 segundos con respaldo de memoria espacial (*tracking*). |
| **Latencia de Procesamiento** | **~5,81 FPS** | Ejecución en diferido acelerada por GPU (NVIDIA RTX 2080 Ti - Clúster UGR). |

---

## 🏗️ Arquitectura y Stack Tecnológico

El sistema sigue una arquitectura distribuida cliente-servidor sin estado persistente (*stateless*):

* **Motor de Inferencia (Backend):** `Python 3.9`, `FastAPI`, `Ultralytics YOLOv8`, `OpenCV`, `EasyOCR`, `SciPy` (Algoritmo Húngaro), `NumPy`, `Pandas` y `FFmpeg` (transcodificación H.264 `+faststart`).
* **Aplicación Cliente (Frontend):** `Flutter` / `Dart` (compilación nativa para Escritorio Windows y Web) con lienzo interactivo de calibración y renderizado de mapas de calor.
* **Control de Acceso:** `Firebase Authentication` (gestión de sesiones mediante tokens JWT).
* **Despliegue:** Contenedor `Podman` con aceleración CUDA desplegado en el clúster de alto rendimiento de la Universidad de Granada.

---
