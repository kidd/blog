---
title: Solución al error ERR_EMPTY_RESPONSE en Sedes Electrónicas (macOS + Chrome)
categories: [hacienda,taxes]
---

Si intentas entrar en la sede de **Hacienda**, **Tributos de Tenerife** o cualquier administración pública con tu certificado digital en Mac y te encuentras con el mensaje:
> **"This page isn’t working. sede.ejemplo.es didn’t send any data. ERR_EMPTY_RESPONSE"**

No entres en pánico. Tu certificado no está roto ni la web está caída. El problema suele ser un "bloqueo silencioso" en los ajustes de red de tu Mac. Aquí te explico cómo solucionarlo en 2 minutos.

---

## 🔍 El Diagnóstico Rápido
Antes de tocar nada, verifica si es un problema general o específico:
1. Intenta entrar en la [Sede de la AEAT (Hacienda Estatal)](https://www.agenciatributaria.gob.es).
2. **Si la AEAT funciona pero otra sede local no**, tu certificado está bien. El problema es un filtro de red que interrumpe la conexión con servidores específicos.

---

## 🛠 La Solución: Desactivar Filtros de Red y Antivirus

En las versiones modernas de macOS (Monterey, Ventura, Sonoma, Sequoia), los antivirus como **Avast, AVG o Kaspersky** instalan "filtros de contenido" que analizan tu tráfico. Las sedes electrónicas detectan esto como una intrusión y cortan la conexión.

### Pasos para arreglarlo:

1. Ve al menú de la manzana  > **Ajustes del Sistema** (o Preferencias del Sistema).
2. En la barra lateral, selecciona **Red** (Network).
3. Haz clic en el apartado **Filtros y Proxies** (Filters & Proxies).
4. Aquí verás una lista de aplicaciones. Si ves **Avast**, **AVG** o cualquier otro antivirus con un interruptor activado:
   * **Desactívalo** (ponlo en OFF).
   * En algunos casos, también deberás desactivar el **Relé Privado de iCloud** (iCloud Private Relay) si aparece en esta sección.
5. Haz clic en **Aceptar**.

---

## 🚀 Pasos adicionales si el error persiste

Si después de desactivar los filtros sigues sin poder entrar, realiza esta limpieza rápida en Chrome:

### 1. Limpiar Sockets en Chrome
Copia y pega esto en la barra de direcciones de Chrome:
`chrome://net-internals/#sockets`
Luego haz clic en el botón **"Flush socket pools"**.

### 2. Dar permisos en el Acceso a Llaveros
A veces macOS bloquea el acceso de Chrome a la "llave" del certificado:
1. Abre la app **Acceso a Llaveros**.
2. Busca tu certificado en **Mis certificados**.
3. Despliega la flecha, haz doble clic en la **Clave privada** (icono de llave).
4. En la pestaña **Control de acceso**, marca: *"Permitir que todas las aplicaciones accedan a este ítem"*.

---

## ✅ Resumen
La mayoría de las veces, el culpable es el **Escudo Web de tu antivirus** (como Avast) ubicado en los ajustes de Red de tu Mac. Al desactivarlo, permites que la conexión SSL entre tu certificado y la Administración sea directa y segura.

¡Espero que te sirva de ayuda! Si te ha funcionado, ¡comparte este post con otros usuarios de Mac sufridores! 🍎💻
