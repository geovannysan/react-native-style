# 02. Alineación y Distribución

Esta sección cubre las propiedades encargadas de alinear y distribuir los elementos a lo largo de los ejes principal y secundario.

---

## 1. Alineación Horizontal (justifyContent)

Alinea a los hijos dentro del eje principal del contenedor. Si `flexDirection` es `row`, maneja el eje horizontal; si es `column`, maneja el eje vertical.

### Valores y Diagramas (Eje principal horizontal, flexDirection: 'row')

#### `flex-start`
```
┌─────────────────────────────────────────┐
│ [ A ][ B ][ C ]                         │
└─────────────────────────────────────────┘
```
#### `center`
```
┌─────────────────────────────────────────┐
│             [ A ][ B ][ C ]             │
└─────────────────────────────────────────┘
```
#### `flex-end`
```
┌─────────────────────────────────────────┐
│                         [ A ][ B ][ C ] │
└─────────────────────────────────────────┘
```
#### `space-between`
```
┌─────────────────────────────────────────┐
│ [ A ]                 [ B ]       [ C ] │
└─────────────────────────────────────────┘
```
#### `space-around`
```
┌─────────────────────────────────────────┐
│   [ A ]         [ B ]         [ C ]   │
└─────────────────────────────────────────┘
```
#### `space-evenly`
```
┌─────────────────────────────────────────┐
│   [ A ]     [ B ]     [ C ]   │
└─────────────────────────────────────────┘
```

* **Ejemplo Sencillo:**
  ```typescript
  import { StyleSheet } from 'react-native';

  const styles = StyleSheet.create({
    centradoHorizontal: {
      flexDirection: 'row',
      justifyContent: 'center',
    },
  });
  ```
* **Ejemplo Real:**
  ```typescript
  import React from 'react';
  import { View, Text, StyleSheet, TouchableOpacity } from 'react-native';

  export const HeaderNavegacion = () => {
    return (
      <View style={styles.header}>
        <TouchableOpacity style={styles.boton}><Text>Atrás</Text></TouchableOpacity>
        <Text style={styles.titulo}>Perfil</Text>
        <TouchableOpacity style={styles.boton}><Text>Ajustes</Text></TouchableOpacity>
      </View>
    );
  };

  const styles = StyleSheet.create({
    header: {
      flexDirection: 'row',
      height: 56,
      alignItems: 'center',
      justifyContent: 'space-between', // Separa botones a los extremos y título al centro
      backgroundColor: '#f8fafc',
      paddingHorizontal: 16,
      borderBottomWidth: 1,
      borderBottomColor: '#e2e8f0',
    },
    boton: {
      padding: 8,
    },
    titulo: {
      fontSize: 18,
      fontWeight: 'bold',
    },
  });
  ```
* **Buenas Prácticas:** Utilizar `space-between` en componentes header y barras de control para mantener botones a los lados de forma limpia.
* **Errores Comunes:** Utilizar `space-around` cuando se desea un alineado al ras con los bordes de la pantalla.

---

## 2. Alineación Vertical (alignItems)

Alinea a los elementos dentro del eje secundario. Si `flexDirection` es `row`, controla la alineación vertical.

### Valores y Diagramas (Eje secundario vertical, flexDirection: 'row')

#### `flex-start` (Alinea al inicio del eje secundario)
```
┌─────────────────────────────────────────┐
│ [ A ]   [ B ]                           │
│         [ B ]                           │
└─────────────────────────────────────────┘
```
#### `center` (Alinea al centro)
```
┌─────────────────────────────────────────┐
│         [ B ]                           │
│ [ A ]   [ B ]                           │
│         [ B ]                           │
└─────────────────────────────────────────┘
```
#### `flex-end` (Alinea al final)
```
┌─────────────────────────────────────────┐
│         [ B ]                           │
│ [ A ]   [ B ]                           │
└─────────────────────────────────────────┘
```
#### `stretch` (Estira a los hijos que no tienen una dimensión fija definida)
```
┌─────────────────────────────────────────┐
│ ┌───┐   ┌───┐                           │
│ │ A │   │ B │                           │
│ └───┘   └───┘                           │
└─────────────────────────────────────────┘
```
#### `baseline` (Alinea los textos según su base tipográfica inferior)
```
┌─────────────────────────────────────────┐
│                                         │
│ (Texto Grande)  (Texto Chico)           │
│ ──────────────  ─────────────           │
└─────────────────────────────────────────┘
```

* **Ejemplo Sencillo:**
  ```typescript
  const styles = StyleSheet.create({
    centradoEjeSecundario: {
      alignItems: 'center',
    },
  });
  ```
