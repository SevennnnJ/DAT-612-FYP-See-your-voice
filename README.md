# Final Year Project -《See your voice》
# Name: JIE XU
# Student ID:10664684
# 《See your voice》
This is final work that code project for DAT612 module.

### GitHub link ###
https://github.com/SevennnnJ/DAT-612-FYP-See-your-voice.git

#### Idea's description ####
My inspiration comes from the fact that it's hard for deaf people to get music information. Hearing loss will make hearing impaired patients encounter many problems in their daily life, especially hearing loss is easy to bring them social problems, and the accumulation of these problems for a long time is easy to induce hearing impaired patients to have mental health problems. But at the same time, music information has a potential impact on people's psychological aspects. Therefore, the combination of music visualization technology and web design can make more people get music information through vision.


#### Description ####
In this web page, the homepage is set in galaxy, and in the middle is the slogan that this page wants to convey. In the lower left corner is the click button to enter the main page. The main page is set in the starry sky to create a relaxed atmosphere. There are two buttons on the main page. One is to select your own folder button. Users can click this button to get the local music file. Then a visual image of the music will appear in the middle of the page. Another is the button of the pull-down page. Users click it to pull down the page and get the music of the page itself. There is a pause button in the top left corner of the page, which can pause the music playing at any time.


#### Catalogue ####
DAT-612-FYP-See-your-voice
    assets
      img
        snowflake6.png
        snowflake6a.png
      mp3
        Ameneurosis ---Original.mp3
        Epic Cinematic ---soundbay.mp3
        Moonlit night ---LinG.mp3
        Solitude ---Original.mp3
        What Is Love ---Melanie Ungar.mp3
      a1-2.png
      a4a.png
      bank.jpg
      dr.png
      help.png
      OrbitControls.d.ts
      OrbitControls.js
      play.png
      stop.png
      three.min.js
   Final show
      1.png
      2.png
      3.png
      4.png
      5.png
      6.png
   index.html
   music.html


### Usage ###
```html
<script src="./assets/three.min.js"></script>
<script src="./assets/OrbitControls.js"></script>
```

### Code ###
This code defines the values I need. The camera, renderer, data group and audio analyzerthat I need for the basic scene.

```javascript
/* Define scene，camera，renderer，audio analyzer */
let scene, camera, renderer, analyser;
/* Defining the width and height defaults to the size of the window */
let width = window.innerWidth, height = window.innerHeight;
/* Defining data objects*/
let data;
/* Define a three group*/
let group = new THREE.Group();
let clicktag = true;

let fftSize = 128; // Number of data segments
let materials = []; // Material data

scene = new THREE.Scene(); // Scene

camera = new THREE.PerspectiveCamera(75, width / height, 1, 10000); // Camera
camera.position.z = 1000; // position of camera
camera.lookAt(0, 0, 0); // Camera observation point

renderer = new THREE.WebGLRenderer(); // Renderer
renderer.setPixelRatio(window.devicePixelRatio);
renderer.setSize(width, height);

let container = document.getElementById('container'); // Get DOM node
container.appendChild(renderer.domElement); // The rendered result is inserted into the DOM

```

In this section, I add five songs in the webpage and set startbutton behind each music and set the stopbutton in the whole web.

```javascript
/* Default six songs add one click event for each song, click play button to start playing*/
let startButton = document.getElementById('startButton1');
startButton.addEventListener('click', (e) => {
  playdefmp3("Moonlit night")
});

let startButton1 = document.getElementById('startButton2');
startButton1.addEventListener('click', (e) => {
  playdefmp3("What Is Love");
});

let startButton2 = document.getElementById('startButton3');
startButton2.addEventListener('click', (e) => {
  playdefmp3("Epic Cinematic")
});

let startButton3 = document.getElementById('startButton4');
startButton3.addEventListener('click', (e) => {
  playdefmp3("Solitude")
});

let startButton4 = document.getElementById('startButton5');
startButton4.addEventListener('click', (e) => {
  playdefmp3("Ameneurosis")
});

```

