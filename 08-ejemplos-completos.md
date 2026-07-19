# 08. Ejemplos Completos de Pantallas

Esta sección expone 7 maquetaciones de componentes e interfaces reales listas para producción. Todo el código está tipado bajo estándares estrictos de TypeScript.

---

## 1. Pantalla de Login (Inicio de Sesión)
```typescript
import React, { useState } from 'react';
import { View, Text, TextInput, Pressable, StyleSheet, KeyboardAvoidingView, Platform } from 'react-native';

export const LoginScreen = () => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  return (
    <KeyboardAvoidingView 
      behavior={Platform.OS === 'ios' ? 'padding' : 'height'} 
      style={styles.evitador}
    >
      <View style={styles.contenedor}>
        <View style={styles.cabecera}>
          <Text style={styles.titulo}>Bienvenido</Text>
          <Text style={styles.subtitulo}>Ingresa tus credenciales para continuar</Text>
        </View>

        <View style={styles.formulario}>
          <Text style={styles.label}>Correo Electrónico</Text>
          <TextInput 
            style={styles.input} 
            value={email} 
            onChangeText={setEmail} 
            placeholder="ejemplo@correo.com"
            keyboardType="email-address"
            autoCapitalize="none"
          />

          <Text style={styles.label}>Contraseña</Text>
          <TextInput 
            style={styles.input} 
            value={password} 
            onChangeText={setPassword} 
            placeholder="••••••••"
            secureTextEntry
          />

          <Pressable style={styles.boton}>
            <Text style={styles.textoBoton}>Iniciar Sesión</Text>
          </Pressable>
        </View>
      </View>
    </KeyboardAvoidingView>
  );
};

const styles = StyleSheet.create({
  evitador: {
    flex: 1,
    backgroundColor: '#f8fafc',
  },
  contenedor: {
    flex: 1,
    padding: 24,
    justifyContent: 'center',
  },
  cabecera: {
    marginBottom: 32,
  },
  titulo: {
    fontSize: 28,
    fontWeight: '800',
    color: '#0f172a',
  },
  subtitulo: {
    fontSize: 14,
    color: '#64748b',
    marginTop: 8,
  },
  formulario: {
    backgroundColor: '#ffffff',
    padding: 20,
    borderRadius: 16,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 4 },
    shadowOpacity: 0.05,
    shadowRadius: 12,
    elevation: 3,
  },
  label: {
    fontSize: 14,
    fontWeight: '600',
    color: '#334155',
    marginBottom: 8,
    marginTop: 16,
  },
  input: {
    height: 48,
    borderWidth: 1,
    borderColor: '#cbd5e1',
    borderRadius: 8,
    paddingHorizontal: 16,
    backgroundColor: '#f8fafc',
  },
  boton: {
    backgroundColor: '#3b82f6',
    height: 48,
    borderRadius: 8,
    justifyContent: 'center',
    alignItems: 'center',
    marginTop: 24,
  },
  textoBoton: {
    color: '#ffffff',
    fontWeight: 'bold',
    fontSize: 16,
  },
});
```

---

## 2. Tarjeta de Producto (Card)
```typescript
import React from 'react';
import { View, Text, Image, Pressable, StyleSheet } from 'react-native';

export const CustomCard = () => {
  return (
    <View style={styles.tarjeta}>
      <Image 
        source={{ uri: 'https://images.unsplash.com/photo-1542291026-7eec264c27ff' }} 
        style={styles.imagen}
        resizeMode="cover"
      />
      <View style={styles.cuerpo}>
        <Text style={styles.categoria}>NUEVO</Text>
        <Text style={styles.titulo}>Tenis Running Pro 2026</Text>
        <Text style={styles.precio}>$129.99 USD</Text>
        <Pressable style={styles.botonDetalle}>
          <Text style={styles.textoBoton}>Ver Producto</Text>
        </Pressable>
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  tarjeta: {
    backgroundColor: '#fff',
    borderRadius: 16,
    overflow: 'hidden', // Necesario para recortar la imagen en las esquinas
    borderWidth: 1,
    borderColor: '#e2e8f0',
    maxWidth: 320,
  },
  imagen: {
    width: '100%',
    height: 180,
  },
  cuerpo: {
    padding: 16,
  },
  categoria: {
    fontSize: 10,
    fontWeight: '800',
    color: '#ef4444',
    letterSpacing: 1,
  },
  titulo: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#1e293b',
    marginTop: 4,
  },
  precio: {
    fontSize: 16,
    color: '#10b981',
    fontWeight: '600',
    marginTop: 8,
  },
  botonDetalle: {
    backgroundColor: '#0f172a',
    paddingVertical: 10,
    borderRadius: 8,
    alignItems: 'center',
    marginTop: 16,
  },
  textoBoton: {
    color: '#fff',
    fontWeight: '600',
  },
});
```

