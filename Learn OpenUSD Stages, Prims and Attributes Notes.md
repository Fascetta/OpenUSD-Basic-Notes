# Learn OpenUSD: Stages, Prims and Attributes Notes

# 1. Introduction to OpenUSD

## 1.1. Overview of OpenUSD

**OpenUSD (Universal Scene Description)** is an open-source framework developed by **Pixar** to **efficiently create, manage, and exchange 3D scenes** across different applications and workflows. It serves as a **scalable and extensible scene description system** that enables collaboration between artists, engineers, and technical directors.

At its core, OpenUSD provides:

- A **hierarchical scene representation** (Stage and Prims).
- A **non-destructive workflow** with **layering and composition**.
- **Interoperability** across different software (Maya, Houdini, Blender, Unreal Engine, etc.).
- A **powerful rendering pipeline** through **Hydra**.
- A **high-performance file format** optimized for **large-scale production pipeline**

### Timeline

- **2000s: In-House Development at Pixar**
Pixar first built a prototype system to handle the increasingly complex 3D data for feature films.
- **Early 2010s: Core Foundation**
USD’s foundational concepts (layering, referencing, overrides) began evolving inside Pixar to enable large teams to work on the same shots without conflicts.
- **2016: Open Source Release**
Pixar released USD under an open-source license, making its robust framework accessible to the entire 3D community. This milestone catalyzed the technology’s rapid adoption across VFX, gaming, and tech industries.
- **2018–2020: Industry Adoption Accelerates**
Companies like **Autodesk**, **Apple**, **NVIDIA**, and **Unity** began incorporating USD into their products. Meanwhile, the 3D ecosystem continued to build connectors, plugins, and official support for the format.
- **2021 and Beyond: Broad Ecosystem Growth**
USD became a recognized industry standard for cross-application workflows. The ecosystem expanded to include real-time rendering, game engines, AR/VR platforms, and next-generation 3D pipelines.

---

## 1.2. Why is OpenUSD Important?

As 3D workflows grow in complexity, **OpenUSD solves many critical challenges** in content creation, visualization, and collaboration:

- **Standardization & Interoperability**
    1. OpenUSD acts as a **universal standard** for 3D assets, ensuring that content **moves seamlessly between different DCC (Digital Content Creation) tools**.
    2. Supported by major industry leaders like **Pixar, NVIDIA, Apple, Autodesk, and Unity**.
- **Scalability for Large Productions**
    1. Used extensively in **film, VFX, animation, gaming, and industrial design**.
    2. Enables teams to work on **massive environments and complex scenes** without performance bottlenecks.
- **Layering & Non-Destructive Editing**
    1. Artists and engineers can **stack multiple scene layers** (e.g., animation, shading, lighting) **without modifying the original data**.
    2. This **non-destructive workflow** makes it easy to experiment and iterate.
- **Efficient Data Management**
    1. OpenUSD uses an **optimized binary format (USDC)** to handle **large assets efficiently**.
    2. Built-in **lazy loading** ensures that only the necessary parts of a scene are loaded into memory.
- **Hydra Rendering for High-Performance Visualization**
    1. OpenUSD includes **Hydra**, a high-performance rendering framework that enables **real-time visualization** of USD scenes across different renderers.
    2. Supports **multiple render engines** like **Pixar’s RenderMan, NVIDIA Omniverse, Arnold, and Unreal Engine**.

### Use Cases and Success Stories

OpenUSD is widely adopted in industries where complex 3D scenes, collaboration, and performance are paramount. Major film studios like **Pixar** and **Industrial Light & Magic** rely on USD to coordinate large teams of artists and technical directors on blockbuster VFX and animated films. **NVIDIA** integrates USD in its Omniverse platform to enable real-time collaboration across remote teams, letting multiple artists or engineers work on the same scene simultaneously. Other adopters include **Apple**, incorporating USDZ into iOS for easy AR content creation, and **Autodesk**, providing USD workflows in Maya to streamline 3D pipelines. This cross-industry usage illustrates USD’s strength in standardizing 3D data exchange and enabling fast iteration on huge projects.

### Key Features & Benefits

| **Feature** | **Description** | **Benefit** |
| --- | --- | --- |
| **Hierarchical Scene Structure** | Uses a **Stage & Prims** model to organize 3D data. | Scalable, modular, and structured content creation. |
| **Layering & Composition** | Supports **multiple layers** for non-destructive edits. | Enables iterative workflows and collaboration. |
| **Efficient Data Storage** | Uses **USDC (compressed binary format)** for large scenes. | Reduces file size, improves loading speed. |
| **Interoperability** | Works with **Maya, Houdini, Blender, Unreal, NVIDIA Omniverse**, etc. | Seamless content exchange between tools. |
| **Real-Time Rendering (Hydra)** | Supports multiple **real-time & offline renderers**. | Enables **fast previews** and production-quality rendering. |
| **Collaboration-Friendly** | Supports **referencing & overrides** in USD files. | Multiple artists/teams can work on the same scene without conflicts. |
| **Scalability** | Handles **large-scale environments & complex assets**. | Ideal for **VFX, animation, gaming, industrial design**. |

---

# **2. Understanding OpenUSD Stages**

## **2.1. Defining a Stage**

A **stage** in OpenUSD is the **container for a 3D scene**. It defines a **hierarchical structure** that organizes objects, relationships, and properties into a structured graph known as the **scenegraph**.

### **Key Characteristics of a Stage**

- **Holds all scene elements** (geometry, lights, cameras, materials, etc.).
- **Supports composition** (merging multiple USD files into a single scene).
- **Allows layering** (non-destructive modifications via overrides).
- **Efficiently loads data** (lazy loading, references).

---

## **2.2. The Role of the Scenegraph**

The **scenegraph** is a **hierarchical structure** that defines how objects (primitives, or **prims**) are organized within a stage.

### **Scenegraph Structure**

- **Root Level** – The **stage** itself (`/`).
- **Parent-Child Relationships** – Objects are **nested within groups**.
- **Ordered Transformations** – Transformations are **inherited** by child objects.
- **Metadata & Properties** – Each object stores **attributes, materials, and relationships**.

### **Example: Scenegraph Representation**

```swift
/World                   # Root Stage
 ├── /Environment        # Scope (Organizational Element)
 │    ├── /TreeA         # Geometry
 │    ├── /TreeB         # Geometry
 │    ├── /Sky           # Background Element
 ├── /Character          # Main Animated Character
 │    ├── /Body          # Mesh
 │    ├── /Eyes          # Sub-Mesh
 │    ├── /Materials     # Assigned Materials
 ├── /Lights             # Scene Lighting
 │    ├── /SunLight      # Distant Light Source
 │    ├── /AreaLight     # Soft Rectangular Light
```

