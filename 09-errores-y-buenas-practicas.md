# 09. Buenas Prácticas, Errores y Cheatsheet

Esta sección recopila herramientas de referencia rápida, soluciones a problemas comunes y reglas arquitectónicas para escribir estilos mantenibles en React Native.

---

## 1. Cheatsheet (Tabla de Propiedades Clave)

| Propiedad | Tipo | Valores permitidos | Uso recomendado | Ejemplo |
| :--- | :--- | :--- | :--- | :--- |
| `flex` | `number` | `>= 0` | Dividir espacio disponible | `flex: 1` |
| `flexDirection` | `string` | `'row' \| 'column' \| ...` | Definir eje del flujo | `flexDirection: 'row'` |
| `justifyContent`| `string` | `'center' \| 'space-between' \| ...`| Alineación en eje principal | `justifyContent: 'space-between'` |
| `alignItems` | `string` | `'center' \| 'stretch' \| ...` | Alineación en eje secundario| `alignItems: 'center'` |
| `alignSelf` | `string` | `'auto' \| 'flex-end' \| ...` | Auto-alineación individual | `alignSelf: 'flex-end'` |
| `gap` | `number` | `>= 0` | Espaciado entre hijos | `gap: 16` |
| `aspectRatio` | `number` | `number` | Relación de aspecto fija | `aspectRatio: 16 / 9` |
| `position` | `string` | `'relative' \| 'absolute'` | Modo de posicionamiento | `position: 'absolute'` |
| `elevation` | `number` | `number` | Sombras 3D en Android | `elevation: 5` |

---

## 2. Errores Frecuentes (30 Errores y Soluciones)

1. **Uso de unidades relativas web como `vh`, `vw`, `em` o `rem` en StyleSheet.**
   * *Error:* `width: '50vw'` resulta en un crash de la app en tiempo de ejecución.
   * *Solución:* Usar porcentajes de cadenas (`'50%'`) o recurrir al hook `useWindowDimensions`.

2. **Asignar múltiples objetos de estilos a la propiedad `style` usando llaves de objetos en lugar de un arreglo.**
   * *Error:* `style={{ styles.uno, styles.dos }}` (sintaxis no válida).
   * *Solución:* Usar sintaxis de arreglo `style={[styles.uno, styles.dos]}`.

3. **Colocar cadenas de texto sueltas fuera del componente `<Text>`.**
   * *Error:* `<View>Hola Mundo</View>` arroja error fatal en tiempo de compilación.
   * *Solución:* `<View><Text>Hola Mundo</Text></View>`.

4. **Estilo `flex: 1` colapsado a cero dentro de un ScrollView sin contenedor.**
   * *Error:* Un contenedor interno de un `ScrollView` con `flex: 1` no muestra nada.
   * *Solución:* Usar `contentContainerStyle` con `flexGrow: 1` en el `ScrollView`.

5. **El componente `Image` de red no se muestra en pantalla.**
   * *Error:* `<Image source={{ uri: 'url' }} />` no dibuja nada.
   * *Solución:* Las imágenes externas requieren que definas sus dimensiones (`width` y `height` o `aspectRatio`) obligatoriamente.

6. **Intentar heredar estilos de texto de un contenedor padre a hijos.**
   * *Error:* `<View style={{ color: 'red' }}><Text>Hola</Text></View>` no cambia el color del texto.
   * *Solución:* Aplicar directamente el estilo al componente `<Text>`.

7. **Uso de la propiedad obsoleta `marginHorizontal` mezclada con `marginLeft` de forma imprevista.**
   * *Error:* La precedencia de estas propiedades en dispositivos difiere.
   * *Solución:* Declarar o bien `marginHorizontal`, o bien combinaciones limpias de `marginLeft`/`marginRight` por separado.

8. **La propiedad `zIndex` no funciona en dispositivos Android.**
   * *Error:* Un menú desplegable absoluto queda tapado por la vista inferior en Android pero no en iOS.
   * *Solución:* En Android, añade `elevation` para asegurar la renderización sobre el plano 3D correcto.

