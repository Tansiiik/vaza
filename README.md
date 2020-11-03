
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>three.js webgl - геометрические фигуры</title>
		<meta charset="utf-8">
		<meta name="viewport" content="width=device-width, user-scalable=no, minimum-scale=1.0, maximum-scale=1.0">
		<link type="text/css" rel="stylesheet" href="https://threejs.org/examples/main.css">
	</head>
	<body>
		<div id="info">
			Трехмерные фигуры
		</div>

		<script type="module">

			import * as THREE from 'https://threejs.org/build/three.module.js';

			import { OrbitControls } from 'https://threejs.org/examples/jsm/controls/OrbitControls.js';

			var camera, scene, renderer;
			var controls;
			var ambientLight, light;
			init();
			animate();

			function init() {

				var container = document.createElement( 'div' );
				document.body.appendChild( container );

				// CAMERA
				camera = new THREE.PerspectiveCamera( 20, window.innerWidth / window.innerHeight, 1, 80000 );
				camera.position.set( 150, 20, 400 );

				// LIGHTS
				ambientLight = new THREE.AmbientLight( 0x333333 );	// 0.2

				light = new THREE.DirectionalLight( 0x0000FF, 1.0 );
				light.position.set( 1, 1, 1 );				
				// direction is set in GUI

				// RENDERER
				renderer = new THREE.WebGLRenderer( { antialias: true } );
				renderer.setPixelRatio( window.devicePixelRatio );
				renderer.setSize( window.innerWidth, window.innerHeight );
				container.appendChild( renderer.domElement );

				// EVENTS
				window.addEventListener( 'resize', onWindowResize, false );

				// CONTROLS
				controls = new OrbitControls( camera, renderer.domElement );
				controls.addEventListener( 'change', render );
				//controls.rotateSpeed = 1; 
				controls.enableZoom = true;  
				controls.zoomSpeed = 0.5;  

				controls.minDistance = 50;
				controls.maxDistance = 2500;
				
				controls.enableDamping = true;

				// scene itself
				scene = new THREE.Scene();
				scene.background = new THREE.Color( 0x87CEFA );

				scene.add( ambientLight );
				scene.add( light );
					
				var material = new THREE.MeshPhongMaterial( { 
              color: 0xDAA520, specular: 0x00B2FC, shininess: 3,
              side: THREE.DoubleSide
				} );
						
				var points = [ ];
				for ( var i = - 2.4; i < 0.7; i = i + 0.1 ) {
							points.push( new THREE.Vector2( 40 + 20*Math.exp( -i*i ), -20*i ) );
				}


				var geometry = new THREE.LatheGeometry( points, 32 );
					var object = new THREE.Mesh( geometry, material );
					scene.add( object );

			}

			// EVENT HANDLERS


			function onWindowResize() {

				camera.aspect = window.innerWidth / window.innerHeight;
				camera.updateProjectionMatrix();

				renderer.setSize( window.innerWidth, window.innerHeight );

			}

			//

			function animate() {

				requestAnimationFrame( animate );
				controls.update(); //
				render();

			}

			function render() {

				renderer.render( scene, camera );

			}			


		</script>

	</body>
</html>