---

## 3. Lista de Elementos (FlatList)
```typescript
import React from 'react';
import { FlatList, View, Text, StyleSheet, Pressable } from 'react-native';

interface ItemLista {
  id: string;
  nombre: string;
  email: string;
}

const DATOS: ItemLista[] = [
  { id: '1', nombre: 'Ana Gomez', email: 'ana@ejemplo.com' },
  { id: '2', nombre: 'Carlos Perez', email: 'carlos@ejemplo.com' },
  { id: '3', nombre: 'Maria Silva', email: 'maria@ejemplo.com' },
];

export const PantallaLista = () => {
  const renderItem = ({ item }: { item: ItemLista }) => (
    <Pressable style={styles.filaItem}>
      <View style={styles.inicialesCirculo}>
        <Text style={styles.textoIniciales}>{item.nombre.substring(0, 2).toUpperCase()}</Text>
      </View>
      <View style={styles.info}>
        <Text style={styles.nombre}>{item.nombre}</Text>
        <Text style={styles.email}>{item.email}</Text>
      </View>
      <Text style={styles.chevron}>→</Text>
    </Pressable>
  );

  return (
    <FlatList 
      data={DATOS}
      renderItem={renderItem}
      keyExtractor={item => item.id}
      ItemSeparatorComponent={() => <View style={styles.separador} />}
      contentContainerStyle={styles.listaContenedor}
    />
  );
};

const styles = StyleSheet.create({
  listaContenedor: {
    padding: 16,
    backgroundColor: '#fff',
  },
  filaItem: {
    flexDirection: 'row',
    alignItems: 'center',
    paddingVertical: 12,
  },
  inicialesCirculo: {
    width: 44,
    height: 44,
    borderRadius: 22,
    backgroundColor: '#e0f2fe',
    justifyContent: 'center',
    alignItems: 'center',
  },
  textoIniciales: {
    color: '#0284c7',
    fontWeight: 'bold',
    fontSize: 14,
  },
  info: {
    flex: 1,
    marginLeft: 16,
  },
  nombre: {
    fontSize: 16,
    fontWeight: '600',
    color: '#1e293b',
  },
  email: {
    fontSize: 14,
    color: '#64748b',
    marginTop: 2,
  },
  chevron: {
    fontSize: 18,
    color: '#cbd5e1',
  },
  separador: {
    height: 1,
    backgroundColor: '#f1f5f9',
  },
});
```

---

## 4. Perfil de Usuario
```typescript
import React from 'react';
import { View, Text, Image, StyleSheet, ScrollView } from 'react-native';

export const ProfileScreen = () => {
  return (
    <ScrollView style={styles.pantalla} contentContainerStyle={styles.contenido}>
      <View style={styles.cabecera}>
        <Image 
          source={{ uri: 'https://images.unsplash.com/photo-1494790108377-be9c29b29330' }} 
          style={styles.avatar} 
        />
        <Text style={styles.nombre}>Mariana Vega</Text>
        <Text style={styles.usuario}>@marianavega</Text>
      </View>

      <View style={styles.seccionEstadisticas}>
        <View style={styles.cajaEstadistica}>
          <Text style={styles.valorEstadistica}>142</Text>
          <Text style={styles.labelEstadistica}>Publicaciones</Text>
        </View>
        <View style={styles.cajaEstadistica}>
          <Text style={styles.valorEstadistica}>12.5k</Text>
          <Text style={styles.labelEstadistica}>Seguidores</Text>
        </View>
        <View style={styles.cajaEstadistica}>
          <Text style={styles.valorEstadistica}>380</Text>
          <Text style={styles.labelEstadistica}>Seguidos</Text>
        </View>
      </View>
    </ScrollView>
  );
};

const styles = StyleSheet.create({
  pantalla: {
    flex: 1,
    backgroundColor: '#fff',
  },
  contenido: {
    paddingVertical: 32,
    alignItems: 'center',
  },
  cabecera: {
    alignItems: 'center',
    marginBottom: 24,
  },
  avatar: {
    width: 120,
    height: 120,
    borderRadius: 60,
    borderWidth: 4,
    borderColor: '#f1f5f9',
  },
  nombre: {
    fontSize: 22,
    fontWeight: 'bold',
    color: '#0f172a',
    marginTop: 16,
  },
  usuario: {
    fontSize: 14,
    color: '#64748b',
    marginTop: 4,
  },
  seccionEstadisticas: {
    flexDirection: 'row',
    width: '90%',
    borderTopWidth: 1,
    borderBottomWidth: 1,
    borderColor: '#e2e8f0',
    paddingVertical: 16,
    justifyContent: 'space-around',
  },
  cajaEstadistica: {
    alignItems: 'center',
  },
  valorEstadistica: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#0f172a',
  },
  labelEstadistica: {
    fontSize: 12,
    color: '#64748b',
    marginTop: 4,
  },
});
```

