---
sidebar_position: 2
---

# 🚢 React Navigation V6

> La navigation entre les différente vue ce fait avec

* React Navigation

## Installation

```shell title="Installation React Navigation"
npm i @react-navigation/native --save
```

```shell
npm i react-native-screens react-native-safe-area-context --save
```

### Android

Dans le fichier `MainActivity.java` on rajoute une méthode à la Classe

```java title="MainActivity.java"
@Override
protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(null);
}
```

toujours dans le fichier `MainActivity.java` on rajoute une librairie 

```java title="MainActivity.java"
import android.os.Bundle;
```

### Type de Navigation

il existe trois type de navigation

* **Stack** (Navigation simple)
* Tab (Navigation avec une tabBar en bas)
* Drawer (Navigation avec un menu latéral)

Dans ce projet uniquement **Stack** est utilisé

```shell
npm i @react-navigation/native-stack
```

:::tip 🎊 Installation react-navigation terminé 🎊

Tout les installations de base sont faite

:::

## Implémentation

```md title="Structure du projet"
  ... // autre fichier et dossier
  App.tsx // point d'entrée au projet
  src
      ... // autre dossier
      navigation
          index.tsx // définition des routes
      screen
          screenA.tsx       // screen A
          screenB.tsx       // screen B
          screenC.tsx       // screen C
          detailScreen.tsx  // ecran ou on affiche les détails grace au donnée passé en paramètre
          index.tsx // contient tout les screens (A B et C)
```

```jsx title="App.tsx"
// On rajoute l'import de NavigationContainer et on Encapsule notre balise navigation
import { NavigationContainer } from '@react-navigation/native';
import Navigation from './src/navigation';

const App = () => {

  return (
    <NavigationContainer>
      <Navigation />
    </NavigationContainer>
  );
};

export default App;
```

```tsx title="src/navigation/index.tsx"
import * as React from 'react';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

import screen from '../screen';

export type StackParams = {
    ScreenA: undefined;
    ScreenB: undefined;
    ScreenC: undefined;
    DetailScreen: 
        | {
            id: string;
        }
        | undefined;
};

const Stack = createNativeStackNavigator<StackParams>();

const Navigation: React.FC = () => {
    return (
        <Stack.Navigator initialRouteName='ScreenA' screenOptions={{ headerShown: false }}>
            <Stack.Screen name="ScreenA" component={screen.ScreenA} />
            <Stack.Screen name="ScreenB" component={screen.ScreenB} />
            <Stack.Screen name="ScreenC" component={screen.ScreenC} />
            <Stack.Screen name="DetailScreen" component={screen.DetailScreen} />
        </Stack.Navigator>
    );
};

export default Navigation
```

```tsx title="src/screen/detailScreen.tsx"
import { NativeStackScreenProps } from '@react-navigation/native-stack';

import { StackParams } from '../navigation';

type Props = NativeStackScreenProps<StackParams, 'DetailScreen'>;

const DetailScreen: React.FC<Props> = (props) => {
   const id = props.route.params.id;
};

export default DetailScreen
```

