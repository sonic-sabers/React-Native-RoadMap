# React Native

## Road Map
       
![](/Road-Map.png)|


## Installing dependencies
- Node, Watchman installation
    
        brew install node watchman

- React Native CLI

        npm install -g react-native-cli


## Creating a New Application
- Creating new application with latest version

        react-native init demo

- With specific version

        react-native init demo --version X.XX.X
        react-native init demo --version react-native@next

- Installing developer tools

        npm install -g react-devtools

## Running Native app
- Run IOS

        react-native run-ios

- Run Android

        react-native run-android

- Cache issue Run

        npm start -- --reset-cache

## Hot Keys

- IOS

        Reaload - cmd+R
        Debug - cmd+D
- Android

        Reload - Double press R
        Debug - cmd+m

## Eslint setup

        npm i -D eslint babel-eslint eslint-plugin-react

## Devtools

        npm i -g react-devtools

## Reset commands

        rm -rf /tmp/metro-bundler-cache-*
        rm -rf node_modules && npm install
        npm start -- --reset-cache

## Generator toolbox - React Native toolbox

- https://github.com/bamlab/generator-rn-toolbox

## Material kit & vector icons

-
        npm install --save react-native-material-kit react-native-vector-icons 

- https://oblador.github.io/react-native-vector-icons/

## Linking all dependencies 
        
        react-native link

- Cource

        Building Restfull web APIs with Node.js and Express

## Native debugger

- Install debugger
        
        brew update && brew cask install react-native-debugger
- In createStore add

        window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()

## SafeAreaView

        import { SafeAreaView } from 'react-native';

        <SafeAreaView style={styles.safeArea}>
        </SafeAreaView>

        const styles = StyleSheet.create({
                safeArea: {
                        flex: 1,
                        backgroundColor: '#ddd'
                }
        }

## Navigation 

### Bottom Tab Navigation 
-
        npm i --save react-navigation
        npm i --save react-native-gesture-handler

-  Respective pages

        import Icon from 'react-native-vector-icons/EvilIcons';

        static navigationOptions = {
                tabBarIcon : ({tintColor}) => (
                <Icon name={'plus'} size={50} color={tintColor} />
                )
        }

-
        import { createBottomTabNavigator, createAppContainer } from 'react-navigation'
        import page1 from './page1';
        import page2 from './page2';
        import page3 from './page3';

        const TabNavigator = createBottomTabNavigator(
                {
                        page1: page1,
                        page2: page2,
                        page3: page3
                },
                {
                        initialRouteName: 'page1',
                        tabBarOptions: {
                                activeTintColor: 'white',
                                inactiveTintColor: '#80cbc4',
                                showLabel: false,
                                // showIcon: true,
                                // labelStyle: {
                                //     color: 'white',
                                // },
                                style:{
                                        backgroundColor: '#26a69a'
                                }
                        }
                }
        )

        export default createAppContainer(TabNavigator);
        
## Redux

- Initial State

        {
                "param1": [],
                "param2": true
        }
- Actions
        
        import C from './constants';
        export const updatePeopleData = (data) => ({
                type: C.PARAM1,
                payload: data
        })

        export function updateConfigurationApi(status){
                if(status)
                        return { type: C.SET_PARAM2 }
                else
                        return { type: C.UNSET_PARAM2 }
        }

- Constants

        const constants = {
                UPDATE_PARAM1 : "UPDATE_PARAM1",
                SET_PARAM2 : "SET_PARAM2",
                UNSET_PARAM2 : "UNSET_PARAM2"
        }

        export default constants;

- Reducer

        import C from './constants';
        import { combineReducers } from 'redux';

        export const param1 = (state = [], action) => {
                switch (action.type) {
                        case C.UPDATE_PARAM1:
                                return action.payload;
                        default:
                                return state;
                }
        }

        export const param2 = (state = false, action) => {

                switch (action.type) {

                        case C.SET_PARAM2:
                        return true

                        case C.UNSET_PARAM2:
                        return false

                        default:
                        return state
                }

        }

        export default combineReducers({
                param1,
                param2
        })

- storeProvider

        import sampleData from './initialState';

        var store = undefined;

        export default {
                init(configureStore){
                        store = configureStore(sampleData);
                },
                getStore(){
                        return store;
                },
                getCurrentState(){
                        return store.getState();
                },
                getApi(){
                        var currentState = this.getCurrentState();
                        return currentState.config.api;
                 }
        };

- index

        import appReducer from './reducers';
        import thunk from 'redux-thunk';
        import { createStore, applyMiddleware } from 'redux';

        const consoleMessages = store => next => action => {
        
                let result;
                
                /*console.groupCollapsed(`dispatching action => ${action.type}`)
        
                console.log(`

                        Data: ${JSON.stringify(action)}

                `)
                
                console.groupEnd()*/

                result = next(action);
                return result;
        }

        export default (initialState = {}) => {
                return applyMiddleware(thunk, consoleMessages)(createStore)(appReducer, initialState);
        }

- Integration

        import storeFactory from './store';
        import storeProvider from './store/storeProvider';

        storeProvider.init(storeFactory);
        const store = storeProvider.getStore();
        const saveState = () => JSON.stringify(store.getState());
        store.subscribe(saveState);

        <Provider store={store}>
        </Provider>

## Side drawer

-
        npm install --save react-native-drawer-menu

-
        https://github.com/Tinysymphony/react-native-drawer-menu

-
        openDrawer(){
                this._drawer.openDrawer();
        }

        render() {

                var drawerContent = (
                        <View style={styles.drawerContent}>
                        <View style={styles.leftTop}/>
                        <View style={styles.leftBottom}>
                                <View><Text>Drawer Content</Text></View>
                        </View>
                        </View>
                );
                // customize drawer's style (Optional)
                var customStyles = {
                        drawer: {
                                shadowColor: '#000',
                                shadowOpacity: 0.8,
                                shadowRadius: 10
                        },
                        mask: {}, // style of mask if it is enabled
                        main: {} // style of main board
                };


                return (
                        <Drawer
                                ref={(ref) => this._drawer = ref}
                                style={styles.container}
                                drawerWidth={300}
                                drawerContent={drawerContent}
                                type={Drawer.types.Overlay}
                                customStyles={{drawer: styles.drawer}}
                                drawerPosition={Drawer.positions.left}
                                onDrawerOpen={() => {console.log('Drawer is opened');}}
                                onDrawerClose={() => {console.log('Drawer is closed')}}
                                easingFunc={Easing.ease}
                                >
                                <View style={styles.content}>
                                        <Text onPress={this.openDrawer}>{Object.values(Drawer.positions).join(' ')}</Text>
                                        <Text>{Object.values(Drawer.types).join(' ')}</Text>
                                </View>
                        </Drawer>
                );
        }

## navigation drawer android fix

- Edit in MainActivity.java

        import com.facebook.react.ReactActivity;
        + import com.facebook.react.ReactActivityDelegate;
        + import com.facebook.react.ReactRootView;
        + import com.swmansion.gesturehandler.react.RNGestureHandlerEnabledRootView;

        public class MainActivity extends ReactActivity {

        @Override
                protected String getMainComponentName() {
                return "Example";
        }

        +  @Override
        +  protected ReactActivityDelegate createReactActivityDelegate() {
        +    return new ReactActivityDelegate(this, getMainComponentName()) {
        +      @Override
        +      protected ReactRootView createRootView() {
        +       return new RNGestureHandlerEnabledRootView(MainActivity.this);
        +      }
        +    };
        +  }
        }