---

## **2.3. Composition: Assembling Multiple USD Files**

Composition in OpenUSD allows **multiple USD files** to be combined into a **single scene**.
This is done using **references, layers, and overrides**.

### **Composition Techniques**

| **Method** | **Description** | **Use Case** |
| --- | --- | --- |
| **References** | Pulls in external USD files as part of the scene. | Large-scale environments with modular assets. |
| **Payloads** | Similar to references, but can be loaded/unloaded dynamically. | Optimizing scene memory usage. |
| **Overrides** | Modifies attributes without changing the original file. | Non-destructive editing (animation, materials, lighting). |
| **Variants** | Allows different versions of an asset within the same file. | Character outfits, environment states. |

### **Example: Adding a Reference to a USD File**

```python
from pxr import Usd

# Create a new USD stage
stage = Usd.Stage.CreateNew("main_scene.usda")

# Define a root prim
root_prim = stage.DefinePrim("/World")

# Add a reference to an external USD file (e.g., a tree model)
root_prim.GetReferences().AddReference("tree_model.usda")

# Save the stage
stage.GetRootLayer().Save()
```

---

## **2.4. Data Aggregation & Non-Destructive Editing**

A key feature of OpenUSD is its **non-destructive workflow**, where **data from multiple sources is layered and composed dynamically**.

### **How Data Aggregation Works**

- **Each USD file can be loaded independently** → Promotes **modularity**.
- **Edits can be stored in separate layers** → Allows **overriding** without modifying original files.
- **Composition ensures scalability** → Scenes with **millions of objects** can be managed efficiently.

### **Example: Overriding an Attribute Without Changing the Source**

```python
from pxr import Usd, UsdGeom

# Open an existing stage
stage = Usd.Stage.Open("main_scene.usda")

# Get a reference to an object (e.g., a tree)
tree = stage.GetPrimAtPath("/World/TreeA")

# Override its scale (without modifying the original file)
UsdGeom.XformCommonAPI(tree).SetScale((2.0, 2.0, 2.0))

# Save the modifications
stage.GetRootLayer().Save()
```

---

## **2.5. Basic USD Stage Operations (Python Examples)**

Here are some essential operations when working with OpenUSD stages.

- **Creating a New USD Stage**
    
    ```python
    from pxr import Usd
    
    # Create a new USD stage (saved as a USDA file)
    stage = Usd.Stage.CreateNew("scene.usda")
    
    # Save the stage
    stage.GetRootLayer().Save()
    ```
    
- **Opening an Existing USD Stage**
    
    ```python
    stage = Usd.Stage.Open("existing_scene.usda")
    print("Loaded stage:", stage.GetRootLayer().identifier)
    ```
    
- **Defining a New Prim**
    
    ```python
    from pxr import UsdGeom
    
    # Define a new cube in the stage
    cube = UsdGeom.Cube.Define(stage, "/World/Cube")
    
    # Save the stage
    stage.GetRootLayer().Save()
    ```
    
- **Listing All Prims in a Scene**
    
    ```python
    for prim in stage.Traverse():
        print("Prim:", prim.GetPath())
    ```
    
- **Saving All Layers in a Stage**
    
    ```python
    stage.GetRootLayer().Save()
    ```
    

---

# 3. USD File Formats

## **3.1. Overview of USD Formats**

Universal Scene Description (**USD**) supports multiple file formats, each designed for different purposes such as **efficiency, readability, and interchangeability**. These formats allow users to store and manipulate **scene data, materials, transformations, and metadata** efficiently.

USD provides **four primary formats**:

1. **USD** – The standard scene description format.
2. **USDA** – A **human-readable** text-based format.
3. **USDC** – A **binary, optimized** format for performance.
4. **USDZ** – A **compressed archive** format for **AR applications**.

Each format offers unique advantages depending on the **workflow requirements**.

### **Comparison Table**

| **Format** | **Description** | **Pros** | **Cons** | **Best Use Case** |
| --- | --- | --- | --- | --- |
| **USD** | General-purpose format for storing scene descriptions. | Supports both **human-readable (USDA)** and **binary (USDC)** storage. | Not always the most optimized for large-scale scenes. | Standard interchange and general storage. |
| **USDA** | ASCII (text-based) format. | **Easily editable**, human-readable, good for debugging. | **Larger file size**, slower than binary formats. | Small scenes, debugging, collaboration. |
| **USDC** | Binary, highly compressed format. | **Efficient storage**, **fast loading**, optimized for **large scenes**. | **Not human-readable**, harder to debug. | Large-scale productions, high-performance rendering. |
| **USDZ** | ZIP archive containing USDC and textures for **AR/VR**. | **Compact, portable, lightweight**, widely supported on **iOS & Web**. | **Read-only**, cannot be modified once created. | Augmented Reality (AR), web-based visualization. |

---

# **4. Primitives (Prims) in OpenUSD**

## **4.1. What is a Prim?**

A **Prim (Primitive)** is the **fundamental building block** of OpenUSD.
Every object in a USD scene—whether it’s **geometry, a light, a camera, or even an organizational element**—is represented as a **prim**.

### **Key Features of Prims**

- **Encapsulates data** – Stores properties such as **position, materials, transformations, and metadata**.
- **Hierarchical** – Can be **nested inside other prims**, forming a structured scenegraph.
- **Unique Identifiers** – Each prim has a **unique path** in the USD scenegraph (`/World/Cube`, `/Character/Arm`).
- **Supports Composition** – Prims can **reference** external files and support **overrides** for non-destructive edits.

---

## **4.2. Types of Prims**

Prims in USD are classified based on their **function and visibility** in the scene.

### **Imageable Prims (Renderable)**

These **directly contribute to rendering**. They define **geometry, lights, and cameras**.

| **Prim Type** | **Description** |
| --- | --- |
| **Mesh** | Defines 3D **geometry** (e.g., cube, sphere, character model). |
| **Light** | Represents a **light source** (DistantLight, DomeLight, RectLight). |
| **Xform** | Stores **transformations** (position, rotation, scale). |
| **Camera** | Defines a **camera** in the scene. |
| **Skeleton** | Represents a **skeletal rig** for animation. |

### **Non-Imageable Prims (Organizational)**

These do **not contribute directly to rendering** but **store important metadata**.