In this section, I add the cubes in the scene and make them as groups and change the size with the music.

```javascript
// let axesHelper = new THREE.AxesHelper(50); //辅助观察的坐标系 Coordinate system of auxiliary observation
// scene.add(axesHelper);

/* Define a cube define a cube*/
let geometry = new THREE.BoxGeometry(1, 40, 50);
let material = new THREE.MeshNormalMaterial();
let cube = new THREE.Mesh(geometry, material);

/* Create two cube arrays*/
let cubes = [], cubes2 = [];
/* Let the cube distribute evenly around a circle to calculate the radius of each interval angle*/
let ang = 720 / fftSize, _r = 350;
for (let i = 0; i < fftSize / 2; i++) {
  let angl = i * ang;
  let c = cube.clone();
  c.position.x = _r * Math.sin(Math.PI * angl / 180);
  c.position.y = _r * Math.cos(Math.PI * angl / 180);
  // Rotate the cube to make it conform to the tangent direction of the circle
  group.add(c);
  c.rotation.z = -Math.PI * (angl) / 180;
  cubes.push(c);
  c.visible = false;  //Set invisible by default


  let c2 = cube.clone();
  c2.position.x = (_r - 0) * Math.sin(Math.PI * angl / 180);
  c2.position.y = (_r - 0) * Math.cos(Math.PI * angl / 180);
  c2.rotation.z = -Math.PI * (angl) / 180;
  group.add(c2);
  cubes2.push(c2);
  c2.visible = false; //Set invisible by default
}


scene.add(group) // Add all cubes to the scene

```

I add particals in the scene and use these particles as the background that looks like starry sky.

```javascript
//  Add 20000 star particles
addsprite()
function addsprite() {

  var geometry = new THREE.BufferGeometry();
  var vertices = [];
  /* Material loader */
  var textureLoader = new THREE.TextureLoader();

  var sprite1 = textureLoader.load('./assets/img/snowflake6a.png');
  var sprite2 = textureLoader.load('./assets/img/snowflake6a.png');

  /* Randomly generated coordinate XYZ */
  for (var i = 0; i < 10000; i++) {
    var x = Math.random() * 2000 - 1000;
    var y = Math.random() * 4000 - 2000;
    var z = Math.random() * 2000 - 1000;
    vertices.push(x, y, z);
  }
  /* Set vertex position */
  geometry.addAttribute('position', new THREE.Float32BufferAttribute(vertices, 3));

  parameters = [
    [[1, 0.01, 0.5], sprite1, 10],
    [[0.95, 0.1, 0.5], sprite2, 15],
  ];

  for (var i = 0; i < parameters.length; i++) {

    var color = parameters[i][0];
    var sprite = parameters[i][1];
    var size = parameters[i][2];

    materials[i] = new THREE.PointsMaterial({
      size: size,
      map: sprite,
      blending: THREE.AdditiveBlending,
      depthTest: false,
      transparent: true,
      opacity: 0.6,
    });
    materials[i].color.setHSL(color[0], color[1], color[2]);

    var particles = new THREE.Points(geometry, materials[i]);

    particles.rotation.x = Math.random() * 6;
    particles.rotation.y = Math.random() * 6;
    particles.rotation.z = Math.random() * 6;
    scene.add(particles);

    // materials[i].color.setHSL(1, 0.5, 0.5);



  }
  //Make particles randomly switch colors
  let k = 0.001;
  setInterval(() => {
    materials[0].color.setHSL(Math.abs(Math.sin(k)), 0.5, 0.5);
    materials[0].color.setHSL(Math.abs(Math.sin(k+0.5)), 0.5, 0.5);
    k += 0.01;
    // Math.abs(Math.sin(k));
  }, 60);
}


```

This section add the audio monitor in the web to analyze the music in the web.

