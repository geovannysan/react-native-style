# Guía Oficial de Estilos y Layout en React Native (Expo + TypeScript)

Bienvenido a la documentación modular de estilos y diseño para React Native. Esta guía está diseñada para desarrolladores de todos los niveles con el fin de estandarizar la maquetación y la experiencia visual de nuestras aplicaciones móviles.

## 📂 Contenido de la Guía

Para facilitar su consulta y lectura, la guía se encuentra dividida en las siguientes secciones temáticas:

1. **[01. Introducción y Flexbox](guia-estilos-react-native/01-introduccion-y-flexbox.md)**
   * ¿Qué es StyleSheet?
   * Diferencias entre CSS y React Native.
   * Conceptos clave de Flexbox (`flex`, `flexGrow`, `flexShrink`, `flexBasis`).
   * Dirección (`flexDirection`).

2. **[02. Alineación y Distribución](guia-estilos-react-native/02-alineacion-y-distribucion.md)**
   * Alineación Horizontal (`justifyContent`).
   * Alineación Vertical (`alignItems`).
   * Alineación Individual (`alignSelf`).
   * Distribución de elementos (`gap`, `rowGap`, `columnGap`).

3. **[03. Dimensiones y Espaciado](guia-estilos-react-native/03-dimensiones-y-espaciado.md)**
   * Tamaños (`width`, `height`, `minWidth`, `maxWidth`, `aspectRatio`).
   * Espaciado (`margin` vs `padding` y atajos direccionales).

4. **[04. Posicionamiento, Bordes y Fondos](guia-estilos-react-native/04-posicionamiento-y-bordes.md)**
   * Posicionamiento (`relative`, `absolute`, `zIndex`).
   * Bordes (`borderWidth`, `borderColor`, `borderRadius`).
   * Fondos y visibilidad (`backgroundColor`, `opacity`, `overflow`).

5. **[05. Elementos de Texto e Imágenes](guia-estilos-react-native/05-textos-e-imagenes.md)**
   * Tipografías y Texto (`fontSize`, `fontWeight`, `lineHeight`, etc.).
   * Escalado de Imágenes (`resizeMode`).

6. **[06. Sombras y Componentes Interactivos](guia-estilos-react-native/06-sombras-y-componentes-scroll.md)**
   * Sombras nativas (iOS vs Android `elevation`).
   * Contenedores de desplazamiento (`ScrollView` y `contentContainerStyle`).
   * Áreas seguras e interacción con el teclado (`SafeAreaView`, `KeyboardAvoidingView`).
   * Estructuras y botones (`View`, `Pressable` y estilos dinámicos de estado).

7. **[07. Arquitectura, Sistema de Diseño y Responsive](guia-estilos-react-native/07-arquitectura-y-responsive.md)**
   * Organización y estructura modular de carpetas.
   * Configuración de un Design System (Espaciados, Border Radius, Tipografías).
   * Estrategias responsivas (`Dimensions`, `useWindowDimensions`, `Platform`, `PixelRatio`).
   * Implementación de Dark Mode usando Hooks personalizados.

8. **[08. Ejemplos Completos de Pantallas](guia-estilos-react-native/08-ejemplos-completos.md)**
   * Login, Card, Lista de elementos, Perfil de usuario, Dashboard, Formulario interactivo y Modal.

9. **[09. Buenas Prácticas, Errores y Cheatsheet](guia-estilos-react-native/09-errores-y-buenas-practicas.md)**
   * Hojas de trucos rápida (Cheatsheet) de propiedades autorizadas.
   * Lista detallada de 30 errores frecuentes y cómo resolverlos.
   * 50 Reglas y buenas prácticas para mantener un código limpio y escalable.

---
*Esta documentación ha sido generada bajo estándares de TypeScript estricto y el motor de maquetación Yoga de React Native.*