| **Prim Type** | **Description** |
| --- | --- |
| **Material** | Stores **shading** and **texturing** data. |
| **Shader** | Defines surface **shading models**. |
| **Scope** | Organizes prims **without affecting transformations**. |
| **Skeletal Animation** | Stores **animation data** for rigged characters. |

---

## **4.3. Prim Attributes & Properties**

Each prim can have **various attributes and metadata** that define its **behavior and appearance**.

### **Common Prim Attributes**

| **Attribute** | **Description** |
| --- | --- |
| **Translation, Rotation, Scale** | Defines object **position and transformations**. |
| **Material Assignment** | Links prim to a **material or shader**. |
| **Visibility** | Controls **whether a prim is visible or hidden**. |
| **Extent** | Defines the **bounding box** of a prim. |
| **Display Color** | Sets the **default color** for a prim (if no material is assigned). |

### **Relationships in Prims**

Prims can **connect to other prims** via **relationships**, which act as **pointers or references**.
Examples:

- A **mesh prim** can have a **relationship** to a **material prim**.
- A **character rig** can have a **relationship** to its **animation data**.

---

## **4.4. Python Examples: Creating and Querying Prims**

- **Creating a New Prim in a USD Stage**
    
    ```python
    from pxr import Usd, UsdGeom
    
    # Create a new USD stage
    stage = Usd.Stage.CreateNew("prims_example.usda")
    
    # Define a root prim
    root_prim = stage.DefinePrim("/World")
    
    # Create a Cube Prim
    cube = UsdGeom.Cube.Define(stage, "/World/Cube")
    
    # Save the stage
    stage.GetRootLayer().Save()
    ```
    
- **Retrieving and Inspecting a Prim**
    
    ```python
    # Open an existing USD stage
    stage = Usd.Stage.Open("prims_example.usda")
    
    # Get the Cube prim
    cube_prim = stage.GetPrimAtPath("/World/Cube")
    
    # Print basic properties
    print("Prim Name:", cube_prim.GetName())
    print("Prim Type:", cube_prim.GetTypeName())
    print("Children:", list(cube_prim.GetChildren()))
    ```
    
- **Checking If a Prim is Imageable**
    
    ```python
    from pxr import UsdGeom
    
    # Check if a prim is a geometric object
    if cube_prim.IsA(UsdGeom.Gprim):
        print("This is a geometric prim.")
    ```
    
- **Listing All Prims in a Scene**
    
    ```python
    # Iterate through all prims in the stage
    for prim in stage.Traverse():
        print("Prim:", prim.GetPath())
    ```
    
- **Modifying a Prim's Attribute**
    
    ```python
    from pxr import Gf
    
    # Modify the Cube's scale
    UsdGeom.XformCommonAPI(cube_prim).SetScale(Gf.Vec3f(2.0, 2.0, 2.0))
    
    # Save changes
    stage.GetRootLayer().Save()
    ```
    

---

# **5. Creating Hierarchies in USD**

## **5.1. The Importance of Scene Organization**

In OpenUSD, organizing objects properly is **crucial** for maintaining a **scalable, readable, and efficient** scene structure.

**Why structure scenes hierarchically?**

- **Improves scene organization** – Keeps assets modular and logical.
- **Enables transformation inheritance** – Parent objects affect children.
- **Supports efficient traversal and querying** – Finding objects is easier.
- **Optimizes rendering and editing workflows** – Grouped objects allow batch operations.

USD provides **two main ways** to structure objects:

1. **Scopes** – Used for **logical grouping** (without transformations).
2. **Xforms** – Used for **spatial transformations** (position, rotation, scale).

---

## **5.2. Scopes: Logical Grouping of Objects**

A **Scope** is used when you need to **organize objects** but **don’t need to move them together**.
Think of it as an **empty folder** in your computer—it helps **group assets logically** but does not change their properties.

### **Key Features of Scopes**

- **Used for grouping** prims into **logical sets**.
- **Does not affect transformations** (no position, rotation, scale).
- **Useful for organizing animations, materials, or different scene elements.**
- **Helps with non-destructive workflows** by keeping data separate.

### **Common Use Cases**

- Grouping **all environment assets** together (`/Environment`).
- Separating **characters from props**.
- Organizing **animation layers separately from geometry**.

### **Python Example: Using Scope to Organize a Scene**

```python
from pxr import Usd, UsdGeom

# Create a new USD stage
stage = Usd.Stage.CreateNew("scene_with_scope.usda")

# Define a Scope to group objects (but NOT transform them)
scope = UsdGeom.Scope.Define(stage, "/SceneObjects")

# Add a cube inside the Scope
cube = UsdGeom.Cube.Define(stage, "/SceneObjects/Cube")

# Check if the prim is a Scope
prim = stage.GetPrimAtPath("/SceneObjects")
print("Is this a Scope?", prim.IsA(UsdGeom.Scope))  # Output: True

# Save the stage
stage.GetRootLayer().Save()
```
---

## **5.3. Xform: Applying Transformations**

Unlike Scopes, an **Xform** is used when you need to **apply transformations (position, rotation, scale)** to a group of objects.

### **Key Features of Xform**

- **Stores transformation data** – Position, rotation, scale.
- **Applies hierarchical transformations** – Child objects **inherit** transformations from parents.
- **Essential for moving complex objects together** – e.g., **vehicles, animated characters, robotic arms**.

<aside>
💡

A **Scope only groups objects**, while an **Xform moves them together**.

</aside>

### **Common Use Cases:**

- Moving **an entire character model** with all its parts.
- Rotating **a robotic arm** with all its components.
- Scaling **a full city model** while keeping object relationships intact.

### **Python Example: Using Xform for Transformation**

```python
from pxr import Usd, UsdGeom, Gf

# Create a new USD stage
stage = Usd.Stage.CreateNew("scene_with_xform.usda")

# Define an Xform to move objects together
xform = UsdGeom.Xform.Define(stage, "/Group")

# Add a cube inside the Xform
cube = UsdGeom.Cube.Define(stage, "/Group/Cube")

# Apply a transformation to the Xform (move 5 units on X-axis)
xform.AddXformOp(UsdGeom.XformOp.TypeTranslate).Set(Gf.Vec3d(5, 0, 0))

# Save the stage
stage.GetRootLayer().Save()
```

---

## **5.4. Scopes vs. Xforms**

A **Scope** is a **purely organizational container** that **groups objects together** but does **not** store transformations.
An **Xform** is a **transformable container** that can **apply transformations** to itself and its child objects.