---

## 5. Dashboard (Tablero Operativo)
```typescript
import React from 'react';
import { View, Text, StyleSheet, useWindowDimensions } from 'react-native';

export const DashboardScreen = () => {
  const { width } = useWindowDimensions();
  const esCompacto = width < 360;

  return (
    <View style={styles.contenedor}>
      <Text style={styles.tituloSeccion}>Resumen Operativo</Text>
      
      <View style={styles.rejilla}>
        <View style={[styles.tarjeta, { width: esCompacto ? '100%' : '48%' }]}>
          <Text style={styles.valorCard}>$14,200</Text>
          <Text style={styles.labelCard}>Ingresos</Text>
        </View>

        <View style={[styles.tarjeta, { width: esCompacto ? '100%' : '48%' }]}>
          <Text style={styles.valorCard}>1,840</Text>
          <Text style={styles.labelCard}>Ventas</Text>
        </View>

        <View style={[styles.tarjeta, { width: esCompacto ? '100%' : '48%' }]}>
          <Text style={styles.valorCard}>+12.4%</Text>
          <Text style={[styles.labelCard, { color: '#10b981' }]}>Conversión</Text>
        </View>

        <View style={[styles.tarjeta, { width: esCompacto ? '100%' : '48%' }]}>
          <Text style={styles.valorCard}>98.2%</Text>
          <Text style={styles.labelCard}>SLA Soporte</Text>
        </View>
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  contenedor: {
    flex: 1,
    padding: 16,
    backgroundColor: '#f8fafc',
  },
  tituloSeccion: {
    fontSize: 20,
    fontWeight: '700',
    color: '#0f172a',
    marginBottom: 16,
  },
  rejilla: {
    flexDirection: 'row',
    flexWrap: 'wrap',
    justifyContent: 'space-between',
    gap: 12,
  },
  tarjeta: {
    backgroundColor: '#ffffff',
    padding: 16,
    borderRadius: 12,
    borderWidth: 1,
    borderColor: '#e2e8f0',
  },
  valorCard: {
    fontSize: 22,
    fontWeight: 'bold',
    color: '#1e293b',
  },
  labelCard: {
    fontSize: 12,
    color: '#64748b',
    marginTop: 4,
  },
});
```

---

