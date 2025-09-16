---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: /assets/intro.jpg
# some information about your slides (markdown enabled)
title: Software Development | Foundations
info: |
  ## Software Development | Foundations
# apply unocss classes to the current slide
class: text-left
drawings:
  persist: false
transition: slide-left
mdc: true
---

# React Native
Mobile Development: Unit 08 - Lesson 06

- [ ] Install a UI framework
- [ ] Continue playing with expo APIs: media, speech, sensors etc.
- [ ] Guest Speaker: Dion Tu

<div class="abs-br m-6 text-xl">
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
-->

---
transition: slide-left
---

# Recap

- Deployment to stores
1. Get a developer account:
   - Apple: $99 per year
   - Google: one time fee $25
   - business account: must provide proof
   - individual account: if you charge for your app, your personal address will be made public
- Recommendation: if you charge for your app (or even in-app purchases) use business account so you can use a PO box for your address
- Difference: Must pay $25 to Google when ready to deploy to stores vs Must pay $99 to Apple just to generate certificates to run on physical device

---
transition: slide-left
---

# Video Recording
Record and play videos  

- `npx expo install expo-video`
- see https://docs.expo.dev/versions/latest/sdk/video/

```tsx
import { useEvent } from 'expo';
import { useVideoPlayer, VideoView } from 'expo-video';

const videoSource =
  'https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4';

export default function VideoScreen() {
  const player = useVideoPlayer(videoSource, player => {
    player.loop = true;
    player.play();
  });

  const { isPlaying } = useEvent(player, 'playingChange', { isPlaying: player.playing });
...
<VideoView style={styles.video} player={player} allowsFullscreen allowsPictureInPicture />
   ...
   onPress={() => { isPlaying ? player.pause() : player.play() }}
```


---
transition: slide-left
---

# Media Library Access

- `npx expo install expo-media-library`
- see https://docs.expo.dev/versions/latest/sdk/media-library/
```tsx
import * as MediaLibrary from 'expo-media-library';

export default function App() {
   const [permissionResponse, requestPermission] = MediaLibrary.usePermissions();
...
  async function getAlbums() {
    if (permissionResponse.status !== 'granted') {
      await requestPermission();
    }
    const fetchedAlbums = await MediaLibrary.getAlbumsAsync({
      includeSmartAlbums: true,
    });
    setAlbums(fetchedAlbums);
  }
```

---
transition: slide-left
---

# Speech Recognition & Text-to-Speech

- `npx expo install expo-speech`
- see https://docs.expo.dev/versions/v52.0.0/sdk/speech/

```tsx
import * as Speech from 'expo-speech';

export default function App() {
  const speak = () => {
    const thingToSay = '1';
    Speech.speak(thingToSay);
  };

  return (
    <View style={styles.container}>
      <Button title="Press to hear some words" onPress={speak} />
    </View>
  );
}
```

---
transition: slide-left
---

# Sensors 