```javascript
let listener = new THREE.AudioListener();// Audio monitor
let sound = new THREE.Audio(listener);// Audio object


let audioLoader = new THREE.AudioLoader();// Sound loader
let audioContext = new AudioContext();// Audio context
/* Play the default audio in path */
function playdefmp3(src) {
  /* If the current normal playback, it will stops first */
  if (sound.isPlaying) {
    sound.stop();
  }
  /* Load a new audio file to assign the result to the audio object */
  audioLoader.load('./assets/mp3/' + src + '.mp3', function (buffer) {
    sound.setBuffer(buffer);
    sound.setLoop(true);
    sound.setVolume(1);
    sound.play();
  });
  /* Modify audio analyzer */
  analyser = new THREE.AudioAnalyser(sound, fftSize);
}

```

The purpose of this part is to add the function of selecting music file and selecting music from the drop-down page to the web page.

```javascript
/* Select File click event */
function choosefile() {
  let f = document.getElementById("file0");
  f.click();// Because it is hidden, a function call is used to simulate a manual click event
}
/* Click the drop-down page event */
function dropdown() {
  // let conlist = document.getElementById('conlist');
  // if(clicktag === true){
  // 	conlist.style.animation = "myupdown 1s forwards";
  // 	clicktag = false;
  // }else{
  // 	conlist.style.animation = "mydownup 1s forwards";
  // 	clicktag = true;
  // }

}
/* Choose file */
function selectedFile() {
  let selectedFile = document.getElementById('file0').files[0];
  // let name = selectedFile.name;// Read the file name of the selected file
  // let size = selectedFile.size;// Read the size of the selected file
  let reader = new FileReader();// the read operation is completed
  reader.readAsArrayBuffer(selectedFile);
  reader.onload = function () {
    //Call back this function after reading, and then the contents of the file are stored in the result, which can be operated directly
    playLoadmp3(this.result)
  }
}
/* Play your own loaded audio file */
function playLoadmp3(buffer) {
  if (sound.isPlaying) {
    sound.stop();
  }
  audioContext.decodeAudioData(buffer, (e) => {
    sound.setBuffer(e);
    sound.setLoop(true);
    sound.setVolume(0.5);
    sound.play();
    analyser = new THREE.AudioAnalyser(sound, fftSize);
  })
}
/* Reset size event of listening page*/
window.addEventListener('resize', onResize, false);
/* Automatically adapt to window changes */
function onResize() {
  // Reset renderer size
  renderer.setSize(window.innerWidth, window.innerHeight);
  // Reset camera angle of view
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
}

/* Loop animation */
function animate() {
  requestAnimationFrame(animate); // Loop execution function
  render();
}
```

This part renders the animation of the visualization part and controls it to follow the audio analysis.I have set up three kinds of visualization forms, which can be switched at any time during the music playing process.