| **Feature** | **Scope** | **Xform** |
| --- | --- | --- |
| **Purpose** | Grouping objects logically | Grouping **+ applying transformations** |
| **Affects Transformations?** | ❌ No | ✅ Yes |
| **Stores Position/Rotation/Scale?** | ❌ No | ✅ Yes |
| **Use Case** | Organizing objects **without modifying their placement** | Moving, rotating, and scaling objects (and their children) |
| **Example** | Grouping **all materials, all props, or all lights** | Moving **a robot arm or a car with all its components** |

---

# **6. Stage Traversal: Navigating a USD Scene**

## **6.1. What is Stage Traversal?**

Stage traversal is the **process of navigating the hierarchical structure of a USD scene**.
It allows users to:

- **Access and query prims** efficiently.
- **Modify and filter objects** based on their properties.
- **Optimize workflows** for large-scale scene editing and automation.

Since **USD stages are hierarchical**, traversal is a fundamental operation for:

- **Finding specific objects** (`GetPrimAtPath()`).
- **Iterating through all objects** (`stage.Traverse()`).
- **Filtering objects by type, visibility, or metadata** (`Usd.PrimRange()`).

---

## **6.2. Traversal Methods**

USD provides multiple ways to **iterate over prims** within a stage.
The method you choose depends on whether you need:

- **Basic Traversal with `stage.Traverse()`**
    
    ```python
    from pxr import Usd
    
    # Open an existing USD stage
    stage = Usd.Stage.Open("scene.usda")
    
    # Iterate over all prims
    for prim in stage.Traverse():
        print("Prim:", prim.GetPath())
    ```
    
- **Advanced Traversal with `Usd.PrimRange()`**
    
    ```python
    from pxr import Usd
    
    # Open the stage
    stage = Usd.Stage.Open("scene.usda")
    
    # Iterate over all prims using PrimRange
    for prim in Usd.PrimRange(stage.GetPseudoRoot()):
        print("Prim:", prim.GetPath())
    ```
    
- **Direct Children Traversal (`GetChildren()`)**
    
    ```python
    # Get the root prim
    root_prim = stage.GetPseudoRoot()
    
    # Get only top-level children
    for child in root_prim.GetChildren():
        print("Child Prim:", child.GetPath())
    ```
    
- **Retrieving a Specific Prim (`GetPrimAtPath()`)**
    
    ```python
    # Retrieve a specific prim
    prim = stage.GetPrimAtPath("/World/Character")
    
    if prim:
        print("Found prim:", prim.GetPath())
    else:
        print("Prim not found.")
    ```
    
- **Recursive vs. Non-Recursive Traversal**
    - **Recursive Traversal (Full Scene)**
    - **Non-Recursive Traversal (Direct Children Only)**
    
    ```python
    # Recursive Traversal
    for prim in stage.Traverse():
        print("Prim:", prim.GetPath())
    
    # Non-Recursive Traversal (Direct Children Only)
    for child in stage.GetPseudoRoot().GetChildren():
        print("Child Prim:", child.GetPath())
    ```
    

---

## **6.3. Filtering Prims by Metadata**

USD allows filtering prims **based on metadata**, such as **visibility, type, or properties**.

### **Finding All Visible Prims**

```python
from pxr import UsdGeom

# Iterate through the scene and find visible prims
for prim in stage.Traverse():
    if prim.HasAttribute("visibility"):
        visibility_attr = prim.GetAttribute("visibility")
        if visibility_attr.Get() != UsdGeom.Tokens.invisible:
            print("Visible Prim:", prim.GetPath())
```

---

## 6.4. Checking Prim Properties

USD provides functions to check prim states, such as **whether an object is active, loaded, or part of a model group**.

```python
# Checking multiple prim properties
if prim.IsActive():
    print(prim.GetPath(), "is active")

if prim.IsLoaded():
    print(prim.GetPath(), "is loaded")

if prim.IsModel():
    print(prim.GetPath(), "is a model")

if prim.IsGroup():
    print(prim.GetPath(), "is a group")

if prim.IsAbstract():
    print(prim.GetPath(), "is abstract")

if prim.IsDefined():
    print(prim.GetPath(), "is defined")

if prim.IsInstance():
    print(prim.GetPath(), "is an instance")
```

---

# **7. Working with Attributes and Relationships**

## **7.1. What Are Attributes?**

In OpenUSD, **attributes** define an object's **properties and behaviors**.
They store **data values** that affect an object's **appearance, transformations, and relationships within a scene**.

### **Key Features of Attributes**

- **Stores Data Values** → Defines **size, position, color, material properties, and more**.
- **Supports Animation** → Can be **keyframed** over time for dynamic scenes.
- **Layered & Overridable** → Edits **don’t modify original data**, enabling **non-destructive workflows**.
- **Efficient Storage** → Uses **USD’s time sampling model** for optimized **memory usage**.

### **Common Attribute Examples**

| **Attribute** | **Description** | **Example** |
| --- | --- | --- |
| **Transformations** | Defines an object's **position, rotation, and scale**. | `"translate" = (10, 5, 0)` |
| **Material Properties** | Stores **color, roughness, metalness, and shader values**. | `"diffuseColor" = (1.0, 0.5, 0.2)` |
| **Visibility** | Determines if an object is **rendered or hidden**. | `"visibility" = "invisible"` |
| **Extent** | Stores **bounding box information** for optimization. | `"extent" = [(0, 0, 0), (1, 1, 1)]` |

### **Python Examples: Creating & Modifying Attributes**

- **Creating a New Attribute**
    
    ```python
    from pxr import Usd, UsdGeom, Sdf
    
    # Create a new USD stage
    stage = Usd.Stage.CreateNew("attributes.usda")
    
    # Define a sphere
    sphere = UsdGeom.Sphere.Define(stage, "/MySphere")
    
    # Set radius using schema-specific API (recommended)
    sphere.GetRadiusAttr().Set(2.5)
    
    # Create a custom float attribute (e.g., mass of an object)
    custom_attr = sphere.GetPrim().CreateAttribute("custom:mass", Sdf.ValueTypeNames.Float)
    custom_attr.Set(10.0)
    
    # Retrieve and print attribute values
    print("Radius:", sphere.GetRadiusAttr().Get())
    print("Mass:", custom_attr.Get())
    
    # Save the stage
    stage.GetRootLayer().Save()
    ```
    
- **Overriding an Attribute Without Modifying the Original File**
    
    ```python
    # Open an existing stage
    stage = Usd.Stage.Open("attributes.usda")
    
    # Get the sphere
    sphere = stage.GetPrimAtPath("/MySphere")
    
    # Override the radius attribute
    radius_attr = sphere.GetAttribute("radius")
    radius_attr.Set(5.0)  # Change radius to 5.0
    
    # Save the modifications
    stage.GetRootLayer().Save()
    ```
    
