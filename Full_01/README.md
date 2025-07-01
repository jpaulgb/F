---
title: Diagrama de flujo - Simulación Geotécnica (OpenSees)
---

```mermaid
flowchart TD
    A[Inicio - wipe y configuración inicial]

    B[1. Geometría del suelo y malla]
    B1[Definir geometría del suelo]
    B2[Definir geometría de malla]
    B3[Calcular nodos totales]

    C[2. Crear nodos de presión de poros]
    C1[Crear nodos y coordenadas]
    C2[Definir fixities]
    C3[Establecer equalDOF]

    D[3. Crear nodos interiores]
    D1[Crear columna central]
    D2[Nodos interiores laterales]
    D3[Fixities y equalDOF]

    E[4. Propiedades de Materiales]
    E1[Propiedades elásticas]
    E2[Ángulos y presiones]
    E3[Coeficientes de contracción y dilatación]

    F[5. Crear elementos del suelo]
    F1[Asignar nodos a elementos]
    F2[Definir materiales por capa]

    G[6. Dashpot de Lysmer]
    G1[Crear nodos y fixities]
    G2[Material y elemento dashpot]

    H[7. Recorders de gravedad]
    H1[Lista de nodos y parámetros]
    H2[Recopilar desplazamiento, presión, aceleración]
    H3[Recopilar esfuerzos y deformaciones]

    I[8. Archivo .msh para GiD]
    I1[Escribir coordenadas y elementos]

    J[9. Parámetros de análisis]
    J1[Rango de frecuencias, damping]
    J2[Condición CFL y paso de tiempo]
    J3[Parámetros Newmark]

    K[10. Análisis gravitacional]
    K1[Etapa elástica]
    K2[Etapa plástica]

    L[11. Actualizar permeabilidades]
    L1[Parámetros por elemento]
    L2[Actualizar vPerm y hPerm]

    M[12. Recorders post-gravedad]
    M1[Reset de tiempo]
    M2[Recorders de desplazamiento y esfuerzo]

    N[13. Análisis dinámico]
    N1[Crear carga dinámica]
    N2[Configuración de análisis]
    N3[Bucle de reducción de paso de tiempo]

    O[Fin - wipe final y tiempo total]

    A --> B
    B --> B1 --> B2 --> B3
    B3 --> C --> C1 --> C2 --> C3
    C3 --> D --> D1 --> D2 --> D3
    D3 --> E --> E1 --> E2 --> E3
    E3 --> F --> F1 --> F2
    F2 --> G --> G1 --> G2
    G2 --> H --> H1 --> H2 --> H3
    H3 --> I --> I1
    I1 --> J --> J1 --> J2 --> J3
    J3 --> K --> K1 --> K2
    K2 --> L --> L1 --> L2
    L2 --> M --> M1 --> M2
    M2 --> N --> N1 --> N2 --> N3
    N3 --> O
