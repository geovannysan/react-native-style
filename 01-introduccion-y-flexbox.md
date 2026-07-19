# 01. Introducción y Flexbox

Esta sección introduce el motor de maquetación de React Native (`StyleSheet` + Yoga engine) y las bases fundamentales de Flexbox.

---

## 1. Introducción a StyleSheet

### ¿Qué es StyleSheet?
`StyleSheet` es una abstracción nativa que permite definir estilos utilizando sintaxis de JavaScript, la cual es enviada directamente a los motores de renderizado de iOS y Android.

### ¿Cómo funciona?
Al inicializar la aplicación, `StyleSheet.create` compila todas las declaraciones y las transfiere al hilo nativo en un solo paso, generando IDs numéricos internos. Esto evita re-evaluaciones en cada ciclo de renderizado de React, mejorando sustancialmente el rendimiento de la interfaz.

### Diferencias entre CSS y React Native
* **Sin cascada:** Los estilos aplicados en componentes padre no se propagan automáticamente hacia abajo.
* **Layout predeterminado:** Las vistas tienen por defecto `display: 'flex'` y `flexDirection: 'column'`.
* **Unidades:** Los tamaños son valores numéricos puros que representan píxeles lógicos independientes de la densidad (dp).

### Propiedades Existentes vs No Existentes
| Propiedad | React Native | Alternativa |
| :--- | :---: | :--- |
| `display: 'block' \| 'inline'` | ❌ | Solo soporta `flex` y `none`. |
| `float` / `clear` | ❌ | Usar Flexbox o posicionamiento absoluto. |
| `border` shorthand | ❌ | Declarar `borderWidth`, `borderColor`, `borderStyle` por separado. |

---

## 2. Flexbox

### flex
* **Qué hace:** Atajo que define la capacidad de un elemento para rellenar proporcionalmente el espacio libre de su contenedor padre.
* **Cuándo usarla:** Para maquetar la vista raíz de una pantalla (`flex: 1`) u organizar proporciones equitativas de tamaño.
* **Cuándo NO usarla:** En contenedores internos de componentes de scroll infinito donde el tamaño debe ser dictado exclusivamente por el contenido.
* **Ejemplo Sencillo:**
  ```typescript
  import { StyleSheet, View } from 'react-native';

  const styles = StyleSheet.create({
    pantallaRaiz: {
      flex: 1, // Llena el 100% de la pantalla
      backgroundColor: '#f8fafc',
    },
  });
  ```
* **Ejemplo Real:**
  ```typescript
  import React from 'react';
  import { View, StyleSheet } from 'react-native';

  export const LayoutDividido = () => {
    return (
      <View style={styles.contenedor}>
        <View style={styles.cajaSuperior} />
        <View style={styles.cajaInferior} />
      </View>
    );
  };

  const styles = StyleSheet.create({
    contenedor: {
      flex: 1, // Ocupa toda la pantalla
    },
    cajaSuperior: {
      flex: 2, // Toma 2/3 partes del espacio vertical
      backgroundColor: '#3b82f6',
    },
    cajaInferior: {
      flex: 1, // Toma 1/3 parte del espacio vertical
      backgroundColor: '#10b981',
    },
  });
  ```
* **Buenas Prácticas:** Asegurar que el contenedor padre tenga una dimensión definida o `flex: 1` para evitar colapsos.
* **Errores Comunes:** Definir `flex: 1` dentro de un `ScrollView` sin definir `contentContainerStyle`, provocando que el contenido interno tenga altura `0`.

---

### flexGrow
* **Qué hace:** Define la proporción con la que un elemento flexible crece para ocupar el espacio restante del contenedor padre.
* **Cuándo usarla:** Cuando se quiere que un elemento se expanda para rellenar espacios en blanco sin forzar un tamaño absoluto.
* **Cuándo NO usarla:** Si los elementos de la cuadrícula deben poseer dimensiones fijas y consistentes sin importar la resolución.
* **Ejemplo Sencillo:**
  ```typescript
  const styles = StyleSheet.create({
    cajaCreciente: {
      flexGrow: 1,
    },
  });
  ```
* **Ejemplo Real:**
  ```typescript
  import React from 'react';
  import { View, Text, StyleSheet } from 'react-native';

  export const FilaConTextoExpandido = () => {
    return (
      <View style={styles.fila}>
        <Text style={styles.textoCreciente}>Este texto crecerá tanto como sea posible</Text>
        <View style={styles.botonEstatico} />
      </View>
    );
  };

  const styles = StyleSheet.create({
    fila: {
      flexDirection: 'row',
      padding: 16,
      backgroundColor: '#fff',
    },
    textoCreciente: {
      flexGrow: 1, // Llena el espacio vacío horizontal restante
      fontSize: 16,
    },
    botonEstatico: {
      width: 50,
      height: 50,
      backgroundColor: '#ef4444',
    },
  });
  ```
* **Buenas Prácticas:** Utilizar `flexGrow: 1` en conjunto con `flexShrink: 1` para elementos que manejan textos dinámicos.
* **Errores Comunes:** Confundir `flexGrow` con `flex` pensando que operan de forma idéntica; `flexGrow` respeta el tamaño base determinado por `flexBasis`.

---

### flexShrink
* **Qué hace:** Determina cómo se encoge un elemento flexible cuando el tamaño de los elementos hijos supera el espacio físico de su contenedor.
* **Cuándo usarla:** Esencial en filas (`row`) que contienen etiquetas de texto extensas para evitar que sobresalgan del marco de la pantalla.
* **Cuándo NO usarla:** Si el contenido bajo ninguna circunstancia debe recortarse o perder su tamaño mínimo de legibilidad.
* **Ejemplo Sencillo:**
  ```typescript
  const styles = StyleSheet.create({
    textoEncogible: {
      flexShrink: 1,
    },
  });
  ```
