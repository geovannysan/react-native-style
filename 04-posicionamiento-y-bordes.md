# 04. Posicionamiento, Bordes y Fondos

Esta sección aborda cómo estructurar elementos fuera del flujo estándar de Flexbox, cómo manejar bordes y cómo estilizar fondos.

---

## 1. Posicionamiento

En React Native todos los componentes se posicionan de manera `relative` por defecto.

### position: 'relative'
Los elementos se disponen de forma secuencial según el motor Flexbox. Las propiedades `top`, `bottom`, `left`, `right` desplazan visualmente al elemento sin alterar el espacio que ocupaba originalmente en el layout.

### position: 'absolute'
El elemento se retira del flujo natural de maquetación y se posiciona de forma precisa en relación con su ancestro contenedor más cercano que tenga estilos activos.

### zIndex
Determina el orden tridimensional en el que se superponen y renderizan los componentes.

* **Ejemplo Sencillo:**
  ```typescript
  import { StyleSheet } from 'react-native';

  const styles = StyleSheet.create({
    cajaAbsoluta: {
      position: 'absolute',
      top: 10,
      right: 10,
      zIndex: 5,
    },
  });
  ```
* **Ejemplo Real:**
  ```typescript
  import React from 'react';
  import { View, Text, StyleSheet } from 'react-native';

  export const TarjetaConBadge = () => {
    return (
      <View style={styles.tarjeta}>
        <Text style={styles.titulo}>Novedades</Text>
        <View style={styles.badgeNuevo}>
          <Text style={styles.textoBadge}>Nuevo</Text>
        </View>
      </View>
    );
  };

  const styles = StyleSheet.create({
    tarjeta: {
      position: 'relative', // Contenedor de referencia indispensable
      padding: 16,
      backgroundColor: '#f8fafc',
      borderRadius: 12,
      borderWidth: 1,
      borderColor: '#e2e8f0',
      minHeight: 100,
    },
    titulo: {
      fontSize: 16,
      fontWeight: '600',
    },
    badgeNuevo: {
      position: 'absolute',
      top: -8, // Sobresale por la parte superior
      right: 12,
      backgroundColor: '#ef4444',
      paddingHorizontal: 8,
      paddingVertical: 4,
      borderRadius: 99,
      zIndex: 10, // Se renderiza encima de cualquier elemento vecino
    },
    textoBadge: {
      color: '#fff',
      fontSize: 11,
      fontWeight: 'bold',
    },
  });
  ```
* **Buenas Prácticas:** Asegurar que el contenedor padre de un elemento absoluto declare explícitamente `position: 'relative'` para evitar que el hijo se posicione respecto a la pantalla del dispositivo.
* **Errores Comunes:** En Android, la propiedad `zIndex` a veces se ignora si los componentes no son hermanos directos o si no se acompaña con la propiedad `elevation`.

---

## 2. Bordes

Los bordes en React Native no admiten una declaración simplificada de una sola línea (shorthand).

### Propiedades
* `borderWidth`, `borderLeftWidth`, `borderTopWidth`, etc.
* `borderColor`, `borderLeftColor`, `borderTopColor`, etc.
* `borderRadius`, `borderTopLeftRadius`, `borderTopRightRadius`, etc.
* `borderStyle` (`'solid'`, `'dotted'`, `'dashed'`).

* **Ejemplo Sencillo:**
  ```typescript
  const styles = StyleSheet.create({
    bordeCurvo: {
      borderWidth: 2,
      borderColor: '#3b82f6',
      borderRadius: 8,
    },
  });
  ```
* **Ejemplo Real:**
  ```typescript
  import React from 'react';
  import { View, StyleSheet, Image } from 'react-native';

  export const AvatarRedondo = () => {
    return (
      <View style={styles.contenedor}>
        <Image 
          source={{ uri: 'https://images.unsplash.com/photo-1535713875002-d1d0cf377fde' }} 
          style={styles.avatar}
        />
      </View>
    );
  };

  const styles = StyleSheet.create({
    contenedor: {
      width: 80,
      height: 80,
      borderRadius: 40, // Radio exacto de la mitad del tamaño para círculo perfecto
      borderWidth: 3,
      borderColor: '#3b82f6',
      padding: 2, // Espacio entre el borde azul y la foto
      backgroundColor: '#fff',
    },
    avatar: {
      width: '100%',
      height: '100%',
      borderRadius: 40,
    },
  });
  ```
* **Buenas Prácticas:** Si se aplica `borderRadius` a un contenedor que envuelve una imagen u otros elementos hijos dinámicos, es crucial activar `overflow: 'hidden'` para que el contenido sea recortado respetando las esquinas curvas.
* **Errores Comunes:** No declarar `borderWidth` al especificar un `borderColor`, lo que impide que el borde sea visible en pantalla.

---

## 3. Fondos

### backgroundColor
Establece el color de fondo. Soporta Hexadecimal (`'#fff'`), RGBA (`'rgba(0,0,0,0.5)'`) y nombres de color estándar (`'blue'`).

### opacity
Define la opacidad total del componente (incluyendo sus elementos hijos) de `0.0` a `1.0`.

### overflow
Define si el contenido que sobresale de las dimensiones del contenedor se visualiza (`'visible'`) o se oculta (`'hidden'`).

* **Ejemplo Sencillo:**
  ```typescript
  const styles = StyleSheet.create({
    fondoSemiTransparente: {
      backgroundColor: 'rgba(0, 0, 0, 0.7)',
    },
  });
  ```
* **Ejemplo Real:**
  ```typescript
  import React from 'react';
  import { View, Text, StyleSheet } from 'react-native';

  export const BannerRecortado = () => {
    return (
      <View style={styles.contenedorBanner}>
        <View style={styles.decoracionCirculo} />
        <Text style={styles.texto}>Contenido destacado</Text>
      </View>
    );
  };

  const styles = StyleSheet.create({
    contenedorBanner: {
      height: 120,
      backgroundColor: '#0f172a',
      borderRadius: 16,
      overflow: 'hidden', // Recorta el círculo decorativo sobrante
      justifyContent: 'center',
      padding: 24,
      position: 'relative',
    },
    decoracionCirculo: {
      position: 'absolute',
      width: 150,
      height: 150,
      borderRadius: 75,
      backgroundColor: 'rgba(255, 255, 255, 0.05)',
      top: -50,
      right: -50,
    },
    texto: {
      color: '#fff',
      fontSize: 18,
      fontWeight: 'bold',
    },
  });
  ```
* **Buenas Prácticas:** Si se desea hacer translúcido el fondo de una vista sin afectar la opacidad y legibilidad del texto e iconos que se encuentran dentro de ella, **no usar opacity: 0.5**. En su lugar, aplicar un color de fondo con formato RGBA.
* **Errores Comunes:** En iOS, aplicar `overflow: 'hidden'` en la misma vista contenedora donde declaras sombras nativas (`shadowColor`) eliminará las sombras. Se debe separar el contenedor en una vista externa para la sombra y una interna para el recorte de esquinas.