9. **La sombra `shadowColor` e iOS desaparecen al aplicar `overflow: 'hidden'`.**
   * *Error:* Esquinas recortadas eliminan la sombra difusa.
   * *Solución:* Separar el componente en dos: una vista padre que contenga la sombra y una vista hija con el borde y el recorte.

10. **Alineación `alignItems: 'stretch'` no funciona si defines un ancho fijo.**
    * *Error:* Los componentes no cubren el ancho completo a pesar de usar stretch.
    * *Solución:* Elimina `width` o `flexBasis` del elemento secundario que deseas estirar.

11. **Textos sin `flexShrink: 1` rompiendo la cuadrícula en filas (`row`).**
    * *Error:* El texto largo empuja los elementos laterales fuera de la pantalla.
    * *Solución:* Añadir `flexShrink: 1` (o `flex: 1`) al componente `<Text>`.

12. **La propiedad `borderRadius` no hace círculo perfecto en imágenes en iOS.**
    * *Error:* La imagen se ve ovalada.
    * *Solución:* Asegura que el `width` y `height` sean iguales, y añade `overflow: 'hidden'` a los estilos de la imagen.

13. **Aplicar estilos condicionales en línea recreando objetos de StyleSheet continuamente.**
    * *Error:* Degradación de rendimiento por renderizado.
    * *Solución:* `style={[styles.boton, pressed && styles.botonPresionado]}`.

14. **El teclado tapa los inputs de la mitad inferior de la pantalla.**
    * *Error:* El usuario no puede ver lo que escribe.
    * *Solución:* Envolver la vista en un `KeyboardAvoidingView` con el behavior configurado.

15. **Uso de estilos abreviados al estilo CSS para bordes.**
    * *Error:* `border: '1px solid #ccc'` no es válido.
    * *Solución:* Escribir `borderWidth: 1`, `borderColor: '#ccc'`, `borderStyle: 'solid'`.

16. **Colapsar un contenedor flexible por utilizar `flexDirection: 'row'` sin ajustar el ancho.**
    * *Error:* Los elementos se comprimen horizontalmente.
    * *Solución:* Si es dinámico, permite el salto de línea con `flexWrap: 'wrap'`.

17. **Cálculo responsivo inexacto por usar `Dimensions.get('window')` en una sola inicialización.**
    * *Error:* Rotar la pantalla rompe la cuadrícula responsiva.
    * *Solución:* Utilizar el hook dinámico `useWindowDimensions`.

18. **Uso de colores RGBA sin formato de cadena de texto.**
    * *Error:* `backgroundColor: rgba(0,0,0,0.5)` causa error de sintaxis.
    * *Solución:* Usar comillas: `backgroundColor: 'rgba(0,0,0,0.5)'`.

19. **El componente `SafeAreaView` nativo de React Native no respeta los costados laterales en landscape.**
    * *Error:* Botones ocultos en notches laterales al rotar.
    * *Solución:* Usar la biblioteca de terceros `react-native-safe-area-context` y su hook `useSafeAreaInsets`.

20. **Definir `fontWeight` utilizando números directamente en TypeScript.**
    * *Error:* `fontWeight: 600` arroja error de tipos en TypeScript.
    * *Solución:* Pasar el número como string: `fontWeight: '600'`.

21. **No poder pulsar elementos posicionados absolutamente que sobresalen de su padre.**
    * *Error:* El botón es visible pero no reacciona al toque.
    * *Solución:* El contenedor padre debe ser lo suficientemente grande o remover `overflow: 'hidden'`.

22. **Uso incorrecto de `flex: -1`.**
    * *Error:* Los layouts colapsan inesperadamente.
    * *Solución:* Utiliza solo `flexGrow: 0` y `flexShrink: 1` para este comportamiento.

23. **El espaciado `gap` no se renderiza en versiones de React Native antiguas.**
    * *Error:* Todo el diseño se ve amontonado en dispositivos antiguos.
    * *Solución:* Actualizar React Native a >= 0.71 o usar márgenes normales condicionales.