- **Checking If an Attribute Exists**
    
    ```python
    # Check if the prim has a specific attribute
    if sphere.HasAttribute("custom:mass"):
        print("Mass attribute exists!")
    ```
    

---

## **7.2. What Are Relationships?**

A **relationship** in OpenUSD defines a **connection between two or more prims**.
Instead of storing data **directly**, relationships **act as links**, referencing other objects in the scene.

### **Key Features of Relationships**

- **Links Prims** → Connects objects **without embedding data**.
- **Parent-Child Relationships** → Used to define **hierarchy and dependencies**.
- **Material Binding** → Links **objects to materials and shaders**.
- **Constraint Binding** → Assigns **rigs, constraints, or physics rules**.
- **Automatically Updates** → If a referenced prim’s path changes, relationships **adjust dynamically**.

### **Python Examples: Creating & Querying Relationships**

- **Defining a Parent-Child Relationship**
    
    ```python
    from pxr import Usd
    
    # Create a new USD stage
    stage = Usd.Stage.CreateNew("relationships.usda")
    
    # Define two prims
    parent = stage.DefinePrim("/Parent")
    child = stage.DefinePrim("/Parent/Child")
    
    # Create a relationship from parent to child
    relationship = parent.CreateRelationship("childLink")
    relationship.SetTargets(["/Parent/Child"])
    
    # Save the stage
    stage.GetRootLayer().Save()
    ```
    
- **Binding a Material to Geometry Using Relationships**
    
    ```python
    from pxr import Usd, UsdShade
    
    # Create a new USD stage
    stage = Usd.Stage.CreateNew("material_binding.usda")
    
    # Define a mesh and a material
    mesh = stage.DefinePrim("/MyMesh")
    material = UsdShade.Material.Define(stage, "/MyMaterial")
    
    # Create a relationship to bind the material to the mesh
    binding = mesh.CreateRelationship("material:binding")
    binding.SetTargets(["/MyMaterial"])
    
    # Save the stage
    stage.GetRootLayer().Save()
    ```
    
- **Querying a Relationship**
    
    ```python
    # Open the USD stage
    stage = Usd.Stage.Open("material_binding.usda")
    
    # Get the mesh prim
    mesh = stage.GetPrimAtPath("/MyMesh")
    
    # Get the material relationship
    relationship = mesh.GetRelationship("material:binding")
    
    # Print the linked material
    print("Material Bound To Mesh:", relationship.GetTargets())
    ```
    
- **Removing a Relationship**
    
    ```python
    # Remove the relationship
    relationship.ClearTargets()
    ```
---

## **7.3. Comparison Table: Attributes vs. Relationships**

| **Feature** | **Attributes** | **Relationships** |
| --- | --- | --- |
| **Purpose** | Stores **data values** | Stores **connections** between objects |
| **Data Type** | Number, string, vector, etc. | References to other prims |
| **Supports Animation?** | ✅ Yes | ❌ No |
| **Example** | `"radius" = 2.5` | `"material:binding" → "/MyMaterial"` |
| **Use Case** | Position, color, mass, etc. | Linking meshes to materials, constraints |

---

## **7.4. Stage Composition & Layering**

OpenUSD organizes data in **layers**, allowing for **non-destructive modifications** without altering the base scene.

### **How USD Handles Layer Overrides**

USD supports **layering**, where each USD file is a **separate layer**.
Layers are **stacked in order**, and **stronger layers override weaker ones**.

| **Layer Type** | **Description** | **Example** |
| --- | --- | --- |
| **Root Layer** | Base USD file. | `"base_scene.usda"` |
| **SubLayers** | Additional layers **added on top**. | `"lighting.usda"` |
| **Overrides** | Applies modifications **without changing the source**. | `"overrides.usda"` |

<aside>

Note on Layer Strength:
In USD’s layer stack, the bottom-most layer is the weakest, and each successively higher layer is stronger. This means changes in the top-most layer can override data authored in lower layers.

</aside>

### **Python Example: Working with Layer Stacks**

```python
from pxr import Usd, Sdf

# Open a stage with multiple layers
stage = Usd.Stage.Open("scene_with_layers.usda")

# List all layers in the stack
for layer in stage.GetLayerStack():
    print("Layer:", layer.identifier)
```

---

# **8. Primvars: Enhancing Geometry with Custom Data**

## **8.1. What Are Primvars?**

**Primvars (Primitive Variables)** are a special type of **attribute** in OpenUSD used to store **per-vertex, per-face, or per-instance data**.
They are **crucial for rendering, shading, and procedural workflows**.
Unlike standard attributes that store **single values per prim**, **primvars store multiple values** across a prim’s **geometry**.

---

## **8.2. Why Use Primvars?**

- **Stores Per-Vertex Data** – Used for **vertex colors, UV coordinates, and procedural attributes**.
- **Supports Interpolation** – Values are **smoothed across surfaces**.
- **Optimized for Rendering** – Provides **efficient shader access**.
- **Can Be Inherited** – Allows for **sparse authoring**, meaning shared data can be defined at higher levels.

### **Common Use Cases**

- **Texture Mapping** → Storing **UV coordinates** for shaders.
- **Vertex Colors** → Assigning **per-vertex colors** to objects.
- **Procedural Effects** → Storing **randomization factors** for shading.
- **Custom Metadata** → Embedding **user-defined values** for advanced workflows.

### **Key Features of Primvars**

| **Feature** | **Description** |
| --- | --- |
| **Per-Vertex, Per-Face, or Per-Instance Data** | Can store values for **each vertex, face, or instance**. |
| **Supports Interpolation** | Data can be **smoothed across geometry**. |
| **Shader-Friendly** | Optimized for **shading and rendering workflows**. |
| **Sparse Authoring** | Data can be **defined at a higher level and inherited**. |

<aside>
💡

**Primvars vs. Attributes**

- **Attributes** store **single values** (e.g., object color).
- **Primvars** store **multiple values** (e.g., per-vertex colors).
</aside>

---

## **8.3. Python Examples: Defining Per-Vertex Colors & Texture Coordinates**

