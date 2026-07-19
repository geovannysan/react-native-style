# 05. Elementos de Texto e Imágenes

Esta sección detalla cómo dar formato a textos y cómo escalar imágenes de forma nativa en pantallas móviles.

---

## 1. Texto

En React Native no es posible colocar cadenas de caracteres crudas. Todos los textos deben estar encapsulados por el componente `<Text>`.

### Propiedades de Texto
* `fontSize`: Tamaño en píxeles lógicos.
* `fontWeight`: Peso de la fuente (`'normal'`, `'bold'`, `'100'`, ..., `'900'`).
* `fontStyle`: Estilo de la fuente (`'normal'`, `'italic'`).
* `color`: Color del texto.
* `textAlign`: Alineación del texto (`'auto'`, `'left'`, `'right'`, `'center'`, `'justify'`).
* `lineHeight`: Altura de cada línea o renglón de texto.
* `letterSpacing`: Espaciado entre caracteres.
* `textTransform`: Transformación tipográfica (`'none'`, `'uppercase'`, `'lowercase'`, `'capitalize'`).
* `includeFontPadding` (Solo Android): Activa/Desactiva padding vertical extra incorporado por defecto en la fuente de Android. Se recomienda `false`.
* `textAlignVertical` (Solo Android): Alinea verticalmente dentro del marco de la vista de texto (`'auto'`, `'top'`, `'bottom'`, `'center'`).

* **Ejemplo Sencillo:**
  ```typescript
  import { StyleSheet } from 'react-native';

  const styles = StyleSheet.create({
    textoTitulo: {
      fontSize: 20,
      fontWeight: 'bold',
      color: '#1e293b',
    },
  });
  ```
* **Ejemplo Real:**
  ```typescript
  import React from 'react';
  import { View, Text, StyleSheet } from 'react-native';

  export const ArticuloBreve = () => {
    return (
      <View style={styles.contenedor}>
        <Text style={styles.categoria}>TECNOLOGÍA</Text>
        <Text style={styles.titulo}>Novedades en React Native</Text>
        <Text style={styles.cuerpo}>
          El motor de renderizado Yoga continúa implementando mejoras de rendimiento 
          que reducen significativamente el tiempo de cálculo de layouts complejos.
        </Text>
      </View>
    );
  };

  const styles = StyleSheet.create({
    contenedor: {
      padding: 16,
      backgroundColor: '#fff',
    },
    categoria: {
      fontSize: 12,
      fontWeight: '800',
      color: '#3b82f6',
      letterSpacing: 1.5,
      textTransform: 'uppercase',
    },
    titulo: {
      fontSize: 24,
      fontWeight: '800',
      color: '#0f172a',
      lineHeight: 30, // Ajustado proporcionalmente al tamaño
      marginVertical: 8,
    },
    cuerpo: {
      fontSize: 15,
      color: '#475569',
      lineHeight: 22, // Facilita la lectura de párrafos largos
      includeFontPadding: false, // Alineación vertical consistente en Android
    },
  });
  ```
* **Buenas Prácticas:**
  * Configurar siempre `includeFontPadding: false` en los textos de Android para garantizar que los contenedores no muestren padding extra e irregular arriba y abajo.
  * Mantener una escala tipográfica centralizada en un archivo global (por ejemplo, `theme/typography.ts`).
* **Errores Comunes:**
  * Intentar pasar el valor de `fontWeight` como número entero (e.g., `fontWeight: 700`). TypeScript fallará de inmediato. Debe ser un string (`fontWeight: '700'`).
  * Estructurar el `lineHeight` con un valor numérico menor que el `fontSize`, lo que hace que los renglones se encimen e invaliden la lectura.

---

## 2. Imágenes

El componente `<Image>` se utiliza para desplegar imágenes de recursos locales (`require`) o URLs remotas.

### resizeMode
Determina cómo se escala y acomoda una imagen cuando su proporción original no coincide con el contenedor físico asignado en la hoja de estilos.

#### Valores principales:
* `cover` (Predeterminado): Escala la imagen proporcionalmente para llenar la caja. Corta partes sobrantes de la imagen para que no queden huecos vacíos.
* `contain`: Escala la imagen proporcionalmente para que se visualice completa dentro de la caja. Puede dejar espacios vacíos en los costados.
* `stretch`: Estira la imagen de forma asimétrica para cubrir exactamente el ancho y alto del contenedor. Causa deformación.
* `center`: Centra la imagen sin aplicar ningún escalado en absoluto.
* `repeat`: Duplica y repite la imagen a modo de mosaico o textura.

* **Ejemplo Sencillo:**
  ```typescript
  const styles = StyleSheet.create({
    imagenAjustada: {
      width: 100,
      height: 100,
      resizeMode: 'contain',
    },
  });
  ```
* **Ejemplo Real:**
  ```typescript
  import React from 'react';
  import { View, Image, StyleSheet } from 'react-native';

  export const TarjetaProductoImagen = () => {
    return (
      <View style={styles.contenedor}>
        <Image 
          source={{ uri: 'https://midominio.com/zapatilla.png' }} 
          style={styles.imagenZapatilla}
          resizeMode="contain" // Muestra el producto completo sin recortes
        />
      </View>
    );
  };

  const styles = StyleSheet.create({
    contenedor: {
      width: 150,
      height: 150,
      backgroundColor: '#f1f5f9',
      borderRadius: 12,
      justifyContent: 'center',
      alignItems: 'center',
      padding: 8,
    },
    imagenZapatilla: {
      width: '100%',
      height: '100%',
    },
  });
  ```
* **Buenas Prácticas:**
  * Para avatares e imágenes de cabecera de artículos, utilizar `resizeMode="cover"` combinado con `overflow: 'hidden'`.
  * En la carga de imágenes remotas (URLs de red), definir siempre dimensiones de `width` y `height` o la proporción con `aspectRatio`; de lo contrario, la imagen no se renderizará.
* **Errores Comunes:** Utilizar `resizeMode="stretch"` en imágenes corporativas o logotipos de marcas, ya que deforma la imagen y reduce la calidad visual de la aplicación.
