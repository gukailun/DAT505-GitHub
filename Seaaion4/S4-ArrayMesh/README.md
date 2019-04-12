# DAT505-GitHub

* In this seaaion,

```javacsript
var renderer, scene, camera;
var controls
var cubes = [];
var rot = 0;

function init() {
  scene = new THREE.Scene();

  var W = window.innerWidth,
      H = window.innerHeight;

  camera = new THREE.PerspectiveCamera(45, W / H, .1, 1000);
  camera.position.set(0, 55, 85);
  camera.lookAt(scene.position);

  var spotLight = new THREE.SpotLight(0xFFFFFF);
  spotLight.position.set(1000, 0, 1000);
  scene.add(spotLight);
  //spotLight.castShadow = true;

  var ambLight = new THREE.AmbientLight(0xFFFFFF);
  ambLight.position.set(0,10000,0);
  ambLight.add(spotLight);
  scene.add(ambLight);

  renderer = new THREE.WebGLRenderer({antialias:true});
  renderer.setClearColor(0x000000);
  renderer.setSize(W, H);
  //renderer.shadowMapEnabled = true;

  controls = new THREE.OrbitControls(camera, renderer.domElement);

  //Create a two dimensional grid of objects, and position them accordingly
  for (var x = -20; x <= 5; x += 5) { // Start from -45 and sequentially add one every 5 pixels
  for (var y = -20; y <= 5; y += 5) {
  for (var z = -20; z <= 5; z += 5) {

      console.log("X:" +x+ ", Y: " +y+", Z:" +z);
      var boxGeometry = new THREE.BoxGeometry(3,3,3);
      var boxMaterial = new THREE.MeshLambertMaterial({color: 0xFFFFFF});
      if (x >= 0 && y >=0 && z >=0){
      boxMaterial = new THREE.MeshLambertMaterial({color: 0xF02A694});
    } else if ( x <= 0 && y >= 0 && z >=0){
      boxMaterial = new THREE.MeshLambertMaterial({color: 0xF8B195});
    } else if ( x >= 0 && y <= 0 && z >=0){
      boxMaterial = new THREE.MeshLambertMaterial({color: 0xC06CB4});
    } else if ( x >= 0 && y >= 0 && z <=0){
      boxMaterial = new THREE.MeshLambertMaterial({color: 0xC06CB4});
    } else if ( x <= 0 && y <= 0 && z >=0){
      boxMaterial = new THREE.MeshLambertMaterial({color: 0x355C7D});
    } else if ( x <= 0 && y >= 0 && z <=0){
      boxMaterial = new THREE.MeshLambertMaterial({color: 0xF02A694});
    } else if ( x >= 0 && y <= 0 && z <=0){
      boxMaterial = new THREE.MeshLambertMaterial({color: 0xC06CB4});
    } else {
      boxMaterial = new THREE.MeshLambertMaterial({color: 0xFFFFFF});
    }


      //The color of the material is assigned a random color
    /*  if (x >= 0 && z >=0){
      var boxMaterial = new THREE.MeshLambertMaterial({color: 0xFF00FF});
    }
      if (x >= 0 && z <=0){
      var boxMaterial = new THREE.MeshLambertMaterial({color: 0xFFFFFF});
    }
      if (x <= 0 && z >=0){
      var boxMaterial = new THREE.MeshLambertMaterial({color: 0xFF0000});
    }
      if (x >= 0 && z <=0){
      var boxMaterial = new THREE.MeshLambertMaterial({color: 0x0000FF});
      */

      var mesh = new THREE.Mesh(boxGeometry, boxMaterial);
      //mesh.castShadow = true;
      mesh.position.x = x;
      mesh.position.y = y;
      mesh.position.z = z;
    //  mesh.scale.y = 0.5;
      scene.add(mesh);
      cubes.push(mesh);
  }
  }
}
  console.log(cubes);
  document.body.appendChild(renderer.domElement);
}

function drawFrame(){
  requestAnimationFrame(drawFrame);

  rot += 0.05;
  cubes.forEach(function(c,i){
    c.rotation.x = rot;
  });
  renderer.render(scene, camera);
}

init();
drawFrame();

```

* this code is designed to generate a group of meshs on the X y axis to form a rubik's cube like meshs and then control them to rotate in the same direction.