* **Ejemplo Real:**
  ```typescript
  import React from 'react';
  import { View, Text, StyleSheet } from 'react-native';

  export const TarjetaUsuarioReducida = () => {
    return (
      <View style={styles.tarjeta}>
        <View style={styles.avatar} />
        <View style={styles.cajaTexto}>
          <Text style={styles.nombre} numberOfLines={1}>
            Juan Carlos de la Santísima Trinidad Rodríguez
          </Text>
        </View>
      </View>
    );
  };

  const styles = StyleSheet.create({
    tarjeta: {
      flexDirection: 'row',
      padding: 12,
      backgroundColor: '#fff',
      alignItems: 'center',
    },
    avatar: {
      width: 40,
      height: 40,
      backgroundColor: '#ccc',
      borderRadius: 20,
    },
    cajaTexto: {
      marginLeft: 12,
      flexShrink: 1, // Evita que el nombre empuje los bordes del dispositivo
    },
    nombre: {
      fontSize: 16,
      fontWeight: 'bold',
    },
  });
  ```
* **Buenas Prácticas:** Aplicar `flexShrink: 1` a los contenedores intermedios que envuelven textos.
* **Errores Comunes:** Omitir `flexShrink: 1` en layouts horizontales con avatares e iconos, lo que produce recortes y desbordamientos.

---

### flexBasis
* **Qué hace:** Define la dimensión base inicial del componente antes de que el motor de renderizado aplique reglas de distribución o compresión.
* **Cuándo usarla:** Para fijar tamaños iniciales fluidos y responsivos en maquetaciones multipantalla.
* **Cuándo NO usarla:** Cuando el elemento requiere un ancho o alto estrictamente rígido que no debe verse afectado por el flujo de Flexbox.
* **Ejemplo Sencillo:**
  ```typescript
  const styles = StyleSheet.create({
    baseMitad: {
      flexBasis: '50%',
    },
  });
  ```
* **Ejemplo Real:**
  ```typescript
  import React from 'react';
  import { View, StyleSheet } from 'react-native';

  export const DistribucionFlexible = () => {
    return (
      <View style={styles.contenedor}>
        <View style={styles.cajaA} />
        <View style={styles.cajaB} />
      </View>
    );
  };

  const styles = StyleSheet.create({
    contenedor: {
      flexDirection: 'row',
      width: '100%',
    },
    cajaA: {
      flexBasis: 120, // Tamaño de partida de 120dp
      flexGrow: 1,    // Crecerá si sobra espacio
      backgroundColor: '#f59e0b',
    },
    cajaB: {
      flexBasis: 200, // Tamaño de partida de 200dp
      flexGrow: 2,    // Crecerá el doble de rápido si sobra espacio
      backgroundColor: '#ef4444',
    },
  });
  ```
* **Buenas Prácticas:** Utilizar en lugar de `width`/`height` si el elemento requiere flexibilidad controlada en múltiples orientaciones.
* **Errores Comunes:** Confundir `flexBasis` con un tamaño rígido e inmutable.

---

## 3. Dirección (flexDirection)

Determina el eje sobre el cual se disponen secuencialmente los elementos hijos.

### Valores
* `column` (Predeterminado): Apila verticalmente de arriba hacia abajo.
* `row`: Alinea horizontalmente de izquierda a derecha.
* `column-reverse`: Apila verticalmente de abajo hacia arriba.
* `row-reverse`: Alinea horizontalmente de derecha a izquierda.

### Diagramas ASCII

#### column
```
┌───────────┐
│   [ A ]   │   Eje Principal (Y) ↓
│   [ B ]   │
│   [ C ]   │   Eje Secundario (X) ──→
└───────────┘
```

#### row
```
┌─────────────────────┐
│  [ A ] [ B ] [ C ]  │   Eje Principal (X) ──→
│                     │   Eje Secundario (Y) ↓
└─────────────────────┘
```

* **Ejemplo Sencillo:**
  ```typescript
  const styles = StyleSheet.create({
    cajaHorizontal: {
      flexDirection: 'row',
    },
  });
  ```
* **Ejemplo Real:**
  ```typescript
  import React from 'react';
  import { View, Text, StyleSheet } from 'react-native';

  export const TarjetaMiniatura = () => {
    return (
      <View style={styles.tarjeta}>
        <View style={styles.miniatura} />
        <View style={styles.textos}>
          <Text style={styles.titulo}>Título</Text>
          <Text style={styles.subtitulo}>Subtítulo informativo</Text>
        </View>
      </View>
    );
  };

  const styles = StyleSheet.create({
    tarjeta: {
      flexDirection: 'row', // Miniatura al lado de los textos
      padding: 12,
      backgroundColor: '#fff',
      borderRadius: 8,
    },
    miniatura: {
      width: 60,
      height: 60,
      backgroundColor: '#e2e8f0',
      borderRadius: 4,
    },
    textos: {
      flexDirection: 'column', // Los textos se apilan uno encima de otro
      marginLeft: 12,
      justifyContent: 'center',
    },
    titulo: {
      fontWeight: 'bold',
      fontSize: 16,
    },
    subtitulo: {
      color: '#64748b',
    },
  });
  ```
* **Buenas Prácticas:** Recordar siempre que en React Native la orientación base es `column`, a diferencia del estándar Web de CSS (`row`).
* **Errores Comunes:** Omitir `flexDirection: 'row'` al intentar maquetar botones o barras de navegación horizontales.