- `npx expo install expo-sensors`
- See [docs](https://docs.expo.dev/versions/latest/sdk/sensors/) for available sensors:
   - Accelerometer
   - Barometer
   - Device Motion
   - Gyroscope
   - Magnetometer

---
transition: slide-left
---

# Geolocation

- `npx expo install expo-location`
- see https://docs.expo.dev/versions/latest/sdk/location/

```tsx
import * as Location from 'expo-location';

export default function App() {
  const [location, setLocation] = useState<Location.LocationObject | null>(null);

  useEffect(() => {
    async function getCurrentLocation() {
      
      let { status } = await Location.requestForegroundPermissionsAsync();
      if (status !== 'granted') {
        setErrorMsg('Permission to access location was denied');
        return;
      }

      let location = await Location.getCurrentPositionAsync({});
      setLocation(location);
    }

    getCurrentLocation();
  }, []);
```
---
transition: slide-left
---

# Network Info
Provides access to the device's network such as its IP address, MAC address, and airplane mode status

- `npx expo install expo-network`
- see https://docs.expo.dev/versions/v52.0.0/sdk/network/

## NetInfo
allows you to get information about connection type and connection quality.

- `npx expo install @react-native-community/netinfo`
- see https://docs.expo.dev/versions/v52.0.0/sdk/netinfo/

---
transition: slide-left
---

# Maps

- `npx expo install react-native-maps`
- see https://docs.expo.dev/versions/latest/sdk/map-view/
```tsx
import MapView from 'react-native-maps';

export default function App() {
  return (
    <View style={styles.container}>
      <MapView style={styles.map} />
    </View>
  );
}
```

- [Register](https://console.developers.google.com/apis) a Google Cloud API project and enable the Maps SDK


---
transition: slide-left
---

# 3rd party packages

- [react-native-game-engine](https://www.npmjs.com/package/react-native-game-engine)
   - Gives you a basic game engine to build simple 2D games.
   - Create a Flappy Bird or Pong clone
- [react-native-vision-camera](https://www.npmjs.com/package/react-native-vision-camera)
   - Access the device camera with superpowers (faster, more extensible).
   - Make a fun camera filter app like Snapchat lenses
- [react-native-gesture-handler](react-native-gesture-handler)
   - Adds gesture capabilities like swipe, pinch, drag.
- [flash-list](https://www.npmjs.com/package/@shopify/flash-list)
   - high-performance lists (superior to FlatList for large data)
- [lottie-react-native](https://www.npmjs.com/package/lottie-react-native)
   - parses Adobe After Effects animations exported as JSON and renders them natively

---
transition: slide-left
---

# Confetti
react-native-confetti-cannon

- `npx expo install react-native-confetti-cannon`
- https://www.npmjs.com/package/react-native-confetti-cannon

```tsx
import { ... Dimensions } from "react-native";
import { ... useRef } from "react";
import ConfettiCannon from "react-native-confetti-cannon";

export default function CounterScreen() {
   const confettiRef = useRef<any>();
...
confettiRef?.current?.start(); // run this perhaps in an onPress handler somewhere
...
<ConfettiCannon
  ref={confettiRef}
  count={50}
  origin={{ x: Dimensions.get("window").width / 2, y: -30 }}
  autoStart={false}
  fadeOut={true}
/>
```

---
transition: slide-left
---

# Install a UI framework for React-Native

- Create a new React-Native project but try installing and playing with either:
   - https://reactnativepaper.com/
   - https://reactnativeelements.com/


---
layout: image-right
transition: slide-left
image: /assets/dan.png
backgroundSize: 444px 300px
class: text-left
---

# 10 minute break

🍦 Cool Tips, Trends and Resources:
- 🪏 [Why we ditched Next.js](https://northflank.com/blog/why-we-ditched-next-js-and-never-looked-back)
- ⚡ [content-visibility: auto](https://cekrem.github.io/posts/content-visibility-auto-performance/)
- 🎭 [React for Two Computers](https://www.youtube.com/watch?v=ozI4V_29fj4)
- ⚛️ [JSX Over the Wire = RSC](https://overreacted.io/jsx-over-the-wire/)
- 🍎 [new MAC setup](https://www.swyx.io/new-mac-setup)


<br>
<hr>
<br>

- 🧪 [Enter anonymous lab questions](https://docs.google.com/forms/d/e/1FAIpQLSevvGARdHQikso-uLqFCO481MABKE5HofuSrlzEPMNQ2ZLykw/viewform?usp=dialog)
- ℹ️ [Course feedback survey](https://circuitstream.typeform.com/to/ZoyYk7px#course_id=SoftwareAN&instructor=9514)


---
transition: slide-left
---

# Guest Speaker: Dion Tu

- When Dion arrives around 8:15pm EST, please turn on your cameras
- check out his work experience: https://www.linkedin.com/in/diontu/
- He has graciously given us 20 to 30 minutes of Q&A
- Ask him anything 



---
transition: slide-left
---

# Homework

- Start working on your Mobile Assignment 
   - Other Project Ideas besides note-app:
      - Maze Game: Use accelerometer to move a ball around a maze
      - Compass app: Use magnetometer to show real-time compass direction
      - Step Counter: Approximate steps based on accelerometer spikes
      - Build a voice memo app
      - Make a "soundboard" for sound effects or Build a record & playback feature
      - Create a photo booth
- Start working on your Capstone planning project