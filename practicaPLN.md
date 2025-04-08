**Instrucciones para el Proyecto de Aplicación NLP con Intel® AI PC Development Kit**  

**Objetivo**: Desarrollar un sistema de Procesamiento de Lenguaje Natural (NLP) que resuelva un problema o situación de la vida diaria (ej. asistente virtual, clasificador de reclamos, analizador de reseñas) utilizando técnicas avanzadas y hardware especializado.  

---

### **1. Configuración Inicial**  
a) **Instalación del DevKit**:  
```bash  
git clone https://github.com/intel/aipc-devkit-install  
cd aipc-devkit-install && ./setup install  # Requiere Ubuntu 22.04+ o Windows 11  
```

b) **Solicitud de NUC Intel**:  
- El laboratorio cuenta con 3 equipos Intel NUC 12 Extreme (especificaciones: 32GB RAM, Intel Core i9-12900K).  
- **Proceso para uso del NUC**:  
  1. El alumno debe contactar al laboratorista para reservar franjas horarias (máximo 4 horas por sesión).  
  2. Los equipos no pueden ser retirados del laboratorio, pero se pueden utilizar en el lugar asignado o acceder remotamente vía SSH para entrenar modelos pesados.  

---

### **2. Fases del Proyecto**  

**Fase 1: Selección del Problema o Situación de la Vida Diaria**  
- El alumno debe identificar una problemática real que pueda ser resuelta mediante NLP. Ejemplos:  
  - Chatbot para seguimiento de salud mental (análisis de emociones).  
  - Clasificador de correos electrónicos urgentes.  
  - Traductor de jerga local a lenguaje formal.  

**Fase 2: Preprocesamiento de Datos**  
- Técnicas obligatorias:  
  - Tokenización, lematización y eliminación de stopwords.  
  - Limpieza avanzada para manejar caracteres especiales, emojis y errores ortográficos.  

```python  
# Ejemplo básico de limpieza textual con expresiones regulares  
import re  
texto = re.sub(r'[^\w\sáéíóúñ]', '', input_text)  # Eliminar caracteres especiales  
```

- **Recomendación**: Usar el NUC para procesar datasets grandes (>1GB).  

**Fase 3: Entrenamiento de Modelos NLP**  
- **Embeddings**: Representación vectorial del texto usando Word2Vec o SBERT preentrenado.  
- **Clasificación o Análisis**: Implementar modelos como BERT o XLNet optimizados para clasificación de texto o análisis de sentimientos.  

```python  
from intel_extension_for_transformers import optimize_model  
modelo = optimize_model(BertForSequenceClassification.from_pretrained('dccuchile/bert-base-spanish'))  # Optimización para CPU Intel  
```

**Fase 4: Implementación del Sistema Final (Ejemplo Chatbot)**  
- Usar el esqueleto del proyecto proporcionado por Intel:  
```bash  
git clone https://github.com/intel/conversational-ai-chatbot  
```

- Aceleración con OpenVINO para inferencias más rápidas en CPU:  
```python  
from openvino.runtime import Core  
core = Core()  
modelo_ov = core.compile_model('modelo.xml', 'CPU')  # Conversión desde ONNX a OpenVINO  
```

---

### **3. Requisitos de Entrega**  

1. **Código fuente completo**: Incluye scripts para preprocesamiento, entrenamiento y ejecución del modelo final (git).  

2. **Documentación técnica**: Debe incluir:  
   - Descripción del problema seleccionado por el alumno.  
   - Diagrama de flujo del sistema desarrollado.  
   - Métricas obtenidas durante las pruebas (precisión, F1-score, tiempos de inferencia).  

3. **Demostración funcional**: Presentar un video corto (máximo 10 minutos) mostrando cómo el sistema interactúa con datos reales y resuelve el problema seleccionado.  