- **Assigning Per-Vertex Colors**
    
    ```python
    from pxr import Usd, UsdGeom, Sdf, Gf
    
    # Create a new USD stage
    stage = Usd.Stage.CreateNew("primvars.usda")
    
    # Define a mesh prim
    mesh = UsdGeom.Mesh.Define(stage, "/MyMesh")
    
    # Create a Primvars API instance
    primvar_api = UsdGeom.PrimvarsAPI(mesh.GetPrim())
    
    # Create a per-vertex color primvar
    color_primvar = primvar_api.CreatePrimvar("primvars:displayColor", Sdf.ValueTypeNames.Color3fArray)
    
    # Assign colors (RGB values)
    color_primvar.Set([Gf.Vec3f(1.0, 0.0, 0.0),  # Red
                       Gf.Vec3f(0.0, 1.0, 0.0),  # Green
                       Gf.Vec3f(0.0, 0.0, 1.0)]) # Blue
    
    # Save the stage
    stage.GetRootLayer().Save()
    ```
    
- **Storing UV Texture Coordinates**
    
    ```python
    # Create a UV texture coordinate primvar
    uv_primvar = primvar_api.CreatePrimvar("primvars:st", Sdf.ValueTypeNames.TexCoord2fArray)
    
    # Assign UV values (U, V coordinates)
    uv_primvar.Set([Gf.Vec2f(0.0, 0.0),  # Bottom-left
                    Gf.Vec2f(1.0, 0.0),  # Bottom-right
                    Gf.Vec2f(1.0, 1.0),  # Top-right
                    Gf.Vec2f(0.0, 1.0)]) # Top-left
    
    # Save the stage
    stage.GetRootLayer().Save()
    ```
    
- **Querying an Existing Primvar**
    
    ```python
    # Retrieve the displayColor primvar
    primvar = primvar_api.GetPrimvar("primvars:displayColor")
    
    # Print stored values
    print("Stored Vertex Colors:", primvar.Get())
    ```
    

---

## **8.4. Comparison Table: Attributes vs. Primvars**

| **Feature** | **Attributes** | **Primvars** |
| --- | --- | --- |
| **Data Type** | Stores **single values** (e.g., `"radius" = 2.5"`) | Stores **arrays of values** (e.g., `"vertexColors" = [R, G, B]`) |
| **Per-Vertex/Faces/Instances?** | ❌ No | ✅ Yes |
| **Interpolated?** | ❌ No | ✅ Yes |
| **Shader Access?** | ❌ Not directly | ✅ Yes |
| **Best Use Case** | **Object-level properties** (e.g., mass, visibility) | **Per-vertex data** (e.g., UV mapping, vertex colors) |

---

# **9. Xform Common API: Applying Transformations**

## **9.1. What is Xform Common API?**

The **Xform Common API** is a **standardized transformation system** in OpenUSD that provides a **consistent way** to manipulate object transformations.

---

## **9.2. Why Use Xform Common API?**

- **Simplifies transformation operations** – Provides **easy-to-use methods** for **translation, rotation, scale, pivot, and ordering**.
- **Ensures consistency** – Uses a **standardized way** to manipulate transforms **across different software tools**.
- **Works with time samples** – Allows **smooth animation** of transformations over time.
- **Built on top of UsdGeom.Xform** – Makes transformation editing **more intuitive**.

### **Key Features**

| **Feature** | **Description** |
| --- | --- |
| **Standardized API** | Provides a **unified way** to manipulate transformations. |
| **Works with Keyframes** | Can animate transformations **over time**. |
| **Compatible with UsdGeom.Xform** | Allows hierarchical transformations. |
| **Handles Pivot Offsets** | Supports pivot-based transformations. |

---

## **9.3. Standardized Transform Operations**

The **Xform Common API** provides built-in methods for applying transformations.

- **Translation (Move an Object)**
    
    ```python
    xform_api.SetTranslate((10, 0, 0))  # Move 10 units along the X-axis
    ```
    
- **Rotation (Rotate an Object)**
    
    ```python
    xform_api.SetRotate((0, 45, 0))  # Rotate 45 degrees around Y-axis
    ```
    
- **Scale (Resize an Object)**
    
    ```python
    xform_api.SetScale((2, 2, 2))  # Double the size
    ```
    
- **Pivot Offset (Rotate Around a Specific Point)**
    
    ```python
    xform_api.SetPivot((0, -1, 0))  # Move pivot 1 unit down
    ```
    
- **Rotation Order**
    
    ```python
    xform_api.SetRotationOrder(UsdGeom.XformCommonAPI.RotationOrderXYZ)
    ```
    

---

## **9.4. How Xform Common API Differs from UsdGeom.Xform**

| **Feature** | **UsdGeom.Xform** | **Xform Common API** |
| --- | --- | --- |
| **Purpose** | General transformation container | Simplified API for applying transformations |
| **Translation** | Uses **UsdGeom.XformOp** | Uses **SetTranslate()** |
| **Rotation** | Uses **XformOp with Rotation Types** | Uses **SetRotate()** |
| **Scale** | Uses **XformOp** | Uses **SetScale()** |
| **Pivot Handling** | Requires multiple ops | Built-in **SetPivot()** function |
| **Easier to Use?** | ❌ No | ✅ Yes |

<aside>
💡

**Use Xform Common API when you need a simple way to apply transformations**.

**Use UsdGeom.Xform for more complex, fine-grained transformation control**.

</aside>

### **Python Examples: Applying Transformations with Xform Common API**

- **Basic Transformation Example**
    
    ```python
    from pxr import Usd, UsdGeom, Gf
    
    # Create a new USD stage
    stage = Usd.Stage.CreateNew("xform_common.usda")
    
    # Define an Xform prim
    xform = UsdGeom.Xform.Define(stage, "/MyXform")
    
    # Apply the Xform Common API
    xform_api = UsdGeom.XformCommonAPI.Apply(xform)
    
    # Set transformations
    xform_api.SetTranslate((5, 0, 0))  # Move 5 units on X-axis
    xform_api.SetRotate((0, 45, 0))  # Rotate 45 degrees on Y-axis
    xform_api.SetScale((2, 2, 2))  # Double the size
    
    # Save the stage
    stage.GetRootLayer().Save()
    ```
    
- **Working with Pivot Offsets**
    
    ```python
    # Set pivot to move the center point
    xform_api.SetPivot((0, -1, 0))  # Move pivot 1 unit down
    xform_api.SetRotate((0, 90, 0))  # Rotate 90 degrees
    ```
    
- **Animating Transformations Over Time**
    
    ```python
    # Move object at frame 1
    xform_api.SetTranslate((0, 0, 0), time=1)
    
    # Move object at frame 10
    xform_api.SetTranslate((10, 0, 0), time=10)
    
    # Move object at frame 20
    xform_api.SetTranslate((20, 0, 0), time=20)
    ```
    

---

# **10. Hydra: OpenUSD’s Rendering Architecture**