## 6. Formulario con Inputs
```typescript
import React from 'react';
import { View, Text, TextInput, StyleSheet, ScrollView, Switch } from 'react-native';

export const FormularioRegistro = () => {
  const [esPremium, setEsPremium] = React.useState(false);

  return (
    <ScrollView style={styles.pantalla} contentContainerStyle={styles.formulario}>
      <Text style={styles.encabezado}>Crea tu Cuenta</Text>
      
      <View style={styles.campo}>
        <Text style={styles.label}>Nombre Completo</Text>
        <TextInput style={styles.input} placeholder="Escribe tu nombre completo" />
      </View>

      <View style={styles.campo}>
        <Text style={styles.label}>Teléfono</Text>
        <TextInput style={styles.input} keyboardType="phone-pad" placeholder="e.g. +34 600 000 000" />
      </View>

      <View style={styles.campoFila}>
        <View style={styles.textosFila}>
          <Text style={styles.labelFila}>Acceso Premium</Text>
          <Text style={styles.descFila}>Activa opciones avanzadas de la app</Text>
        </View>
        <Switch value={esPremium} onValueChange={setEsPremium} />
      </View>
    </ScrollView>
  );
};

const styles = StyleSheet.create({
  pantalla: {
    flex: 1,
    backgroundColor: '#f1f5f9',
  },
  formulario: {
    padding: 24,
  },
  encabezado: {
    fontSize: 22,
    fontWeight: 'bold',
    color: '#0f172a',
    marginBottom: 24,
  },
  campo: {
    marginBottom: 16,
  },
  label: {
    fontSize: 14,
    fontWeight: '600',
    color: '#334155',
    marginBottom: 6,
  },
  input: {
    height: 48,
    borderWidth: 1,
    borderColor: '#cbd5e1',
    borderRadius: 8,
    paddingHorizontal: 12,
    backgroundColor: '#fff',
  },
  campoFila: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    backgroundColor: '#fff',
    padding: 16,
    borderRadius: 8,
    borderWidth: 1,
    borderColor: '#e2e8f0',
    marginTop: 8,
  },
  textosFila: {
    flex: 1,
    paddingRight: 16,
  },
  labelFila: {
    fontSize: 14,
    fontWeight: '600',
    color: '#1e293b',
  },
  descFila: {
    fontSize: 12,
    color: '#64748b',
    marginTop: 2,
  },
});
```

---

## 7. Ventana Modal (Cuadro de Diálogo)
```typescript
import React from 'react';
import { View, Text, StyleSheet, Pressable, Modal } from 'react-native';

interface PropiedadesModal {
  visible: boolean;
  alCerrar: () => void;
}

export const CustomModal: React.FC<PropiedadesModal> = ({ visible, alCerrar }) => {
  return (
    <Modal
      transparent
      visible={visible}
      animationType="fade"
      onRequestClose={alCerrar}
    >
      <View style={styles.fondoGris}>
        <View style={styles.cuerpoModal}>
          <Text style={styles.titulo}>Acción Requerida</Text>
          <Text style={styles.mensaje}>
            ¿Estás seguro de que deseas eliminar este elemento? Esta acción no se puede deshacer.
          </Text>
          
          <View style={styles.botonesAccion}>
            <Pressable style={[styles.boton, styles.botonCancelar]} onPress={alCerrar}>
              <Text style={styles.textoBotonCancelar}>Cancelar</Text>
            </Pressable>
            <Pressable style={[styles.boton, styles.botonConfirmar]}>
              <Text style={styles.textoBotonConfirmar}>Confirmar</Text>
            </Pressable>
          </View>
        </View>
      </View>
    </Modal>
  );
};

const styles = StyleSheet.create({
  fondoGris: {
    flex: 1,
    backgroundColor: 'rgba(15, 23, 42, 0.6)', // Capa oscura de transparencia
    justifyContent: 'center',
    alignItems: 'center',
    padding: 24,
  },
  cuerpoModal: {
    backgroundColor: '#ffffff',
    width: '100%',
    maxWidth: 340,
    borderRadius: 16,
    padding: 24,
    alignItems: 'center',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 10 },
    shadowOpacity: 0.15,
    shadowRadius: 20,
    elevation: 10,
  },
  titulo: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#0f172a',
    marginBottom: 12,
  },
  mensaje: {
    fontSize: 14,
    color: '#64748b',
    textAlign: 'center',
    lineHeight: 20,
    marginBottom: 24,
  },
  botonesAccion: {
    flexDirection: 'row',
    width: '100%',
    gap: 12,
  },
  boton: {
    flex: 1,
    height: 44,
    borderRadius: 8,
    justifyContent: 'center',
    alignItems: 'center',
  },
  botonCancelar: {
    backgroundColor: '#f1f5f9',
  },
  botonConfirmar: {
    backgroundColor: '#ef4444',
  },
  textoBotonCancelar: {
    color: '#475569',
    fontWeight: '600',
  },
  textoBotonConfirmar: {
    color: '#ffffff',
    fontWeight: '600',
  },
});
```
