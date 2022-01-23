# React Native

## Road Map
       
![](/Road-Map.png)|

Checkout this for more info : https://github.com/jondot/awesome-react-native

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


## ✈️ Getting started

### Prerequisites

1.  [Git](https://git-scm.com/downloads).
1.  [Node](https://nodejs.org/en/download/) _(version 12 or greater)_.
1.  [Yarn](https://yarnpkg.com/lang/en/docs/install/) _(version 1.5 or greater)_.
1.  A fork of the repo _(for any contributions)_.
1.  A clone of the `react-native-website` repo.

### Installation

1.  `cd react-native-website` to go into the project root.
1.  Run `yarn` to install the website's workspace dependencies.

### Running locally

1.  `cd website` to go into the website portion of the project.
1.  `yarn start` to start the development server _(powered by [Docusaurus](https://v2.docusaurus.io))_.
1.  `open http://localhost:3000/` to open the site in your favorite browser.

## 📖 Overview

If you would like to **_contribute an edit or addition to the docs,_** read through our [style guide](STYLEGUIDE.md) before you write anything. All our content is generated from markdown files you can find in the `docs` directory.

**_To edit the internals of how the site is built,_** you may want to get familiarized with how the site is built. The React Native website is a static site generated using [Docusaurus](https://v2.docusaurus.io). The website configuration can be found in the `website` directory. Visit the Docusaurus website to learn more about all the available configuration options.

### Directory Structure

The following is a high-level overview of relevant files and folders.

```
react-native-website/
├── docs/
│   ├── accessibility.md
│   └── ...
└── website/
    ├── blog/
    │   ├── 2015-03-26-react-native-bringing-modern-web-techniques-to-mobile.md
    │   └── ...
    ├── core/
    ├── pages/
    │   └── en/
    ├── src/
    │   ├── css/
    │   │   ├── customTheme.scss
    │   │   └── ...
    │   ├── pages/
    │   │   ├── index.js
    │   │   └── ...
    │   └── theme/
    ├── static/
    │   ├── blog/
    │   │   └── assets/
    │   ├── docs/
    │   │   └── assets/
    │   ├── img/
    │   └── js/
    ├── versioned_docs/
    │   ├── version-0.60/
    │   └── ...
    ├── versioned_sidebars/
    │   ├── version-0.60-sidebars.json
    │   └── ...
    ├── docusaurus.config.js
    ├── package.json
    ├── showcase.json
    ├── sidebars.json
    └── versions.json
```


####GIT BASICS

### Documentation sources

As mentioned above, the `docs` folder contains the source files for all of the docs in the React Native website. In most cases, you will want to edit the files within this directory. If you're adding a new doc or you need to alter the order the docs appear in the sidebar, take a look at the `sidebars.json` file in the `website` directory. The sidebars file contains a list of document ids that should match those defined in the header metadata (aka frontmatter) of the docs markdown files.

### Versioned docs

The React Native website is versioned to allow users to go back and see the API reference docs for any given release. A new version of the website is generally generated whenever there is a new React Native release. When this happens, any changes made to the `docs` and `website/sidebars.json` files will be copied over to the corresponding location within `website/versioned_docs` and `website/versioned_sidebars`.


#### Cutting a new version

1.  `cd react-native-website` to go into the project root.
1.  `cd website` to go into the website portion of the project.
1.  Run `yarn version:cut <newVersion>` where `<newVersion>` is the new version being released.

## 🔧 Website configuration

The `core` subdirectory contains JavaScript and React components that are the core part of the website.

The `src/pages` subdirectory contains the React components that make up the non-documentation pages of the site, such as the homepage.

The `src/theme` subdirectory contains the swizzled React components from the Docusaurus theme.

The `showcase.json` file contains the list of users that are highlighted in the React Native showcase.

## 👏 Contributing

### Create a branch

1.  `git checkout main` from any folder in your local `react-native-website` repository.
1.  `git pull origin main` to ensure you have the latest main code.
1.  `git checkout -b the-name-of-my-branch` to create a branch.
    > replace `the-name-of-my-branch` with a suitable name, such as `update-animations-page`

### Make the change

1.  Follow the "[Running locally](#running-locally)" instructions.
1.  Save the files and check in the browser.
1.  Some changes may require a server restart to generate new files. (Pages in `docs` always do!)
1.  Edits to pages in `docs` will only be visible in the latest version of the documentation, called "Next", located under the `docs/next` path.

Visit **http://localhost:3000/docs/next/YOUR-DOCS-PAGE** to see your work.

> Visit http://localhost:3000/versions to see the list of all versions of the docs.

### Test the change

If possible, test any visual changes in all latest versions of the following browsers:

- Chrome and Firefox on the desktop.
- Chrome and Safari on mobile.

### Push it

1.  Run `yarn prettier` and `yarn language:lint` in `./website` directory to ensure your changes are consistent with other files in the repo.
1.  `git add -A && git commit -m "My message"` to stage and commit your changes.
    > replace `My message` with a commit message, such as `Fixed header logo on Android`
1.  `git push my-fork-name the-name-of-my-branch`
1.  Go to the [react-native-website repo](https://github.com/facebook/react-native-website) and you should see recently pushed branches.
1.  Follow GitHub's instructions.
1.  Describe briefly your changes (in case of visual changes, please include screenshots).