```javascript

  /* Rendering functions */
  function render() {
    var time = Date.now() * 0.00005; // Current timestamp


    if (analyser != undefined) {
      /* Get new data to drive the cube's length to change according to the data, and the cube rotates continuously at the same time*/
      data = analyser.getFrequencyData();
      if (seeType === 1) {
        for (let i = 0; i < fftSize / 2; i++) {
          let s = 1 + data[i] / 8;
          let angl = i * ang;
          s > 30 ? 30 : s; // Format the zoom scale. If it is greater than 30, it is equal to 30
          cubes[i].scale.x = 1;
          cubes[i].scale.y = s;
          cubes[i].scale.z = 1;
          cubes[i].rotation.x = 0;
          cubes[i].rotation.z = 0.2 * Math.sin(time) + 0.8 * Math.cos(time + i * 0.005) + 5 * time + i * 0.005;
          cubes[i].position.x = (_r - 0) * Math.sin(Math.PI * angl / 180);
          cubes[i].position.y = (_r - 0) * Math.cos(Math.PI * angl / 180);
          // c2.rotation.z = -Math.PI * (angl) / 180;

          cubes2[fftSize / 2 - 1 - i].scale.x = 1;
          cubes2[fftSize / 2 - 1 - i].scale.y = s;
          cubes2[fftSize / 2 - 1 - i].scale.z = 1;
          cubes2[fftSize / 2 - 1 - i].position.x = (_r - 0) * Math.sin(Math.PI * angl / 180);
          cubes2[fftSize / 2 - 1 - i].position.y = (_r - 0) * Math.cos(Math.PI * angl / 180);
          cubes2[fftSize / 2 - 1 - i].rotation.x = 0;
          cubes2[fftSize / 2 - 1 - i].rotation.z = -(0.7 * Math.sin(time) + 0.3 * Math.cos(time + i * 0.005) + 5 * time + i * 0.005);
          // Hide if zoom is less than 0.2x
          if (s < 1.2) {
            cubes[i].visible = false;
            cubes2[fftSize / 2 - 1 - i].visible = false;
          } else {
            cubes[i].visible = true;
            cubes2[fftSize / 2 - 1 - i].visible = true;
          }
        }

      }
      if (seeType === 2) {
        for (let i = 0; i < fftSize / 2; i++) {
          let s = 1 + data[i] / 8;
          s > 30 ? 30 : s; // Format the zoom scale. If it is greater than 30, it is equal to 30
          cubes[i].scale.x = 1;
          cubes[i].scale.y = 0.05;
          cubes[i].scale.z = s * 0.5;
          cubes[i].position.x = i * 10;
          cubes[i].position.y = 0;

          cubes[i].rotation.x = Math.PI / 2;
          cubes2[fftSize / 2 - 1 - i].scale.x = 1;
          cubes2[fftSize / 2 - 1 - i].scale.y = 0.05;
          cubes2[fftSize / 2 - 1 - i].scale.z = s * 0.5;
          cubes2[fftSize / 2 - 1 - i].position.x = -i * 10;
          cubes2[fftSize / 2 - 1 - i].position.y = 0;
          cubes2[fftSize / 2 - 1 - i].rotation.x = Math.PI / 2;
          // Hide if zoom is less than 0.2x
          if (s < 1.2) {
            cubes[i].visible = false;
            cubes2[fftSize / 2 - 1 - i].visible = false;
          } else {
            cubes[i].visible = true;
            cubes2[fftSize / 2 - 1 - i].visible = true;
          }
        }
      }
      if (seeType === 3) {
        __t += 0.1;
        for (let i = 0; i < fftSize / 2; i++) {
          let s = 1 + data[i] / 8;
          s > 30 ? 30 : s; // Format the zoom scale. If it is greater than 30, it is equal to 30
          cubes[i].scale.x = 1;
          cubes[i].scale.y = 0.05;
          cubes[i].scale.z = s * 0.5;
          cubes[i].position.x = i * 20;
          cubes[i].position.y = 100 * Math.sin(__t + i * 0.1);
          cubes[i].rotation.x = Math.PI / 2;

          cubes2[fftSize / 2 - 1 - i].scale.x = 1;
          cubes2[fftSize / 2 - 1 - i].scale.y = 0.05;
          cubes2[fftSize / 2 - 1 - i].scale.z = s * 0.5;
          cubes2[fftSize / 2 - 1 - i].position.x = -i * 20;
          cubes2[fftSize / 2 - 1 - i].position.y = 100 * Math.sin(__t + i * 0.1);
          cubes2[fftSize / 2 - 1 - i].rotation.x = Math.PI / 2;
          // Hide if zoom is less than 0.2x
          if (s < 1.2) {
            cubes[i].visible = false;
            cubes2[fftSize / 2 - 1 - i].visible = false;
          } else {
            cubes[i].visible = true;
            cubes2[fftSize / 2 - 1 - i].visible = true;
          }
        }
      }

    }

```

This part is to render the whole scene.

```javascript
/* Render Scene  */
renderer.render(scene, camera);
/* Let the particles spin */
for (var i = 0; i < scene.children.length; i++) {
  var object = scene.children[i];
  if (object instanceof THREE.Points) {
    object.rotation.y = 0.1 * time * (i < 4 ? i + 1 : - (i + 1));
  }
}
/* Group rotation */
group.rotation.z = time;
/* The camera moves up, down, left and right */
camera.position.x = 600 * Math.sin(time * 0.1);
camera.position.y = 600 * Math.cos(time * 0.01);
camera.lookAt(0, 0, 0)

}

```
