# 07. Arquitectura, Sistema de Diseño y Responsive

Esta sección cubre las pautas para estructurar un Design System, organizar estilos en proyectos de gran escala, lograr interfaces adaptativas (responsive) e implementar Dark Mode de forma eficiente.

---

## 1. Organización del Proyecto

Para mantener el código escalable, se recomienda desacoplar la lógica visual de los componentes estructurándolos en archivos de estilo específicos y tokens de sistema de diseño globales.

### Estructura de Directorios Recomendada
```
src/
├── theme/
│   ├── colors.ts       # Constantes de color (Hexadecimal, RGBA, semánticos)
│   ├── spacing.ts      # Tokens de márgenes y paddings
│   ├── typography.ts   # Pesos de fuente, escalas de tamaño de letra y lineHeight
│   ├── radius.ts       # Curvas de esquina normalizadas
│   └── shadows.ts      # Definiciones de sombras nativas
├── components/
│   └── Button/
│       ├── Button.tsx
│       └── Button.styles.ts  # Hoja de estilos del botón
└── screens/
    └── Login/
        ├── Login.tsx
        └── Login.styles.ts   # Hoja de estilos de la pantalla
```

---

## 2. Tokens del Sistema de Diseño (Design System)

Al emplear tokens centralizados, se reducen las discrepancias de diseño y se estandariza el look & feel en múltiples plataformas.

### Espaciados (`src/theme/spacing.ts`)
```typescript
export const spacing = {
  xs: 4,
  sm: 8,
  md: 12,
  lg: 16,
  xl: 20,
  xxl: 24,
  xxxl: 32,
  huge: 40,
} as const;
```

### Esquinas (`src/theme/radius.ts`)
```typescript
export const radius = {
  none: 0,
  xs: 4,
  sm: 8,
  md: 12,
  lg: 16,
  xl: 24,
  round: 9999,
} as const;
```

### Tipografía (`src/theme/typography.ts`)
```typescript
export const typography = {
  h1: {
    fontSize: 32,
    fontWeight: '800' as const,
    lineHeight: 40,
  },
  h2: {
    fontSize: 24,
    fontWeight: '700' as const,
    lineHeight: 30,
  },
  h3: {
    fontSize: 20,
    fontWeight: '600' as const,
    lineHeight: 26,
  },
  body: {
    fontSize: 16,
    fontWeight: '400' as const,
    lineHeight: 22,
  },
  caption: {
    fontSize: 12,
    fontWeight: '400' as const,
    lineHeight: 16,
  },
} as const;
```

---

## 3. Estrategias Responsive

Los dispositivos móviles tienen múltiples anchos de pantalla. React Native ofrece herramientas integradas para maquetar interfaces fluidas y adaptativas.

### useWindowDimensions vs Dimensions
* `Dimensions`: Captura el tamaño al arrancar la app. Si la pantalla rota o se divide en tableta, no actualiza sus valores reactivamente.
* `useWindowDimensions`: **Opción recomendada**. Hook reactivo que actualiza el ancho (`width`) y alto (`height`) dinámicamente.

### Platform y PixelRatio
* `Platform.select`: Permite inyectar estilos condicionales basados en el sistema operativo.
* `PixelRatio`: Permite adaptar dimensiones según la resolución nativa de la pantalla física.

* **Ejemplo Real de Maquetación Responsive:**
  ```typescript
  import React from 'react';
  import { View, Text, StyleSheet, useWindowDimensions, Platform } from 'react-native';

  export const TarjetaResponsive = () => {
    const { width } = useWindowDimensions();
    const esTableta = width > 768;

    return (
      <View style={[styles.tarjeta, esTableta ? styles.tarjetaTableta : styles.tarjetaMovil]}>
        <Text style={styles.texto}>Layout adaptado a dispositivo</Text>
      </View>
    );
  };

  const styles = StyleSheet.create({
    tarjeta: {
      padding: 16,
      backgroundColor: '#fff',
      borderRadius: 12,
      ...Platform.select({
        ios: {
          shadowColor: '#000',
          shadowOffset: { width: 0, height: 2 },
          shadowOpacity: 0.1,
          shadowRadius: 4,
        },
        android: {
          elevation: 2,
        },
      }),
    },
    tarjetaMovil: {
      width: '100%',
      marginHorizontal: 16,
    },
    tarjetaTableta: {
      width: '50%',
      alignSelf: 'center', // Centra la tarjeta en pantallas grandes
    },
    texto: {
      fontSize: 16,
      textAlign: 'center',
    },
  });
  ```

---

## 4. Dark Mode (Modo Oscuro Dinámico)

Para resolver estilos en base a la configuración de apariencia activa (Claro / Oscuro) sin penalizar el rendimiento, se utiliza la inyección de temas mediante un Hook de estilos personalizado.

### Ejemplo Completo de Implementación

#### 1. Definición del Tema (`src/theme/theme.ts`)
```typescript
export const lightTheme = {
  background: '#ffffff',
  surface: '#f8fafc',
  text: '#0f172a',
  primary: '#3b82f6',
};

export const darkTheme = {
  background: '#121212',
  surface: '#1e1e1e',
  text: '#f8fafc',
  primary: '#60a5fa',
};

export type Theme = typeof lightTheme;
```

#### 2. Custom Hook de Estilos (`src/hooks/useStyles.ts`)
```typescript
import { useColorScheme } from 'react-native';
import { lightTheme, darkTheme, Theme } from '../theme/theme';

export function useStyles<T>(createStyles: (theme: Theme) => T): T {
  const scheme = useColorScheme();
  const theme = scheme === 'dark' ? darkTheme : lightTheme;
  return createStyles(theme);
}
```

#### 3. Uso en Componente
```typescript
import React from 'react';
import { View, Text, StyleSheet } from 'react-native';
import { useStyles } from '../../hooks/useStyles';
import { Theme } from '../../theme/theme';

export const PantallaConfiguracion = () => {
  const styles = useStyles(getStyles);

  return (
    <View style={styles.contenedor}>
      <Text style={styles.texto}>Tema Adaptativo Activo</Text>
    </View>
  );
};

const getStyles = (theme: Theme) => StyleSheet.create({
  contenedor: {
    flex: 1,
    backgroundColor: theme.background,
    justifyContent: 'center',
    alignItems: 'center',
  },
  texto: {
    color: theme.text,
    fontSize: 18,
    fontWeight: 'bold',
  },
});
```