* **Ejemplo Real:**
  ```typescript
  import React from 'react';
  import { View, Text, StyleSheet } from 'react-native';

  export const FilaDetalleItem = () => {
    return (
      <View style={styles.fila}>
        <View style={styles.circuloIcono} />
        <Text style={styles.texto}>Ajustes de Cuenta</Text>
      </View>
    );
  };

  const styles = StyleSheet.create({
    fila: {
      flexDirection: 'row',
      alignItems: 'center', // Centra verticalmente el icono redondo y el texto
      padding: 12,
      backgroundColor: '#fff',
    },
    circuloIcono: {
      width: 32,
      height: 32,
      borderRadius: 16,
      backgroundColor: '#3b82f6',
    },
    texto: {
      marginLeft: 12,
      fontSize: 16,
    },
  });
  ```
* **Buenas Prácticas:** Combinar `alignItems: 'center'` con `flexDirection: 'row'` para conseguir filas perfectamente alineadas en listas y formularios.
* **Errores Comunes:** Usar `alignItems: 'stretch'` en un contenedor con hijos que tienen anchos explícitos, impidiendo que el motor Yoga los expanda.

---

## 3. Alineación Individual (alignSelf)

Permite anular la propiedad `alignItems` del contenedor padre únicamente para un componente específico.

* **Qué hace:** Configura de forma aislada cómo se alinea una vista hija dentro del eje secundario de su padre.
* **Cuándo usarla:** Para destacar un elemento de una lista o mover un botón/badge específico al extremo opuesto.
* **Cuándo NO usarla:** Si todos los elementos del contenedor deben tener la misma alineación; para eso usa `alignItems` en el padre.
* **Ejemplo Sencillo:**
  ```typescript
  const styles = StyleSheet.create({
    alineadoAlFinal: {
      alignSelf: 'flex-end',
    },
  });
  ```
* **Ejemplo Real:**
  ```typescript
  import React from 'react';
  import { View, Text, StyleSheet } from 'react-native';

  export const FilaMensaje = () => {
    return (
      <View style={styles.contenedor}>
        <View style={styles.globoMensaje}>
          <Text style={styles.texto}>Hola, ¿necesitas ayuda?</Text>
        </View>
        <Text style={styles.marcaTiempo}>14:32</Text>
      </View>
    );
  };

  const styles = StyleSheet.create({
    contenedor: {
      flexDirection: 'column',
      padding: 16,
    },
    globoMensaje: {
      backgroundColor: '#3b82f6',
      padding: 12,
      borderRadius: 8,
      maxWidth: '85%',
    },
    texto: {
      color: '#fff',
    },
    marcaTiempo: {
      fontSize: 11,
      color: '#94a3b8',
      marginTop: 4,
      alignSelf: 'flex-end', // Desplaza solo la hora al borde derecho
    },
  });
  ```
* **Buenas Prácticas:** Utilizar `alignSelf: 'stretch'` en botones interiores de formularios para que cubran todo el ancho del formulario.
* **Errores Comunes:** Aplicar `alignSelf` al contenedor padre en lugar del componente hijo.

---

## 4. Distribución (gap, rowGap, columnGap)

* **Qué hace:** Crea un espaciado equidistante y automático entre componentes hijos sin alterar los márgenes exteriores.
* **Cuándo usarla:** En listas de botones, tarjetas de cuadrícula y filas para evitar lógica de condicionales en márgenes de elementos (`isLastItem`).
* **Cuándo NO usarla:** En versiones de React Native inferiores a la `0.71.0`, ya que causará errores fatales o se ignorará.
* **Ejemplo Sencillo:**
  ```typescript
  const styles = StyleSheet.create({
    espaciadoHijos: {
      gap: 16,
    },
  });
  ```
* **Ejemplo Real:**
  ```typescript
  import React from 'react';
  import { View, StyleSheet, Pressable, Text } from 'react-native';

  export const GrupoBotonesAccion = () => {
    return (
      <View style={styles.contenedorBotones}>
        <Pressable style={styles.botonSecundario}><Text style={styles.txtSec}>Cancelar</Text></Pressable>
        <Pressable style={styles.botonPrincipal}><Text style={styles.txtPri}>Guardar</Text></Pressable>
      </View>
    );
  };

  const styles = StyleSheet.create({
    contenedorBotones: {
      flexDirection: 'row',
      width: '100%',
      gap: 12, // Separa los dos botones exactamente 12dp entre sí
    },
    botonPrincipal: {
      flex: 1,
      height: 48,
      backgroundColor: '#3b82f6',
      borderRadius: 8,
      justifyContent: 'center',
      alignItems: 'center',
    },
    botonSecundario: {
      flex: 1,
      height: 48,
      backgroundColor: '#e2e8f0',
      borderRadius: 8,
      justifyContent: 'center',
      alignItems: 'center',
    },
    txtPri: { color: '#fff', fontWeight: 'bold' },
    txtSec: { color: '#334155', fontWeight: 'bold' },
  });
  ```
* **Buenas Prácticas:** Utilizar `gap` en cuadrículas (`flexWrap: 'wrap'`) para obtener espaciados bi-dimensionales limpios y uniformes.
* **Errores Comunes:** Utilizar `gap` pensando que añade margen exterior a la caja contenedora padre.
