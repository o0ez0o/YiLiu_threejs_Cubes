			var container, stats;
			var camera, scene, raycaster, renderer;

			var mouse = new THREE.Vector2(), INTERSECTED;
			var radius = 100, theta = 0;

			//--------------
			var loader;
			var audioListener, soundFilter, soundAreaAnalyser, soundOutsideAnalyser;
			var soundArea, collisionArea, lightArea, lightOutside;
			//--------------

			init();
			animate();
			setBGM();

	      	function setBGM() {
	        // Create an AudioListener and add it to the camera
	        var listener = new THREE.AudioListener();
	        camera.add( listener );

	        // Create a global audio source
	        var sound = new THREE.Audio( listener );

	        var audioLoader = new THREE.AudioLoader();

	        // Load a sound and set it as the Audio object's buffer
	        audioLoader.load( 'sounds/background.wav', function( buffer ) {
	          sound.setBuffer( buffer );
	          sound.setLoop( true );
	          sound.setVolume( 0.5 );
	          sound.play();
	        });
	      }


			function init() {

				container = document.createElement( 'div' );
				document.body.appendChild( container );

				camera = new THREE.PerspectiveCamera( 70, window.innerWidth / window.innerHeight, 1, 7000 );

				scene = new THREE.Scene();
				scene.background = new THREE.Color( 0x080808 );
				// scene.fog = new THREE.Fog( 0x59472b, 1000, FAR );


				var light = new THREE.PointLight( 0xE6E6E6, 1 );
				light.position.set( 10, 10, 10 ).normalize();
				scene.add( light );
//

				// var light2 = new THREE.PointLight( 0xfefefe, 1 );
				// light2.position.set( -14000, 0, 0 ).normalize();
				// scene.add( light2 );

				var light3 = new THREE.PointLight( 0xfefefe, 1 );
				light3.position.set( 0, -14000, 0 ).normalize();
				scene.add( light3 );

				// var light4 = new THREE.PointLight( 0xfefefe, 1 );
				// light4.position.set( 0, 0, -14000 ).normalize();
				// scene.add( light4 );

					var light5 = new THREE.PointLight( 0xfefefe, 1 );
				light5.position.set( -3000, -5000, -3000 ).normalize();
				// light2.angle = Math.PI / 4;
				scene.add( light5 );

				var geometry = new THREE.BoxBufferGeometry( 20, 20, 20 );

				for ( var i = 0; i < 500; i ++ ) {

					var object = new THREE.Mesh( geometry, new THREE.MeshLambertMaterial( { color: 0x3D0000 } ) );

					object.position.x = Math.random() * 900 - 500;
					object.position.y = Math.random() * 900 - 500;
					object.position.z = Math.random() * 900 - 500;

					object.rotation.x = Math.random() * 0.5 * Math.PI;
					object.rotation.y = Math.random() * 0.5 * Math.PI;
					object.rotation.z = Math.random() * 0.5 * Math.PI;

					object.scale.x = 1.3;
					object.scale.y = 1.3;
					object.scale.z = 1.3;

					scene.add( object );


				}

				var geometry2 = new THREE.CubeGeometry(20, 20, 20);

					// var material2 = new THREE.MeshNormalMaterial({shading: THREE.FlatShading});
					// var mesh2 = new THREE.Mesh(geometry2, material2);
					var mesh2 = new THREE.Mesh( geometry2, new THREE.MeshLambertMaterial( { color: 0xff0000 } ) );


					mesh2.position.x = 300;
					mesh2.position.y = 100;
					mesh2.position.z = 100;

					mesh2.rotation.x = Date.now() * 0.0005;	
					mesh2.rotation.y = Date.now() * 0.0002;	
					mesh2.rotation.z = Date.now() * 0.001;

					mesh2.scale.x = 1.5;
					mesh2.scale.y = 1.5;
					mesh2.scale.z = 1.5;
					
					scene.add(mesh2);



				var geometry3 = new THREE.CubeGeometry(20, 20, 20);	

					var mesh3 = new THREE.Mesh( geometry3, new THREE.MeshLambertMaterial( { color: 0xD61900 } ) );

					mesh3.position.x = 7;
					mesh3.position.y = 5;
					mesh3.position.z = 10;

					mesh3.rotation.x = 3.88;
					mesh3.rotation.y = 6.26;

					mesh3.scale.x = 2;
					mesh3.scale.y = 2;
					mesh3.scale.z = 2;
					
					scene.add(mesh3);

		


				var geometry4 = new THREE.CubeGeometry(20, 20, 20);	

					var mesh4 = new THREE.Mesh( geometry4, new THREE.MeshLambertMaterial( { color: 0xFF2626 } ) );

					mesh4.position.x = 150;
					mesh4.position.y = 200;
					mesh4.position.z = 500;

					mesh4.rotation.x = 3.88;
					mesh4.rotation.y = 6.26;

					mesh4.scale.x = 5;
					mesh4.scale.y = 5;
					mesh4.scale.z = 5;
					
					scene.add(mesh4);


				var geometry5 = new THREE.CubeGeometry(20, 20, 20);	

					var mesh5 = new THREE.Mesh( geometry5, new THREE.MeshLambertMaterial( { color: 0xCC1414 } ) );

					mesh5.position.x = 70;
					mesh3.position.y = -158;
					mesh3.position.z = -100;

					mesh5.rotation.x = 70;
					mesh5.rotation.y = -20;
					mesh5.rotation.y = -200;


					mesh5.scale.x = 0.5;
					mesh5.scale.y = 0.5;
					mesh5.scale.z = 0.5;
					
					scene.add(mesh5);

				


				raycaster = new THREE.Raycaster();

				renderer = new THREE.WebGLRenderer();
				renderer.setPixelRatio( window.devicePixelRatio );
				renderer.setSize( window.innerWidth, window.innerHeight );
				container.appendChild(renderer.domElement);

				stats = new Stats();
				container.appendChild( stats.dom );

				document.addEventListener( 'mousemove', onDocumentMouseMove, false );

				//

				window.addEventListener( 'resize', onWindowResize, false );

			}



			function onWindowResize() {

				camera.aspect = window.innerWidth / window.innerHeight;
				camera.updateProjectionMatrix();

				renderer.setSize( window.innerWidth, window.innerHeight );

			}

			function onDocumentMouseMove( event ) {

				event.preventDefault();

				mouse.x = ( event.clientX / window.innerWidth ) * 2 - 1;
				mouse.y = - ( event.clientY / window.innerHeight ) * 2 + 1;

			}


			//render

			function animate() {

				requestAnimationFrame( animate );

				render();
				stats.update();

			}
		      // Returns a random integer in [min, max)
		      function random(min, max) {
		        return Math.floor(Math.random() * (max - min)) + min;
		      }

		      function renderAudio(camera, obj) {
		        // Create an AudioListener and add it to the camera
		        var listener = new THREE.AudioListener();
		        camera.add( listener );

		        // Create the PositionalAudio object (passing in the listener)
		        var sound = new THREE.PositionalAudio( listener );

		        // Load a sound and set it as the PositionalAudio object's buffer
		        var audioLoader = new THREE.AudioLoader();
		        audioLoader.load( 'sounds/bell/bell ' + random(1, 10 + 1) + '.wav', function( buffer ) {
		          sound.setBuffer( buffer );
		          sound.setRefDistance( 20 );
		          sound.play();
		        });

		        // Finally add the sound to the object
		        obj.add( sound );
		      }

			function render() {

				theta += 0.03;

				camera.position.x = radius * Math.sin( THREE.Math.degToRad( theta ) );
				camera.position.y = radius * Math.sin( THREE.Math.degToRad( theta ) );
				camera.position.z = radius * Math.cos( THREE.Math.degToRad( theta ) );
				camera.lookAt( scene.position );

				camera.updateMatrixWorld();

				// find intersections

				raycaster.setFromCamera( mouse, camera );

				var intersects = raycaster.intersectObjects( scene.children );

				if ( intersects.length > 0 ) {

					if ( INTERSECTED != intersects[ 0 ].object ) {

						if ( INTERSECTED ) INTERSECTED.material.emissive.setHex( INTERSECTED.currentHex );

						INTERSECTED = intersects[ 0 ].object;
						INTERSECTED.currentHex = INTERSECTED.material.emissive.getHex();
						INTERSECTED.material.emissive.setHex( 0xffff66 );
						renderAudio(camera, INTERSECTED);

							// if ( INTERSECTED ) INTERSECTED.material.emissive.setHex( INTERSECTED.currentHex );

							// INTERSECTED = intersects[ 0 ].mesh2;
							// INTERSECTED.currentHex = INTERSECTED.material.emissive.getHex();
							// INTERSECTED.material.emissive.setHex( 0xffff66 );
					}

				} else {

					if ( INTERSECTED ) INTERSECTED.material.emissive.setHex( INTERSECTED.currentHex );

					INTERSECTED = null;

				}

				renderer.render( scene, camera );

				
			}