## **10.1. What is Hydra?**

**Hydra** is OpenUSD’s **high-performance rendering framework**.
It serves as a **bridge between USD scene descriptions and different render engines**, allowing for **real-time and offline rendering**.

---

## **10.2. Why Use Hydra?**

- **Decouples scene representation from rendering** → Any renderer can be used without modifying scene data.
- **Supports multiple rendering engines** → Works with **Pixar's RenderMan, NVIDIA Omniverse, Arnold, and more**.
- **Optimized for large-scale rendering** → Handles **millions of objects efficiently**.
- **Built for real-time workflows** → Enables **fast previews** in DCC tools like **Houdini, Maya, Blender**.

<aside>
💡

**Think of Hydra as a middle layer** that **translates USD data into optimized render-ready form**.

</aside>

### **Key Features of Hydra**

| **Feature** | **Description** |
| --- | --- |
| **Renderer Agnostic** | Works with different renderers via a **plugin system**. |
| **Real-Time & Offline Support** | Enables **interactive previews** and **final renders**. |
| **Efficient Data Processing** | Uses **parallel processing** for **fast rendering**. |
| **Supports Multiple Render Delegates** | Can use **HdStorm, HdEmbree, Arnold, and more**. |
| **Modular & Scalable** | Handles **massive** environments efficiently. |

---

## **10.3. How Hydra Works**

Hydra follows a **modular pipeline** consisting of **three main components**:

1. **Scene Delegate**
- Provides **raw scene data** to Hydra.
- Converts USD prims into **renderable data**.
- Examples: **UsdImagingDelegate** (USD-native delegate).
1. **Render Index**
- Acts as a **data cache** and **manager**.
- Tracks **changes to objects, materials, lights**.
- Optimizes data flow to the renderer.
1. **Render Delegate**
- Handles **final rendering**.
- Different render delegates produce **different visual outputs**.
- Examples: **HdStorm (default), HdEmbree (CPU ray tracing), Arnold, RenderMan**.

### **Hydra Process Flow**

```vbnet
USD Scene  →  Scene Delegate  →  Render Index  →  Render Delegate  →  Final Image
```

---

## **10.4. Comparison: Hydra vs. Direct Rendering**

| **Feature** | **Hydra Rendering** | **Direct Rendering** |
| --- | --- | --- |
| **Flexibility** | Works with multiple renderers | Tied to a single renderer |
| **Performance** | Optimized for large-scale scenes | Can be slower on complex scenes |
| **Modularity** | Can swap render engines easily | Limited to the built-in renderer |
| **Interoperability** | Works with **Pixar RenderMan, Arnold, etc.** | Usually proprietary |
| **Usage** | Used in **DCC tools** (Maya, Houdini, Blender) | Standalone renderers |

<aside>
💡

**Hydra is best for** **interactive previews and production pipelines** where render flexibility is needed.

**Direct rendering is better for** **single-use cases** where a specific render engine is required.

</aside>

---

## **10.5. Supported Render Delegates**

Hydra supports multiple render engines via **render delegates**.

| **Render Delegate** | **Description** |
| --- | --- |
| **HdStorm** (Default) | **GPU-based rasterization** for **real-time rendering**. |
| **HdEmbree** | **Intel Embree-based CPU ray tracing** for high-quality images. |
| **HdArnold** | Arnold renderer support for **VFX workflows**. |
| **HdRenderMan** | **Pixar’s RenderMan** integration for feature film-quality rendering. |
| **HdKarma** | Houdini’s **Karma Renderer** support. |

---

## **10.6. Python Example: Rendering with Hydra**

Hydra can be **controlled via Python** by setting a **render delegate** and enabling scene updates.

- **Setting Up a Hydra Renderer**
    
    ```python
    from pxr import Usd, UsdImagingGL
    
    # Load a USD stage
    stage = Usd.Stage.Open("scene.usda")
    
    # Create a Hydra renderer
    renderer = UsdImagingGL.Engine()
    
    # Render the scene
    renderer.Render(stage)
    ```
    
- **Switching Render Delegates**
Hydra supports switching render engines dynamically.
    
    ```python
    from pxr import HdStorm, HdEmbree
    
    # Set Hydra to use the HdStorm renderer (GPU-based)
    renderer.SetRendererPlugin(HdStorm.GetRendererPluginId())
    
    # Switch to HdEmbree (CPU ray tracing)
    renderer.SetRendererPlugin(HdEmbree.GetRendererPluginId())
    ```
    

---

# **11. Lighting a USD Scene**

Lighting is a **crucial** aspect of scene visualization in OpenUSD.
**USD provides built-in light prims** that allow developers to illuminate a scene efficiently.

## **11.1. Common USD Lighting Modules (`UsdLux`)**

Lighting in OpenUSD is managed through the **UsdLux** module, which provides:

- **Standardized light types** (Distant, Dome, Rect, Sphere, etc.).
- **Advanced lighting features** (light linking, filters, IES profiles).
- **Compatibility with multiple renderers** (Hydra, RenderMan, Arnold).
- **Realistic illumination for physically-based rendering (PBR) workflows**.

---

## **11.2. Types of USD Lights**

USD supports multiple **light types** for different use cases.

| **Light Type** | **Description** | **Use Case** |
| --- | --- | --- |
| **DistantLight** | Simulates a **sunlight-style** light source. | Outdoor environments. |
| **DomeLight** | Uses an **HDRI texture** for ambient lighting. | Skyboxes, realistic reflections. |
| **RectLight** | A **rectangular area light** that casts soft shadows. | Studio lighting, softbox effects. |
| **DiskLight** | A **circular area light**, similar to a spotlight. | Focused lighting, cinematic effects. |
| **SphereLight** | Emits light in **all directions** from a sphere. | Simulates **light bulbs, glowing objects**. |
| **CylinderLight** | Emits light from a **cylindrical shape**. | Neon lights, tubes, light strips. |

---

## **11.3. Python Examples: Creating and Modifying Lights**

- **Adding a Distant (Sun) Light**
    
    ```python
    from pxr import Usd, UsdLux
    
    # Create a new USD stage
    stage = Usd.Stage.CreateNew("lighting.usda")
    
    # Create a Distant Light (Sun-like)
    distant_light = UsdLux.DistantLight.Define(stage, "/SunLight")
    distant_light.GetIntensityAttr().Set(500)  # Set brightness
    
    # Save the stage
    stage.GetRootLayer().Save()
    ```
    
