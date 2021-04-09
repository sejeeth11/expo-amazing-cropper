# expo-amazing-cropper
Image cropper for Expo made with Animated API (with rotation possibility). </br>
Based on https://github.com/ggunti/react-native-amazing-cropper package. </br>


<img src="https://i.imgur.com/c5lqfLr.png" height="200" />&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://i.imgur.com/HNHkWQ7.png" height="200" />
<br/>
<img src="https://i.imgur.com/iyhmNav.png" height="200" />&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://i.imgur.com/5AJTFHZ.png" height="200" />
<br/>

<br/>

<img src="https://i.imgur.com/1GqvB6v.png" height="200" />&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://i.imgur.com/sz1bpT9.png" height="200" />
<br/>
<img src="https://i.imgur.com/Xf3PqJH.png" height="200" />&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://i.imgur.com/Ae4YRGS.png" height="200" />

This component require on `expo-image-manipulator` libraries. </br>
You can use with Expo ImagePicker to fetch Image Properties and Expo Filesystem to localise the image.</br>

**STEPS TO INSTALL:** </br>
1.* `expo install expo-image-manipulator` </br>
2.* `yarn add *this repo link*` </br>

#### Properties
-------------
| Prop  | Type | Description |
| :------------ |:---------------:| :---------------|
| onDone | `function` | A function which accepts 1 argument `croppedImageUri`. Called when user press the 'DONE' button |
| onError | `function` | A function which accepts 1 argument `err` of type `Error`. Called when rotation or cropping fails |
| onCancel | `function` | A function without arguments. Called when user press the 'CANCEL' button |
| imageUri | `string` | The **local** uri of the image you want to crop or rotate |
| imageWidth | `number` | The width (in pixels) of the image you passed in `imageUri` |
| imageHeight | `number` | The height (in pixels) of the image you passed in `imageUri` |
| initialRotation | `number` | Number which set the default rotation of the image when cropper is initialized.</br> Default is `0` |
| footerComponent | `component` | Custom component for footer. Default is `    <DefaultFooter doneText='DONE' rotateText='ROTATE' cancelText='CANCEL' />`|
| NOT_SELECTED_AREA_OPACITY | `number` | The opacity of the area which is not selected by the cropper. Should be a value between `0` and `1`. Default is `0.5`|
| BORDER_WIDTH | `number` | The border width [(see image)](https://i.imgur.com/CMSS953.png). Default is `50`|
| COMPONENT_WIDTH | `number` | The width of the entire component. Default is `Dimensions.get('window').width`, not recommended to change.|
| COMPONENT_HEIGHT | `number` | The height of the entire component. Default is `Dimensions.get('window').height`, you should change it to fix [hidden footer issue](https://github.com/ggunti/react-native-amazing-cropper/issues/30).|


#### Usage example 1 (using the default footer)
-------------
```javascript
import React, { Component } from 'react';
import ExpoAmazingCropper from 'react-native-amazing-cropper';;

class ExpoAmazingCropperPage extends Component {
  //response from Expo Image Picker
  const imagePickerResponse = {
    "cancelled":false,
    "height":1611,
    "width":2148,
    "uri":"file:///data/user/0/host.exp.exponent/cache/my-local-image.jpg"
  }

  const onDone = (croppedImageUri) => {
    console.log('croppedImageUri = ', croppedImageUri);
    // send image to server for example
  }

  const onError = (err) => {
    console.log(err);
  }

  const onCancel = () => {
    console.log('Cancel button was pressed');
    // navigate back
  }

  render() {
    return (
      <ExpoAmazingCropper
        onDone={(uri:string)=> onDone(croppedImageUri)}
        onError={(err:any)=> onError(err)}
        onCancel={()=> onCancel()}
        imageUri= imagePickerResponse.uri
        imageWidth={imagePickerResponse.width}
        imageHeight={imagePickerResponse.height}
        NOT_SELECTED_AREA_OPACITY={0.3}
        BORDER_WIDTH={20}
      />
    );
  }
}
```

#### Usage example 2 (using the default footer with custom text)
-------------
```javascript
import React, { Component } from 'react';
import ExpoAmazingCropper, { DefaultFooter } from 'react-native-amazing-cropper';

class ExpoAmazingCropperPage extends Component {
  //response from Expo Image Picker
  const imagePickerResponse = {
    "cancelled":false,
    "height":1611,
    "width":2148,
    "uri":"file:///data/user/0/host.exp.exponent/cache/my-local-image.jpg"
  }

  const onDone = (croppedImageUri) => {
    console.log('croppedImageUri = ', croppedImageUri);
    // send image to server for example
  }

  const onError = (err) => {
    console.log(err);
  }

  const onCancel = () => {
    console.log('Cancel button was pressed');
    // navigate back
  }

  render() {
    return (
      <ExpoAmazingCropper
        // Pass custom text to the default footer
        footerComponent={<DefaultFooter doneText='OK' rotateText='ROT' cancelText='BACK' />}
        onDone={(uri:string)=> onDone(croppedImageUri)}
        onError={(err:any)=> onError(err)}
        imageUri= imagePickerResponse.uri
        imageWidth={imagePickerResponse.width}
        imageHeight={imagePickerResponse.height}
      />
    );
  }
}
```

#### Usage example 3 (using own fully customized footer)
-------------

Write your custom footer component.</br>
Don't forget to call the **props.onDone**, **props.onRotate** and **props.onCancel** methods inside it (the Cropper will pass them automatically to your footer component). Example of custom footer component:

```javascript
import React from 'react';
import { View, TouchableOpacity, Text, Platform, StyleSheet } from 'react-native';
import PropTypes from 'prop-types';
import MaterialCommunityIcon from 'react-native-vector-icons/MaterialCommunityIcons';

const CustomCropperFooter = (props) => (
  <View style={styles.buttonsContainer}>
    <TouchableOpacity onPress={props.onCancel} style={styles.touchable}>
      <Text style={styles.text}>CANCEL</Text>
    </TouchableOpacity>
    <TouchableOpacity onPress={props.onRotate} style={styles.touchable}>
      <MaterialCommunityIcon name='format-rotate-90' style={styles.rotateIcon} />
    </TouchableOpacity>
    <TouchableOpacity onPress={props.onDone} style={styles.touchable}>
      <Text style={styles.text}>DONE</Text>
    </TouchableOpacity>
  </View>
)

export default CustomCropperFooter;

CustomCropperFooter.propTypes = {
  onDone: PropTypes.func,
  onRotate: PropTypes.func,
  onCancel: PropTypes.func
}

const styles = StyleSheet.create({
  buttonsContainer: {
    flexDirection: 'row',
    alignItems: 'center', // 'flex-start'
    justifyContent: 'space-between',
    height: '100%'
  },
  text: {
    color: 'white',
    fontSize: 16
  },
  touchable: {
    padding: 10,
  },
  rotateIcon: {
    color: 'white',
    fontSize: 26,
    ...Platform.select({
      android: {
        textShadowOffset: { width: 1, height: 1 },
        textShadowColor: '#000000',
        textShadowRadius: 3,
        shadowOpacity: 0.9,
        elevation: 1
      },
      ios: {
        shadowOffset: { width: 1, height: 1 },
        shadowColor: '#000000',
        shadowRadius: 3,
        shadowOpacity: 0.9
      }
    }),
  },
})
```

Now just pass your footer component to the Cropper like here:

```javascript
import React, { Component } from 'react';
import ExpoAmazingCropper from 'react-native-amazing-cropper';
import CustomCropperFooter from './src/components/CustomCropperFooter.component';

class ExpoAmazingCropperPage extends Component {
  const onDone = (croppedImageUri) => {
    console.log('croppedImageUri = ', croppedImageUri);
    // send image to server for example
  }

  const onError = (err) => {
    console.log(err);
  }

  const onCancel = () => {
    console.log('Cancel button was pressed');
    // navigate back
  }

  render() {
    return (
      <ExpoAmazingCropper
        // Use your custom footer component
        // Do NOT pass onDone, onRotate and onCancel to the footer component, the Cropper will do it for you
        footerComponent={<CustomCropperFooter />}
        onDone={(uri:string)=> onDone(croppedImageUri)}
        onCancel={()=> onCancel()}
        imageUri= imagePickerResponse.uri
        imageWidth={imagePickerResponse.width}
        imageHeight={imagePickerResponse.height}
      />
    );
  }
}
```

#### Special thanks to: https://github.com/ggunti