24. **Intentar usar `transform` como un string plano (como en la Web).**
    * *Error:* `transform: 'rotate(45deg)'` arroja un error en tiempo de ejecución.
    * *Solución:* Usar arreglo de objetos: `transform: [{ rotate: '45deg' }]`.

25. **El texto no tiene padding vertical uniforme debido al font padding en Android.**
    * *Error:* El texto parece alineado más hacia abajo.
    * *Solución:* Configurar `includeFontPadding: false` en los estilos del componente `Text`.

26. **Modificar estilos directamente a través del estado para animaciones.**
    * *Error:* Animaciones lentas y con retraso (lag).
    * *Solución:* Utilizar la API nativa de `Animated` o la librería `React Native Reanimated`.

27. **Uso de `pointerEvents` en vistas que deben capturar gestos.**
    * *Error:* La vista ignora completamente los toques del usuario.
    * *Solución:* Configurar `pointerEvents="auto"` o remover la propiedad.

28. **Modificar el tamaño de fuente usando `lineHeight` menor que el `fontSize`.**
    * *Error:* Las líneas de texto se superponen y se vuelven ilegibles.
    * *Solución:* `lineHeight` siempre debe ser igual o superior al valor del `fontSize`.

29. **Uso de `aspectRatio` como cadena de texto.**
    * *Error:* `aspectRatio: "16:9"` falla en tiempo de ejecución.
    * *Solución:* Usar una relación numérica directa: `aspectRatio: 16 / 9` o `1.77`.

30. **No definir `flexDirection` pensando que heredará `row` de la especificación web.**
    * *Error:* Los elementos quedan ordenados verticalmente por defecto.
    * *Solución:* Especificar explícitamente `flexDirection: 'row'` en el contenedor padre.

---

## 3. Buenas Prácticas (50 Recomendaciones)

### Arquitectura y Mantenimiento
1. Define todos tus estilos a través de `StyleSheet.create`. No uses estilos en línea (`style={{...}}`) excepto para valores computados dinámicamente.
2. Nombra tus archivos de estilo con la extensión `.styles.ts` junto a su componente funcional correspondiente.
3. Centraliza tu paleta de colores en un solo archivo global `theme/colors.ts`.
4. Evita el "hardcoding" de espaciados; utiliza una escala de paso fijo (e.g., múltiplos de 4 u 8 píxeles).
5. Mantén los estilos ordenados alfabéticamente dentro de cada objeto para facilitar su búsqueda.
6. Separa los estilos de layouts estructurales de los estilos puramente estéticos (color, bordes, sombras).
7. Utiliza TypeScript estricto para validar los tipos de tus hojas de estilo.
8. No crees hojas de estilo dinámicas dentro del cuerpo de tus componentes funcionales; esto recrea las referencias en cada renderizado.
9. Usa un hook especializado (`useStyles`) si necesitas resolver temas (claro/oscuro) dinámicos.
10. Comenta la intención de layouts complejos (e.g., por qué usas un hack de posicionamiento absoluto).

### Rendimiento de Renderizado (Performance)
11. Prefiere `flexWrap: 'wrap'` y `gap` antes que cálculos complejos de pantallas basados en layouts absolutos.
12. Recuerda que `StyleSheet` devuelve una referencia entera optimizada en producción.
13. Minimiza el anidamiento excesivo de contenedores `<View>` para evitar árboles de vistas profundos y costosos en Android.
14. Utiliza `FlatList` en lugar de `ScrollView` para listas extensas; los elementos fuera de pantalla se destruyen liberando memoria.
15. Activa el uso del hilo nativo para transformaciones y animaciones configurando `useNativeDriver: true`.
16. Evita recalcular estilos dinámicos pesados dentro de funciones que se ejecutan 60 veces por segundo (como listeners de Scroll).
17. Utiliza `memo` en componentes con hojas de estilo muy cargadas si se re-renderizan sin cambiar de propiedades.
18. Configura el `getItemLayout` en tus listas `FlatList` para agilizar los tiempos de renderizado iniciales.
19. No mezcles componentes animados de diferentes librerías (e.g., `Animated` nativo y `Reanimated` de terceros) en una misma vista.
20. Evita el uso de `opacity` en contenedores que tienen muchos hijos; renderizar capas translúcidas ralentiza el GPU.

