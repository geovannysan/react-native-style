# 03. Dimensiones y Espaciado

Esta sección detalla cómo controlar el tamaño físico de los elementos y cómo establecer márgenes internos y externos.

---

## 1. Tamaños

### width y height
Establecen las dimensiones fijas del componente. Admiten valores numéricos (píxeles lógicos) y porcentajes como strings (e.g., `'100%'`).

### minWidth, maxWidth, minHeight, maxHeight
Definen límites estrictos en el tamaño del componente. Muy útiles para prevenir comportamientos incontrolados en pantallas gigantes o extremadamente pequeñas (e.g., tabletas y teléfonos plegables).

### aspectRatio
* **Qué hace:** Obliga al componente a mantener una proporción fija entre su ancho y alto.
* **Cuándo usarla:** Para renderizar miniaturas de videos, banners de publicidad o previsualizaciones de cámaras que deban mantener proporciones sin deformarse.
* **Cuándo NO usarla:** Si el componente requiere que su ancho y alto cambien de forma independiente según el contenido interno.
* **Ejemplo Sencillo:**
  ```typescript
  import { StyleSheet } from 'react-native';

  const styles = StyleSheet.create({
    cajaCuadrada: {
      width: '100%',
      aspectRatio: 1, // Alto igual al ancho calculado
    },
  });
  ```
* **Ejemplo Real:**
  ```typescript
  import React from 'react';
  import { View, Image, StyleSheet } from 'react-native';

  export const BannerVideo = () => {
    return (
      <View style={styles.contenedor}>
        <Image 
          source={{ uri: 'https://midominio.com/banner.jpg' }} 
          style={styles.imagenBanner}
        />
      </View>
    );
  };

  const styles = StyleSheet.create({
    contenedor: {
      width: '100%',
      padding: 16,
    },
    imagenBanner: {
      width: '100%',
      aspectRatio: 16 / 9, // Previene deformación en cualquier dispositivo
      borderRadius: 12,
    },
  });
  ```
* **Buenas Prácticas:** Usar `aspectRatio` al trabajar con imágenes remotas para evitar destellos y cambios bruscos de maquetación durante la carga.
* **Errores Comunes:** Intentar pasar `aspectRatio` como un string tipo `"16:9"`, lo cual arroja un error de ejecución. Debe ser un número decimal (`1.777`) o fracción (`16 / 9`).

---

## 2. Espaciado (Margin & Padding)

### Diferencia Conceptual: Margin vs Padding
```
┌────────────────────────────────────────────────────────┐
│                      MARGIN (Externo)                  │
│   ┌────────────────────────────────────────────────┐   │
│   │                  BORDER                        │   │
│   │   ┌────────────────────────────────────────┐   │   │
│   │   │              PADDING (Interno)         │   │   │
│   │   │   ┌────────────────────────────────┐   │   │   │
│   │   │   │            CONTENIDO           │   │   │   │
│   │   │   └────────────────────────────────┘   │   │   │
│   │   └────────────────────────────────────────┘   │   │
│   └────────────────────────────────────────────────┘   │
└────────────────────────────────────────────────────────┘
```
* **Margin (Margen):** Espacio libre situado en el exterior de los bordes del componente. Se utiliza para empujar y separar al elemento de sus vecinos en la interfaz.
* **Padding (Relleno):** Espacio libre situado entre el contenido interior y los límites físicos o bordes del propio componente.

### Atajos Direccionales
React Native ofrece abreviaciones muy potentes para evitar código redundante:
* `marginHorizontal`: Modifica `marginLeft` y `marginRight` simultáneamente.
* `marginVertical`: Modifica `marginTop` y `marginBottom` simultáneamente.
* `paddingHorizontal` y `paddingVertical` operan exactamente igual para el relleno interno.

* **Ejemplo Sencillo:**
  ```typescript
  const styles = StyleSheet.create({
    cajaEspaciada: {
      marginVertical: 12,
      paddingHorizontal: 16,
    },
  });
  ```
* **Ejemplo Real:**
  ```typescript
  import React from 'react';
  import { View, Text, StyleSheet, Pressable } from 'react-native';

  export const TarjetaInformativa = () => {
    return (
      <View style={styles.tarjeta}>
        <Text style={styles.titulo}>Novedades del Servicio</Text>
        <Text style={styles.descripcion}>
          Hemos actualizado los términos y condiciones de uso para brindar mayor seguridad.
        </Text>
        <Pressable style={styles.botonOk}>
          <Text style={styles.txtBoton}>Entendido</Text>
        </Pressable>
      </View>
    );
  };

  const styles = StyleSheet.create({
    tarjeta: {
      backgroundColor: '#fff',
      paddingHorizontal: 20, // Relleno a los lados del texto
      paddingVertical: 16,   // Relleno arriba y abajo del texto
      borderRadius: 12,
      marginHorizontal: 16,  // Separa la tarjeta de los bordes de la pantalla
      marginVertical: 8,     // Separa la tarjeta de las tarjetas vecinas
      borderWidth: 1,
      borderColor: '#e2e8f0',
    },
    titulo: {
      fontSize: 18,
      fontWeight: 'bold',
      marginBottom: 8, // Empuja la descripción hacia abajo
    },
    descripcion: {
      fontSize: 14,
      color: '#475569',
      marginBottom: 16, // Empuja el botón hacia abajo
    },
    botonOk: {
      backgroundColor: '#0f172a',
      paddingVertical: 10,
      borderRadius: 8,
      alignItems: 'center',
    },
    txtBoton: {
      color: '#fff',
      fontWeight: '600',
    },
  });
  ```
* **Buenas Prácticas:** Usar constantes de espaciado centralizadas en un Design System (e.g., `theme.spacing.lg` en lugar de números sueltos) para garantizar consistencia.
* **Errores Comunes:** Intentar mezclar `marginHorizontal` con propiedades direccionales específicas como `marginLeft` en el mismo estilo, lo que provoca inconsistencia de precedencia visual en Android e iOS.
