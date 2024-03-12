# Raycast System

**Raycast System** is a set of a few classes which help setup basic structure for detecting special interest objects in 3D world, mainly for purposes of some kind of interaction system. 

In Raycast System there are two types of objects differing in behavior and purpose:
1) **Raycasted objects** are objects which are detected in Raycast System. They might contain logic to necessary for the interaction, but they shouldn't call it on their own. Their main purpose is to be detected by raycaster and then used by its controller logic
2) **Raycaster** the subject of the system. It detects the raycasted objects in order to call some logic on them

## Configuration
Configuration of the game to support and use Raycast System consists of 2 parts: (1) raycasted objects configuration and (2) raycaster configuration.

### Objects configuration
1. **Layer** - preparing a special layer for detected objects is not mandatory and raycasted objects might be on _Default_ layer as well. However it is a good practice to create a separate layer for purposes of being detected as raycasted or interactive objects.
2. **Raycast Collider** - every raycasted object needs a collider to be detected. After setting a desired layer (chosen in point 1) on colliders game object, component [RaycastCollider](https://github.com/Kosmik123/Raycast-System/blob/master/Scripts/RaycastCollider.cs) must be added. Raycast collider represents one collider of the object, as the object might use multiple colliders.
3. **Raycast Target** - another component which must be added to raycasted object. The [RaycastTarget](https://github.com/Kosmik123/Raycast-System/blob/master/Scripts/RaycastTarget.cs) component represents the raycasted object and it's logic. It should be added to game object where all other logic related scripts are to fully benefit from TryGetComponent method. Preferably it should be added to ancestor of Raycast Colliders. Raycast target can be inherited from, for example to contain cached logic component, to make referencing it easier instead of using TryGetComponent

### Raycaster (subject) configuration
1. **Raycast Controller** - every raycasting subject in Raycast System (for example Player) needs [RaycastController](https://github.com/Kosmik123/Raycast-System/blob/master/Scripts/RaycastController.cs) component in order to detect raycast targets. The component might be added to any game object of the subject however it's prefered to put it in _Head_ game object of FPP player or _MainCamera_ game object for other perspectives. If raycast controller is needed for other objects such as NPCs, it's also possible to add it to them.
2. **Ray Provider** - a strategy for providing a ray to raycast controller. There are a few predefined ray providers for the most common raycast controller usages, but [RayProvider](https://github.com/Kosmik123/Raycast-System/blob/master/Scripts/RayProvider.cs) class can be inherited and adjusted to personal needs of the project. So, far there are 3 predefined ray providers:
    *  [MainCameraForwardRayProvider](https://github.com/Kosmik123/Raycast-System/blob/master/Scripts/Ray%20Providers/MainCameraForwardRayProvider.cs) - returns ray from MainCamera in the MainCameras direction
    *  [TransformForwardRayProvider](https://github.com/Kosmik123/Raycast-System/blob/master/Scripts/Ray%20Providers/TransformForwardRayProvider.cs) - returns forward looking ray from game object its attached to
    *  [RayFromMouseProvider](https://github.com/Kosmik123/Raycast-System/blob/master/Scripts/RayFromMouseProvider.cs) - returns ray going from camera (by default MainCamera) in the direction specified by mouse pointer position. There are both [Input Manager](https://github.com/Kosmik123/Raycast-System/blob/master/Scripts/Ray%20Providers/InputManagerRayFromMouseProvider.cs) and [Input System](https://github.com/Kosmik123/Raycast-System/blob/master/Scripts/Ray%20Providers/InputSystemRayFromMouseProvider.cs) variants
