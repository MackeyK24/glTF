# CVTOOLS_unity_metadata 

## Contributors

* Mackey Kinard, Canvas Tools

## Status

Draft

## Dependencies

Written against the glTF 2.0 spec.

## Overview

This extension adds serialization of Unity game object metadeta for each project, scene and node. The extension also allows encoding external asset file references. Any implementation of this extension can use the **extra metadata** and **file references** to create any components and scripts. As well as load and manage any external asset files. An example scene `node` with asset file details.

```json
"scene" {
  "extras": {
    "metadata": {
      "files": [
        {
          "bufferView": 12,
          "mimeType": "audio/wav",
          "fileName": "music.wav"
        },
        {
          "uri": "skybox.dds"
        }
      ]
    }
  }
}
```

An example `node` with **extra metadata** that details the **Sphere Collider**, **RigidBody** and **DemoRotator Script** components attached to the Unity game object. The implementation can then create a **physics state** for the `entity` using the exported metadata during the parse.

```json
"nodes": [
    {
      "mesh": 2,
      "scale": [
        1.25,
        1.25,
        1.25
      ],
      "name": "Sphere",
      "extras": {
        "metadata": {
          "alias": "entity",
          "id": "127cca04-7e95-4c82-b63f-b3dd911fb199",
          "tag": "Untagged",
          "name": "Sphere",
          "prefab": false,
          "layer": 0,
          "layername": "Default",
          "components": [
            {
              "alias": "script",
              "klass": "DemoRotator",
              "helloWorld": "Hello World"
            },
            {
              "alias": "spherecollider",
              "material": {
                "dynamicFriction": 0.6,
                "staticFriction": 0.6,
                "bounciness": 0.0,
                "bouncyness": 0.0,
                "frictionDirection2": {
                  "x": 0.0,
                  "y": 0.0,
                  "z": 0.0
                },
                "dynamicFriction2": 0.0,
                "staticFriction2": 0.0,
                "frictionCombine": 0,
                "bounceCombine": 0,
                "frictionDirection": {
                  "x": 0.0,
                  "y": 0.0,
                  "z": 0.0
                },
                "name": "Tester",
                "hideFlags": 0
              },
              "istrigger": false,
              "radius": 0.5,
              "center": {
                "x": 0.0,
                "y": 0.0,
                "z": 0.0
              },
              "x": 0.0,
              "y": 0.0,
              "z": 0.0
            },
            {
              "alias": "rigidbody",
              "mass": 1.0,
              "drag": 0.0,
              "angulardrag": 0.05,
              "usegravity": true,
              "iskinematic": false,
              "interpolate": {
                "Key": 0,
                "Value": "None"
              },
              "collisiondetection": {
                "Key": 0,
                "Value": "Discrete"
              },
              "staticfriction": 0.6,
              "dynamicfriction": 0.6,
              "restitution": 0.0
            }
          ]
        }
      }
    }
]
```

**Unity Editor Components**

* UnityEngine.Light
* UnityEngine.Camera
* UnityEngine.Animator
* UnityEngine.Renderers
* UnityEngine.Colliders
* UnityEngine.Rigidbody
* UnityEngine.AudioSource
* UnityEngine.AudioListener
* UnityEngine.ParticleSystem
* UnityEngine.SpriteRenderer

**Custom Script Components**

The exporter supports custom scripts components. Any `MonoBehaviour` subclass of the **EditorScriptComponent** gets serialized with its classname and any public serializable properties exposed to the editor script component.

```csharp
using System;
using System.IO;

using UnityEngine;
using UnityEditor;

namespace MyProject
{
	public class DemoRotator : EditorScriptComponent
	{
		public string helloWorld = "Hello World";

		public override void OnExportProperties(Transform transform, ref CanvasTools.GLTFMetaDataExporter exporter)
		{
			// Update Complex Export Properties
		}
	}
}
```

An example [PlayCanvas](https://www.playcanvas.com) **backing script class**.

```typescript
/**
 * PlayCanvas Backing Script Class
 * @class DemoRotator
 */
@createScript()
class DemoRotator extends CanvasScript implements ScriptType {
    public initialize():void {
        console.log("Starting demo rotator for: " + this.entity.getName());
    }
    public update(delta: number) {
        let speed = 20 * delta;
        this.entity.rotate(0, speed, 0);
    }
}
```

An example [BabylonJS](https://www.babylonjs.com) **backing script class**.

```typescript
/**
 * BabyonJS Backing Script Class
 * @class DemoRotator
 */
class DemoRotator extends BABYLON.CanvasScript {
    public start():void {
        console.log("Starting demo rotator for: " + this.entity.name);
    }
    public update() {
        let speed = 20 * this.manager.deltaTime;
        this.entity.rotation.y += speed;
    }
}
```

An example [ThreeJS](https://threejs.org) **backing script class**.

```typescript
/**
 * ThreeJS Backing Script Class
 * @class DemoRotator
 */
class DemoRotator extends THREE.CanvasScript {
    public start():void {
        console.log("Starting demo rotator for: " + this.entity.name);
    }
    public update() {
        let speed = 20 * this.manager.deltaTime;
        this.entity.rotation.y += speed;
    }
}
```

## glTF Schema Updates

None

## Known Implementations

This extension is used by the WebGL [Canvas Tools](https://github.com/MackeyK24/CanvasTools) exporter plugin for Unity to export **extra** project, scene and node **metadata**.
