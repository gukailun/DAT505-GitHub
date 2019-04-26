### KailunGu-DAT505-Assignment ###
NuaStudent = B161006065
Name = gukailun
#### Description ####
I create BoxGeometry and add cat texture. Then I add 2 eyes on cat that cat will look at the mouse. I also set up to play the sound "mew" when the mouse touches the cat.

#### Links ####
[github]()

#### Usage ####
```html
<script src="build/three.min.js"></script>
<script src="js/libs/stats.min.js"></script>
```

* Create camera, scene, renderer, mesh, image, mouseX, mouseY, eyes and sound.

```javascript
var camera, scene, renderer, mesh;
var image;
var mouseX = 0, mouseY = 0;
var container;

var eyesNum = 2;
var eyes = [];
var xPos = [];
var yPos = [];
var xPosMap = [];
var yPosMap = [];

var windowHalfX = window.innerWidth / 2;
var windowHalfY = window.innerHeight / 2;
var listener = new THREE.AudioListener();
var sound = new THREE.Audio( listener );
var audioLoader = new THREE.AudioLoader();
```

* Create geometries to be the eyes and add to the cat.

```javascript
init();
animate();

function init() {
	container = document.createElement( 'div' );
	document.body.appendChild( container );

	scene = new THREE.Scene();

	camera = new THREE.PerspectiveCamera( 40, window.innerWidth / window.innerHeight, 1, 1000 );
	camera.position.set( 0, 0, 150 );
  scene.add( camera ); // since light is child of camera

	scene.add( new THREE.AmbientLight( 0xffffff, 0.2 ) );
	var light = new THREE.PointLight( 0xffffff, 1 );
	camera.add( light );

	var geometry = new THREE.SphereGeometry( 30, 32, 16 );
	var geometry1 = new THREE.BoxGeometry( 50,50,3.5 );
	var texture = new THREE.TextureLoader().load('images/texture1.jpg');

	var material = new THREE.MeshPhongMaterial( {
		color: 0xffffff,
		specular: 0x050505,
		shininess: 50,
		map: THREE.ImageUtils.loadTexture('images/eye.png'),
	});
	var material1 = new THREE.MeshBasicMaterial( { map: texture } );
	mesh1 = new THREE.Mesh( geometry1, material1 );
	mesh1.position.set( 0, 0, 0 );
	scene.add( mesh1 );

	var faceVertexUvs = geometry.faceVertexUvs[ 0 ];
	for ( i = 0; i < faceVertexUvs.length; i ++ ) {
		var uvs = faceVertexUvs[ i ];
		var face = geometry.faces[ i ];
		for ( var j = 0; j < 3; j ++ ) {
			uvs[ j ].x = face.vertexNormals[ j ].x * 0.5 + 0.5;
			uvs[ j ].y = face.vertexNormals[ j ].y * 0.5 + 0.5;
		}
	}
	for (var i = 0; i < eyesNum; i++) {
		mesh = new THREE.Mesh( geometry, material );

		xPos [0] = -9;
		yPos [0] = -3;

		xPos [1] = 9;
		yPos [1] = -3;

		xPosMap[i] = map_range(xPos[i], -50, 50, 0, window.innerWidth);
		yPosMap[i] = map_range(yPos[i], -50, 50, 0, window.innerHeight);

		mesh.position.x = xPos[i];
		mesh.position.y = yPos[i];

		var randSize = 0.1;
		mesh.scale.x = randSize;
		mesh.scale.y = randSize;
		mesh.scale.z = randSize;

		scene.add( mesh );
		eyes.push( mesh );
	}

	console.log(mesh);
	renderer = new THREE.WebGLRenderer();
	renderer.setPixelRatio( window.devicePixelRatio );
	renderer.setSize( window.innerWidth, window.innerHeight );
	container.appendChild( renderer.domElement );

	document.addEventListener( 'mousemove', onDocumentMouseMove, false );
	window.addEventListener( 'resize', onWindowResize, false );
}
```

* Set the rotation range of eyes and the range of mouseX and mouseY which used to play sound.

```javascript
function animate() {
	requestAnimationFrame( animate );
	render();
}

function render() {
	console.log(mouseY)
		eyes[0].rotation.x = map_range(mouseY, 0, window.innerWidth, -1.14, 1.14);
		eyes[1].rotation.x = map_range(mouseY, 0, window.innerWidth, -1.14, 1.14);
	for (var i = 0; i < eyesNum; i++) {

		if (mouseX<400) eyes[0].rotation.y = map_range(mouseX, 0, 400, -0.75, 0);
		else eyes[0].rotation.y = map_range(mouseX, 400, window.innerWidth, 0, 0.75);
		if (mouseX<550) eyes[1].rotation.y = map_range(mouseX, 0, 550, -0.75, 0);
		else eyes[1].rotation.y = map_range(mouseX, 550, window.innerWidth, 0, 0.75);

  }

	if ( mouseX > 300 && mouseX < 700 && mouseY > 200 && mouseY < 500) {
		audioLoader.load( 'audio/10822.wav', function( buffer ) {
		sound.setBuffer( buffer );
		sound.setLoop( false );
		sound.setVolume( 0.5 );
		sound.play();
		});
	} else {}


	renderer.render( scene, camera );
}

function onWindowResize() {
  windowHalfX = window.innerWidth / 2;
  windowHalfY = window.innerHeight / 2;
  camera.aspect = window.innerWidth / window.innerHeight;
	camera.updateProjectionMatrix();
  renderer.setSize( window.innerWidth, window.innerHeight );
}

function onDocumentMouseMove( event ) {
	mouseX = event.clientX;
  mouseY = event.clientY;
}

function map_range(value, low1, high1, low2, high2) {
	return low2 + (high2 - low2) * (value - low1) / (high1 - low1);
}
```