### Responsividad y Adaptabilidad
21. Diseña siempre pensando en el dispositivo móvil de menor tamaño disponible en el mercado (Mobile-First).
22. Utiliza `useWindowDimensions` para adaptaciones responsivas reactivas a la rotación del dispositivo.
23. Evita definir dimensiones fijas de `width` y `height` en contenedores raíz; usa porcentajes o `flex: 1`.
24. Aplica `flexShrink: 1` en contenedores que tengan texto largo para evitar roturas visuales.
25. Configura `numberOfLines` y `ellipsizeMode` en componentes de texto vulnerables a desbordamiento.
26. Asegúrate de verificar cómo se ven tus modales en modo Landscape (pantalla horizontal).
27. Utiliza la densidad de píxeles (`PixelRatio.roundToNearestPixel`) al dibujar bordes extra finos de diseño.
28. Nunca asumas que el Notch está ausente. Usa siempre los insets seguros de `react-native-safe-area-context`.
29. Verifica que los botones tengan un área interactiva mínima de **48x48 píxeles lógicos** para cumplir con estándares de accesibilidad física.
30. Prueba de manera rigurosa tus layouts tanto en tabletas como en smartphones pequeños (e.g., iPhone SE).

### Tipografía y Texto
31. Define la altura de línea (`lineHeight`) siempre que aumentes el tamaño del texto para que no se peguen las líneas.
32. Desactiva el padding de fuentes en Android (`includeFontPadding: false`) para asegurar una simetría vertical multiplataforma.
33. Centraliza las familias tipográficas para fuentes personalizadas en un tema común.
34. Utiliza componentes de texto dedicados (e.g., `<BodyText>`, `<Heading>`) en lugar de estilizar cada etiqueta `<Text>` individualmente.
35. Evita justificar textos (`textAlign: 'justify'`) en dispositivos móviles, suele lucir mal debido al espacio entre palabras.
36. Utiliza `textTransform` solo para transformaciones estilísticas y no para lógicas de datos del negocio.
37. Asegura un contraste visual de color suficiente entre el texto y el color de fondo para cumplir con las pautas de accesibilidad WCAG.
38. Carga y pre-compila fuentes personalizadas en la pantalla de carga (Splash Screen) para evitar el salto visual del texto (Flash of Unstyled Text).
39. Utiliza `adjustsFontSizeToFit` en títulos cortos para que se encojan automáticamente si no caben en una línea.
40. Agrupa estilos tipográficos en objetos compuestos dentro de la hoja de estilos global para reutilizarlos mediante destructuración.

### Buenas Prácticas en el Ecosistema Expo
41. Evita alterar los valores de configuración globales de `StyleSheet` nativos de manera directa.
42. Mantén actualizado el archivo de configuración `app.json` para definir la orientación y adaptabilidad de la app.
43. Utiliza la librería expo-google-fonts para cargar tipografías de manera segura y óptima.
44. Valida tus vistas utilizando el simulador oficial de Expo Go tanto para Android como para iOS.
45. Implementa el modo oscuro dinámico directamente conectado con el tema de la configuración de Expo.
46. No manipules la barra de estado (`StatusBar`) con estilos en línea; prefiere componentes declarativos del tema.
47. Si utilizas la navegación de Expo Router, estiliza los headers utilizando las opciones integradas del componente `<Stack.Screen>`.
48. Utiliza `expo-image` en lugar del componente `<Image>` nativo para obtener mejor caché y rendimiento en imágenes pesadas.
49. Estructura layouts independientes para tablets si tu app es híbrida, en lugar de intentar forzar un diseño móvil a pantalla completa.
50. Revisa constantemente las advertencias en la consola de depuración de Expo para corregir malas declaraciones de Flexbox y evitar crashes en producción.
