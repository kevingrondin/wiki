---
sidebar_position: 1
---

# 🚢 Navigation

> Première étape pour la creation du Wireframe consiste à représenter les différente vue, navigation, bloc représentant image, logo, input, bouton

## Installation

Trés peu de dépendances sont nécessaire pour la création du wireframe

  * Tailwind qui va nous permettre de faire nos bloc sans CSS

```shell title="Installation Tailwind"
npm i twrnc --save
```

## Navigation vers les principaux ecrans

```md title="Structure du projet"
  ... // autre fichier et dossier
  App.tsx // point d'entrée au projet
  src
      ... // autre dossier
      navigation
          navigation.tsx // définition des routes
      screen
          connexionScreen.tsx 
          menuScreen.tsx      
          splashScreen.tsx
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

```tsx title="src/navigation/navigation.tsx"
import * as React from 'react';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

import SplashScreen from '../screen/splashScreen';
import ConnexionScreen from '../screen/connexionScreen';
import MenuScreen from '../screen/menuScreen';

export type StackParams = {
    SplashScreen: undefined;
    Connexion: undefined;
    Menu: undefined;
}

const Stack = createNativeStackNavigator<StackParams>();

const Navigation: React.FC = () => {
    return (
        <Stack.Navigator initialRouteName='SplashScreen' screenOptions={{ headerShown: false }}>
            <Stack.Screen name="SplashScreen" component={SplashScreen} />
            <Stack.Screen name="Connexion" component={ConnexionScreen} />
            <Stack.Screen name="Menu" component={MenuScreen} />
        </Stack.Navigator>
    );
};

export default Navigation
```

```tsx title="src/screen/splashScreen.tsx"
import React from 'react';
import { BackHandler, Text, SafeAreaView } from 'react-native';
import { NativeStackScreenProps } from '@react-navigation/native-stack';
import { useIsFocused } from "@react-navigation/native";
import tw from 'twrnc'

import { StackParams } from '../navigation/navigation';

type Props = NativeStackScreenProps<StackParams, 'SplashScreen'>;

const SplashScreen: React.FC<Props> = ({navigation}) => {
    const isFocused = useIsFocused();

    React.useEffect(() => {
        BackHandler.addEventListener("hardwareBackPress", () => true);
        return () => BackHandler.removeEventListener("hardwareBackPress", () => true)
    }, []);

    React.useEffect(() => {
        if (isFocused) {
            setTimeout(() => {
                navigation.navigate('Connexion')
            }, 2000);
        }
    }, [isFocused])

    return (
        <SafeAreaView style={tw`flex h-full justify-center items-center`}>
            <Text style={tw`text-4xl text-black`}>SplashScreen</Text>
        </SafeAreaView>
    )
}

export default SplashScreen
```

```tsx title="src/screen/connexionScreen.tsx"
import React from 'react';
import { SafeAreaView, Text, ToastAndroid, TouchableOpacity, Vibration, View } from 'react-native';
import { NativeStackScreenProps } from '@react-navigation/native-stack';
import tw from 'twrnc'

import { StackParams } from '../navigation/navigation';

type Props = NativeStackScreenProps<StackParams, 'Connexion'>;

const ConnexionScreen: React.FC<Props> = ({navigation}) => {

    const showToast = () =>
        ToastAndroid.show("Connexion réussi", ToastAndroid.SHORT);

    const vibrate = () => Vibration.vibrate(70);

    return (
        <SafeAreaView style={tw`flex h-full items-center`}>
            <View style={tw`mt-10 justify-center items-center w-60 h-18 bg-gray-400`}>
                <Text style={tw`text-white`}>Titre</Text>
            </View>
            <View style={tw`mt-10 justify-center items-center w-80 h-12 bg-gray-400`}>
                <Text style={tw`text-white`}>Connexion</Text>
            </View>
            <TouchableOpacity onPress={() => {
                vibrate();
                showToast();
                navigation.navigate('Menu');
            }}>
                <View style={tw`mt-10 justify-center items-center w-80 h-18 bg-gray-400`}>
                    <Text style={tw`text-white`}>Input</Text>
                </View>
            </TouchableOpacity>
        </SafeAreaView>
    )
}

export default ConnexionScreen
```

```tsx title="src/screen/menuScreen.tsx"
import React from 'react';
import { BackHandler, SafeAreaView, Text, TouchableOpacity, View } from 'react-native';
import { NativeStackScreenProps } from '@react-navigation/native-stack';
import tw from 'twrnc'

import { StackParams } from '../navigation/navigation';

type Props = NativeStackScreenProps<StackParams, 'Menu'>;

const MenuScreen: React.FC<Props> = ({navigation}) => {

    React.useEffect(() => {
        BackHandler.addEventListener("hardwareBackPress", () => true);
        return () => BackHandler.removeEventListener("hardwareBackPress", () => true)
    }, []);

    return (
        <SafeAreaView style={tw`flex h-full items-center`}>
            <View style={tw`mt-10 flex flex-row w-full`}>
                <View style={tw`w-9/12 flex justify-center items-center`}>
                    <View style={tw`flex justify-center items-center h-20 pl-20 bg-gray-400`}>
                        <Text style={tw`text-white`}>Titre</Text>
                    </View>
                </View>
                <View style={tw`w-3/12 flex justify-center items-center pr-5`}>
                    <TouchableOpacity onPress={() => navigation.navigate('SplashScreen')}>
                        <View style={tw`text-white h-14 w-14 bg-gray-400 rounded-full flex justify-center items-center`}>
                            <Text style={tw`text-white`}>Quit</Text>
                        </View>
                    </TouchableOpacity>
                </View>
            </View>
            <View style={tw`mt-10 flex flex-row justify-around items-center w-full`}>    
                <View style={tw`flex justify-center items-center w-1/2`}>
                    <View style={tw`flex justify-center items-center w-38 h-20 bg-gray-400`}>
                        <Text style={tw`text-white`}>Menu</Text>
                    </View>
                </View>
                <View style={tw`flex justify-center items-center w-1/2`}>
                    <View style={tw`flex justify-center items-center w-38 h-20 bg-gray-400`}>
                        <Text style={tw`text-white`}>Menu</Text>
                    </View>
                </View>
            </View>
            <View style={tw`mt-5 flex flex-row justify-around items-center w-full`}>    
                <View style={tw`flex justify-center items-center w-1/2`}>
                    <View style={tw`flex justify-center items-center w-38 h-20 bg-gray-400`}>
                        <Text style={tw`text-white`}>Menu</Text>
                    </View>
                </View>
                <View style={tw`flex justify-center items-center w-1/2`}>
                    <View style={tw`flex justify-center items-center w-38 h-20 bg-gray-400`}>
                        <Text style={tw`text-white`}>Menu</Text>
                    </View>
                </View>
            </View>
            <View style={tw`mt-5 flex flex-row justify-around items-center w-full`}>    
                <View style={tw`flex justify-center items-center w-1/2`}>
                    <View style={tw`flex justify-center items-center w-38 h-20 bg-gray-400`}>
                        <Text style={tw`text-white`}>Menu</Text>
                    </View>
                </View>
                <View style={tw`flex justify-center items-center w-1/2`}>
                    <View style={tw`flex justify-center items-center w-38 h-20 bg-gray-400`}>
                        <Text style={tw`text-white`}>Menu</Text>
                    </View>
                </View>
            </View>
        </SafeAreaView>
    )
}

export default MenuScreen
```