- **Adding a Dome Light (HDRI Environment)**
    
    ```python
    # Create a Dome Light
    dome_light = UsdLux.DomeLight.Define(stage, "/EnvironmentLight")
    
    # Load an HDRI texture
    dome_light.GetTextureFileAttr().Set("hdr_sky.exr")
    
    # Save the stage
    stage.GetRootLayer().Save()
    ```
    
- **Adding a Rectangular Area Light**
    
    ```python
    # Create a Rect Light (soft lighting source)
    rect_light = UsdLux.RectLight.Define(stage, "/SoftLight")
    rect_light.GetIntensityAttr().Set(100)
    rect_light.GetWidthAttr().Set(10)  # Define size
    rect_light.GetHeightAttr().Set(5)
    
    # Save the stage
    stage.GetRootLayer().Save()
    ```
    

---

## **11.4. Advanced Light Properties**

USD lights include **advanced features** for **realistic lighting effects**.

| **Attribute** | **Description** |
| --- | --- |
| **Intensity** | Controls **light brightness**. |
| **Color** | Defines the **RGB color** of the light. |
| **Exposure** | Adjusts brightness **like a camera exposure setting**. |
| **Width/Height** | Defines the **size of RectLights**. |
| **Texture File** | Applies an **HDRI image** to a **DomeLight**. |
| **IES Profiles** | Uses **real-world photometric light data**. |
| **Light Filters** | Modifies **light behavior using masks and filters**. |
| **Light Linking** | Assigns **specific lights to specific objects**. |
- **Modifying Light Properties**
    
    ```python
    # Set light color (warm tone)
    rect_light.GetColorAttr().Set((1.0, 0.8, 0.6))  # Warm light (orange)
    
    # Adjust exposure
    rect_light.GetExposureAttr().Set(2.0)  # Brighter light
    
    # Apply an IES light profile
    rect_light.GetInputsIntensityAttr().Set(50)  # Adjust light falloff
    
    # Save the stage
    stage.GetRootLayer().Save()
    ```
    
- **3-Point Setup**
    
    ```python
    # Key Light
    key = UsdLux.RectLight.Define(stage, "/Lights/KeyLight")
    key.GetIntensityAttr().Set(300)
    
    # Fill Light
    fill = UsdLux.RectLight.Define(stage, "/Lights/FillLight")
    fill.GetIntensityAttr().Set(100)
    
    # Rim Light
    rim = UsdLux.DiskLight.Define(stage, "/Lights/RimLight")
    rim.GetIntensityAttr().Set(200)
    ```
    

<aside>
💡

Lights can be moved or rotated using Xform

</aside>

---

# **12. Final Considerations & Best Practices**

## **12.1. USD Performance Optimization Tips**

When working with **large-scale USD scenes**, optimizing **scene loading, rendering, and storage** is crucial.

1. **Use Binary Formats (`USDC`) for Large Scenes**
    1. **USDA (ASCII format)** is **great for debugging** but is **slow for large projects**.
    2. **USDC (Binary format)** is **faster** and **more compact**.
    
    ```python
    usd_stage = Usd.Stage.Open("scene.usda")
    usd_stage.Export("scene.usdc")
    ```
    
2. **Use Payloads for On-Demand Loading**
    1. **Payloads** allow large assets to be **loaded only when needed**, reducing memory usage and improving load times.
    
    ```python
    from pxr import Usd
    
    stage = Usd.Stage.CreateNew("main_scene.usda")
    root_prim = stage.DefinePrim("/World")
    root_prim.GetPayloads().AddPayload("large_asset.usdc")
    stage.GetRootLayer().Save()
    ```
    
3. **Optimize Scenegraph Hierarchy**
    1. **Minimize nested transforms (`Xforms`)** to speed up traversal.
    2. **Use** **`Scopes`** for logical organization and **`Xforms`** for transformations.
    
    <aside>
    ⚠️
    
    Too many nested `Xforms` slow down traversal.
    
    </aside>
    
4. **Use Instancing for Repeated Objects**
    1. Instead of duplicating geometry, use **instances** to **share data**, reducing file size and memory usage.
        
        ```python
        from pxr import Usd, UsdGeom
        
        stage = Usd.Stage.CreateNew("instancing.usda")
        
        # Define a base cube
        cube = UsdGeom.Cube.Define(stage, "/Cube")
        
        # Create an instance
        instance = stage.DefinePrim("/Instance")
        instance.GetReferences().AddReference("instancing.usda", "/Cube")
        
        stage.GetRootLayer().Save()
        ```
        
5. **Use Layering for Non-Destructive Edits**
    1. **Layer overrides allow modifications without changing the original file**, ideal for collaborative workflows.
    
    ```python
    stage = Usd.Stage.Open("base_scene.usda")
    override_layer = Sdf.Layer.CreateAnonymous()
    stage.GetRootLayer().subLayerPaths.append(override_layer.identifier)
    ```
    

---

## **12.2. Best Practices for Scene Organization**

A well-structured USD scene is **easier to work with** and **more performant**.

1. **Follow a Clear Naming Convention**
    
    <aside>
    
    ✅ Use **consistent, descriptive names** for objects and paths
    
    ❌ **Avoid generic names like** `Prim1`, `ObjectX`.
    
    </aside>
    
    ```swift
    /World
     ├── /Environment
     │    ├── /Trees/TreeA
     │    ├── /Trees/TreeB
     │    ├── /Sky
     ├── /Character
     │    ├── /Body
     │    ├── /Clothes
     │    ├── /Materials
    ```
    
2. **Separate Logical Groups Using `Scopes`**
    1. **Use Scopes (`UsdGeom.Scope`)** to **organize** objects **without affecting transformations**.
        
        ```python
        from pxr import UsdGeom
        
        scope = UsdGeom.Scope.Define(stage, "/Environment")
        ```
        
3. **Minimize Transformation Hierarchies**
    1. **Too many nested Xforms slow down performance.**
    2. **Group movable objects under a single Xform.**
    
    ```python
    from pxr import UsdGeom
    
    xform = UsdGeom.Xform.Define(stage, "/MovingGroup")
    UsdGeom.Cube.Define(stage, "/MovingGroup/Cube")
    ```
    
4. **Use `Payloads` for Large External Assets**
    1. **Store heavy models in separate USD files** and reference them when needed.
    
    **Benefit:** Improves scene loading speed.
    

## **Common Mistakes to Avoid**

❌ **Mistake 1: Using `USDA` for Production Scenes**

❌ **Mistake 2: Duplicating Geometry Instead of Instancing**

❌ **Mistake 3: Overusing Xform Hierarchies**

❌ **Mistake 4: Modifying Base Files Instead of Using Overrides**

---
