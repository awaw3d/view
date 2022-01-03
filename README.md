# DeviantArt VR Viewer

This VR viewer supports a network of 360 scenes. A "scene" is a 360 image plus some number of buttons that allow navigation to other scenes.

This allows exploration of many images, immersively, inside a VR context. You could use this to show:

* A series of images, like a slideshow, that progresses when the user clicks a button
* Exploration of a visual environment from different camera positions, by moving from one perspective to another
* Playing through an interactive narrative via player choices

A simple configuration might look like:

    {
        "FIRST": {
            "url": "https://deviantart.com/someURL",
            "buttons": [
                [-45, 0, 10, "downward"],
                [45, -45, 10, "upward"]
            ]
        },
        "downward": {
            "url": "https://deviantart.com/someOtherURL",
            "buttons": [
                [45, 180, 10, "FIRST"]
            ]
        },
        "upward": {
            "url": "https://deviantart.com/stillSomeOtherURL",
            "buttons": [
                [-45, 180, 10, "FIRST"]
            ]
        }
     }
     
This data is an object in [JSON](https://www.json.org/json-en.html) format. The keys of the object are names of scenes: `FIRST`, `downward`, `upward`. The scene name `FIRST` is used to designate one scene as the starting scene.

The `url` property indicates where each scene should get its 360 image. This URL may be a Sta.sh URL, a DeviantArt URL, or an image (which must be served with CORS headers). All scenes' images must match in how they present stereo images (top-bottom, left-right, or mono), and that needs to be specified in the options.

The `buttons` property has a list of buttons. Each button is a list, with each item being:

1. Y rotation - in the sphere of the scene, this rotates the button up or down to some lattitude. The value is **rotation in degrees** and can be negative to down or positive to go up.
2. X rotation - after rotating to the specified latitude, next rotate the button around the circle of that lattitude band. Also rotation in degrees, positive/negative for left/right.
3. Depth - how far to move the button away from the viewer. Get too close appears to cause parallax/stereoscopic problems; a distance of `10` works well enough.
4. Name of next scene - when this button is clicked, natigate to the named scene.

The button `[45, -45, 10, "upward"]` says

    45 degrees up (halfway between the equator and the north pole),
    then 45 degrees to the right along that lattitude,
    project outward 10 distance units, and
    go to the scene named "upward" when clicked
    
A helpful tool for validating your JSON syntax is https://jsonlint.com/
