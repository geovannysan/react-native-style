# 06. Sombras y Componentes Interactivos

Esta sección explica cómo maquetar sombras multiplataforma y cómo estructurar áreas desplazables, seguras y reactivas a toques.

---

## 1. Sombras (Multiplataforma)

Las sombras operan de forma diferente entre iOS y Android.
* **iOS:** Utiliza propiedades de CoreGraphics (`shadowColor`, `shadowOffset`, `shadowOpacity`, `shadowRadius`).
* **Android:** Utiliza la propiedad física `elevation` basada en profundidad espacial de Material Design.

* **Ejemplo Real Multiplataforma:**
  ```typescript
  import { StyleSheet, Platform } from 'react-native';

  export const styles = StyleSheet.create({
    tarjetaSombreada: {
      backgroundColor: '#ffffff',
      borderRadius: 8,
      padding: 16,
      ...Platform.select({
        ios: {
          shadowColor: '#000000',
          shadowOffset: { width: 0, height: 2 },
          shadowOpacity: 0.1,
          shadowRadius: 6,
        },
        android: {
          elevation: 3,
        },
      }),
    },
  });
  ```

---

## 2. ScrollView

El componente `ScrollView` permite desplazar pantallas vertical u horizontalmente.

### Propiedades Clave de Estilizado
* `style`: Aplica estilos a la caja exterior del ScrollView.
* `contentContainerStyle`: Aplica estilos a la caja interna contenedora de los datos. **Los paddings internos y alineaciones de centrado deben colocarse aquí**.
* `keyboardShouldPersistTaps`: Define si el teclado se oculta al pulsar elementos de scroll (`'handled'`, `'never'`, `'always'`).
* `keyboardDismissMode`: Oculta el teclado dinámicamente al deslizar (`'on-drag'`, `'none'`).

* **Ejemplo Real:**
  ```typescript
  import React from 'react';
  import { ScrollView, View, Text, StyleSheet } from 'react-native';

  export const PantallaDesplazable = () => {
    return (
      <ScrollView 
        style={styles.scroll}
        contentContainerStyle={styles.cuerpoScroll}
        keyboardDismissMode="on-drag"
      >
        <Text style={styles.texto}>Contenido desplazable</Text>
        <View style={styles.cajaLarga} />
      </ScrollView>
    );
  };

  const styles = StyleSheet.create({
    scroll: {
      flex: 1,
      backgroundColor: '#f8fafc',
    },
    cuerpoScroll: {
      padding: 20,
      alignItems: 'center', // Alinea los componentes internos al centro
    },
    cajaLarga: {
      height: 900,
      width: '100%',
      backgroundColor: '#e2e8f0',
      marginTop: 20,
      borderRadius: 12,
    },
    texto: {
      fontSize: 18,
      fontWeight: 'bold',
    },
  });
  ```

---

## 3. SafeAreaView

* **Qué hace:** Aplica paddings automáticos a los bordes de la pantalla para evitar que el contenido colisione con el Notch de la cámara, esquinas físicas curvas o la barra inferior de gestos en iOS/Android.
* **Cuándo usarla:** Como vista base inicial en todas las pantallas del dispositivo.
* **Buenas Prácticas:** Se aconseja utilizar la librería `react-native-safe-area-context` en lugar del componente por defecto de React Native, ya que provee insets más precisos para dispositivos Android modernos.

* **Ejemplo de Código:**
  ```typescript
  import React from 'react';
  import { Text, StyleSheet } from 'react-native';
  import { SafeAreaView } from 'react-native-safe-area-context';

  export const PantallaSegura = () => {
    return (
      <SafeAreaView style={styles.pantalla}>
        <Text>Contenido seguro de muescas e islas dinámicas</Text>
      </SafeAreaView>
    );
  };

  const styles = StyleSheet.create({
    pantalla: {
      flex: 1,
      backgroundColor: '#fff',
    },
  });
  ```

---

## 4. KeyboardAvoidingView

* **Qué hace:** Desplaza o contrae la vista de la pantalla al activarse el teclado táctil para evitar que cubra los inputs de datos.
* **Propiedades clave:** `behavior` (`'padding'`, `'height'`, `'position'`).
* **Ejemplo Real Multiplataforma:**
  ```typescript
  import React from 'react';
  import { KeyboardAvoidingView, TextInput, StyleSheet, Platform, View } from 'react-native';

  export const FormularioTeclado = () => {
    return (
      <KeyboardAvoidingView 
        behavior={Platform.OS === 'ios' ? 'padding' : 'height'}
        style={styles.evitador}
      >
        <View style={styles.contenedor}>
          <TextInput style={styles.input} placeholder="Escribe aquí tu correo" />
        </View>
      </KeyboardAvoidingView>
    );
  };

  const styles = StyleSheet.create({
    evitador: {
      flex: 1,
    },
    contenedor: {
      flex: 1,
      justifyContent: 'flex-end',
      padding: 24,
    },
    input: {
      height: 48,
      borderWidth: 1,
      borderColor: '#cbd5e1',
      borderRadius: 8,
      paddingHorizontal: 12,
      backgroundColor: '#fff',
    },
  });
  ```

---

## 5. View

El elemento base estructural en React Native. Se compila directamente a las vistas nativas de Android e iOS.

### Propiedades Clave (No-Estilos)
* `onLayout`: Callback ejecutado tras calcularse la posición física del elemento. Retorna un objeto `{ nativeEvent: { layout: { x, y, width, height } } }`.
* `pointerEvents`: Controla la respuesta táctil de la vista y sus hijos (`'auto'`, `'none'`, `'box-none'`, `'box-only'`).

---

## 6. Pressable

Reemplaza los antiguos componentes `TouchableOpacity` y `TouchableHighlight`. Permite estilizar dinámicamente mediante el estado de la presión táctil del usuario.

### Estados Interactivos
* `pressed`: Booleano que indica si el usuario está pulsando el componente en tiempo real.
* `hovered` (Dispositivos compatibles con puntero).
* `focused` (Dispositivos compatibles con enfoque).

* **Ejemplo Real con Estilos Dinámicos:**
  ```typescript
  import React from 'react';
  import { Pressable, Text, StyleSheet } from 'react-native';

  export const BotonElegante = () => {
    return (
      <Pressable 
        style={({ pressed }) => [
          styles.botonBase,
          pressed ? styles.botonPresionado : styles.botonNormal
        ]}
      >
        <Text style={styles.textoBoton}>Guardar Cambios</Text>
      </Pressable>
    );
  };

  const styles = StyleSheet.create({
    botonBase: {
      height: 48,
      borderRadius: 8,
      justifyContent: 'center',
      alignItems: 'center',
      paddingHorizontal: 24,
    },
    botonNormal: {
      backgroundColor: '#3b82f6',
    },
    botonPresionado: {
      backgroundColor: '#1d4ed8',
      transform: [{ scale: 0.98 }], // Micro-animación de pulsación
    },
    textoBoton: {
      color: '#ffffff',
      fontWeight: 'bold',
      fontSize: 16,
    },
  });
  ```
