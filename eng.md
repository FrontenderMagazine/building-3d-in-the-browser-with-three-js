# Building 3D in the browser with Three.js

We have capabilities in our browsers that we wouldn’t have dreamt of 5 or 10
years ago. Entire 3D game worlds can run in the browser and interactive websites
can be more immersive experiences.

With a certain virtual reality company being purchased by a certain social
media company, now is the perfect time to start working with 3D. Amazingly, we 
can now create 3D with pure JavaScript, including meshes and materials.

Of course, even though it’s possible, it does require a huge amount of code,
and that’s where Three.js comes in, allowing us to build 3D environments simply 
and with less code.

## Browser compatibility

Unfortunately, because it’s new, 3D isn’t supported by all browsers. We’re 
currently restricted to Chrome, Safari and Firefox.

As time goes by, support on IE will improve, but for now you’ll need a
fallback for Microsoft devotees.

## Getting started

The first thing we need to do is head over to the [three.js website][1] and
download the latest version of the library.

Next, reference it in the head of your document like you would any other
JavaScript file, and we’re ready to go.

## Creating our first scene

The first thing we need to do, to render anything with three.js is to create a
scene, a camera, and a renderer. Starting with a scene:

	var scene = new THREE.Scene();

Next, we need a camera. Think of this as the point of view that the user is
looking from. Three.js actually has a lot of options here, but for simplicity’s 
sake we’re going to use a perspective camera:

	var camera = new THREE.PerspectiveCamera(100, window.innerWidth / window.innerHeight, 0.1, 1000);

This method takes four parameters: the first is the field of view, the second
is the aspect ratio (I’d recommend always basing this on the user’s viewport), 
the near clipping plane, and lastly the far clipping plane. These last two 
parameters determine the limits of rendering, so that objects that are too close
or too far away aren’t drawn, which would waste resources.

Next up, we need to set up the WebGL Renderer:

	var renderer = new THREE.WebGLRenderer(); 
	renderer.setSize( window.innerWidth, window.innerHeight ); 
	document.body.appendChild( renderer.domElement );

As you can see, the first thing here is to create an instance of the renderer,
then set its size to the user’s viewport, finally we add it to the page to 
create a blank canvas for us to work with.

## Adding some objects

So far, all we’ve done is set up the stage, now we’ve going to create our
first 3D object.

For this tutorial it’s going to be a simple cylinder:

	var geometry = new THREE.CylinderGeometry( 5, 5, 20, 32 );

This method also takes 4 parameters: the first is the radius of the top of the
cylinder, the second is the radius of the bottom of the cylinder, the third is 
the height, the last is the number of height segments.

We’ve set up the mathematics of the object, now we need to wrap it in a
material so that it actually looks like something on screen:

	var material = new THREE.MeshBasicMaterial( { color: 0x0000ff, wireframe: true} );

This code adds a simple material for the object, in this case a blue color. I’ve 
set *wireframe* to *true* because it will show the object more clearly once
we get to animating it.

Finally, we need to add our cylinder to our scene, like so:

	var cylinder = new THREE.Mesh( geometry, material ); 
	scene.add( cylinder );

The last thing to do before we render the scene is to set the camera position:

	camera.position.z = 20;

As you can see, the JavaScript involved is extremely simple, that’s because
Three.js is dealing with all the complicated stuff, so we don’t have to.

## Rendering the scene

If you test the file in a browser now you’ll see that nothing is happening.
That’s because we haven’t told the scene to render. To render the scene we need 
the following code:

	function render() {
		requestAnimationFrame(render);
		renderer.render(scene, camera);
	}
	render();

If you now take a look at the file in your browser you’ll see it does indeed
render a cylinder, but it’s not very exciting.

To really enhance the value of 3D you need to start animating, which we can do
with a couple of small changes to our *render* function:

	function render() {
		requestAnimationFrame(render);
		cylinder.rotation.z += 0.01;
		cylinder.rotation.y += 0.1;
		renderer.render(scene, camera);
	}
	render();

If you test in your browser now you’ll see a properly animating 3D cylinder.

## Conclusion

If you want to see a demo and play around with the code you can 
[do so here.][2]

As you can see, creating this (admittedly very simple) scene took less than two
dozen lines of code, and there’s very little math involved.

If you check out the [examples][3] on the three.js website you’ll see some
incredible work that has been done.

This amazing library for JavaScript has really lowered the entry-level for
coding 3D to the point that anyone who can write basic JavaScript can get 
involved.

[1]: http://threejs.org/
[2]: http://codepen.io/SaraVieira/pen/Ctnov
[3]: http://threejs.org/examples/
