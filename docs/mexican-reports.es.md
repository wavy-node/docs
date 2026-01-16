---
title: Reportes de Cumplimiento para México
description: Entiende los reportes de cumplimiento automatizados para las autoridades financieras de México.
---

# Reportes de Cumplimiento para México

WavyNode te ayuda a cumplir automáticamente con las regulaciones de prevención de lavado de dinero de México (LFPIORPI). Nuestra plataforma genera los reportes específicos requeridos por la Unidad de Inteligencia Financiera (UIF) para actividades que involucran activos virtuales.

## Qué Puedes Esperar

*   **Reportes Mensuales Automáticos**: Al inicio de cada mes, WavyNode genera automáticamente los reportes oficiales en formato XML que cubren la actividad del mes anterior. El proceso es completamente automatizado y no requiere ninguna acción manual de tu parte.

*   **Reportes Basados en Umbrales**: Los reportes se crean únicamente para tus usuarios finales cuyo volumen de transacciones durante el mes, iguala o supera el umbral oficial establecido por las autoridades (basado en el valor de la UMA).

*   **Notificaciones por Correo**: Tan pronto como los reportes de tu proyecto estén listos, tus administradores designados recibirán una notificación por correo electrónico.

*   **Descargas Seguras**: Puedes descargar de forma segura los reportes XML generados directamente desde tu panel de WavyNode. Estos archivos están listos para que los presentes ante las autoridades correspondientes.

## Tus Responsabilidades

Para que los reportes se generen correctamente, WavyNode depende de la información de usuario que proporcionas a través de tu integración.

Es crucial que tu endpoint `GET /users/:userId` esté siempre activo y proporcione información precisa y actualizada de tus usuarios. Estos datos se utilizan para completar los campos requeridos en los reportes oficiales, asegurando su validez y cumplimiento.
