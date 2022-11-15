### 构建 VR 应用 – Cesium

本教程将带您完成使用 Cesium for Unreal 构建虚拟现实 (VR) 应用程序的过程。它将讨论将您的应用程序部署到有线和独立耳机，并将提供 C++ 和蓝图的示例代码。

![img](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/03d461fc-ee15-4a55-a454-8315e2d6d792_CesiumVRTutorial_000.png)

您将学习如何：

-   在 Unreal 中创建 VR-ready 项目
-   创建一个适用于您的 VR 设备的简单用户控制器
-   直接从 Unreal 在您的 VR 设备中预览您的关卡
-   将您的应用程序打包并部署到独立耳机

## [先决条件](https://cesium.com/learn/unreal/unreal-building-vr/#prerequisites)

-   VR 设备，例如 Oculus Quest、HTC Vive 或 Valve Index。
-   与您的 VR 设备兼容的计算机。
    -   Oculus Quest：[Oculus Link 要求](https://support.oculus.com/articles/headsets-and-accessories/oculus-link/oculus-link-compatibility/)和[连接说明](https://support.oculus.com/articles/headsets-and-accessories/oculus-link/connect-link-with-quest-2/)
    -   HTC Vive：[说明](https://support.steampowered.com/steamvr/HTC_Vive/)
    -   阀门索引：[说明](https://support.steampowered.com/kb_article.php?ref=9140-EYIL-0086)
-   虚幻引擎的已安装版本（至少 4.26 或更高版本）。
-   Cesium for Unreal 插件的安装版本。
-   带有 C++ 工作负载的桌面开发的 Visual Studio 2019。

## 1[设置项目](https://cesium.com/learn/unreal/unreal-building-vr/#step-1-set-up-the-project)

1在 Unreal 中创建一个新项目，然后在**New Project Categories**下选择**Games**。单击**下一步**。

![img](https://images.prismic.io/cesium/769f6e3b-4582-4f0c-b157-b7b6c32892dc_CesiumVRTutorial_010.png?auto=compress%2Cformat&w=852)

![](https://images.prismic.io/cesium/0dd1ebff-5bf9-41b5-9101-eb120e8c7bc2_CesiumVRTutorial_020.png?auto=compress%2Cformat&w=1320)

3在**项目设置**下，您可以选择**C++**或**蓝图**。本教程将为这两种项目类型提供说明。

4同样在**Project Settings**下，选择**Scalable 3D or 2D**、**Raytracing Disabled**、**Mobile/Tablet**和**No Starter Content**。为您的项目命名（例如，“CesiumVRTutorial”）。

![](https://images.prismic.io/cesium/b8697634-518b-4a5e-bae5-a5a6817add3a_CesiumVRTutorial_030.png?auto=compress%2Cformat&w=1012)

6项目完全加载后，转到顶部菜单中的**编辑 > 插件**。找到**Cesium for Unreal**并确保它已**启用**。如果不是，请启用它，然后重新启动 Unreal。

7在顶部菜单中，转到“**文件”>“新关卡**...”，然后在出现的窗口中选择“**空关卡”。**新关卡即将开启。

8打开新关卡后，转到顶部菜单中的**文件 > 当前另存为...。**为您的关卡命名（例如，“Main”）并将其保存在**Content**文件夹或子文件夹中。

9在屏幕的左侧，您应该会看到一个**Cesium**选项卡。点击它。

![](https://images.prismic.io/cesium/e8098adf-019b-46cd-8a68-b30fa4da99ec_CesiumVRTutorial_040.png?auto=compress%2Cformat&w=453)

10在 Cesium 选项卡中，单击**连接**按钮。您的网络浏览器将打开并显示登录您的 Cesium ion 帐户的说明。按照说明进行操作，然后在出现提示时返回 Unreal。

11 Cesium选项卡现在应该有一些新选项，包括一个标记为**Cesium** **World Terrain + Bing Maps Aerial Imagery**的选项。单击该选项旁边的**+按钮，将 Cesium World Terrain 添加到您的关卡中。**

![](https://images.prismic.io/cesium/811acc9d-04b9-4cf2-96e0-2308fd75d4ca_CesiumVRTutorial_050.png?auto=compress%2Cformat&w=459)

12翻回**Place Actors选项卡，该选项卡通常位于****Cesium**选项卡旁边。

13在**放置 Actors**选项卡中，选择**灯光**。将**平行光**拖到关卡中。完成此步骤后，您应该能够在关卡预览窗口中看到渲染的 Cesium World Terrain。

![](https://images.prismic.io/cesium/9f00ad27-06b8-42c2-a447-9e5683afd511_CesiumVRTutorial_060.png?auto=compress%2Cformat&w=459)

如果您希望包含一个天空盒，请从**Place Actors**面板添加一个**BP\_Sky\_Sphere 。**

Information

问：_我可以使用_ _**[CesiumSunSky](https://cesium.com/learn/unreal/unreal-geospatially-accurate-sun/)**代替方向灯和天空盒吗？_

A: 可以，但是您需要在**CesiumSunSky的****Details**面板中**启用 Enable Mobile Rendering**选项。您还需要在**“项目设置”>“引擎”>“渲染”>“默认设置”**下的“自动曝光”设置中启用**“自动曝光”**和“**扩展默认亮度范围**” 。

14此时，您应该能够使用 Unreal 的**VR 预览**功能测试您的关卡。我们将在下一节中添加一个自定义用户控制器，但现在，请按照您的设备的说明连接到您的计算机（请参阅上面的先决条件部分）。

15将您的 VR 设备连接到计算机。您可能需要重新启动 Unreal 才能确认该设备。

16在关卡预览窗口上方的工具栏中，找到“播放”按钮。根据您的设置，它可能会显示在工具栏中。如果存在，请单击“播放”按钮旁边的小箭头。如果不存在，请单击工具栏右侧的双箭头（工具提示 =**单击以展开工具栏**），然后选择**活动播放模式**。

![](https://images.prismic.io/cesium/c9ac1525-f5f4-42f2-9e17-7daffb7dfacc_CesiumVRTutorial_070.png?auto=compress%2Cformat&w=571)

17选择**VR 预览**。戴上您的 VR 耳机，环顾您的关卡。您还不能在关卡周围传送，但您应该会在下方看到铯世界地形。

18设置我们下一步需要的动作映射。转到**编辑 > 项目设置**...。将出现一个新窗口。在新窗口的左侧边栏中，转到**Engine > Input**。

![](https://images.prismic.io/cesium/e005147d-fedb-44f3-811f-baa25073723a_CesiumVRTutorial_075.png?auto=compress%2Cformat&w=1009)

19创建两个**动作映射**——将它们命名为“LineTrace”和“Teleport”——并将它们分别绑定到 VR 设备右侧控制器上的不同按钮。下面的示例使用 Oculus Touch 控制器，用于 Oculus Quest 2。

![](https://images.prismic.io/cesium/942d8df0-0124-4843-9eef-44e00ecd9934_CesiumVRTutorial_080.png?auto=compress%2Cformat&w=667)

## 2[创建棋子](https://cesium.com/learn/unreal/unreal-building-vr/#step-2-create-the-pawn)

接下来，您将制作一个简单的用户控制器，让您可以在 VR 关卡中移动。控制器将“从头开始”制作（即，使用内置的虚幻组件），而不是依赖于预制的**Pawn**，这样您就可以了解从头到尾的过程——并学习如何进行更改以适合您的使用案子。

Information

问：_我可以使用 Unreal 的 VR 模板中的**Pawn**而不是自己制作吗？_

A: 是的，但你需要修改它。该**Pawn**依赖于**Nav Mesh Bounds Volume**来确定用户可以移动的位置，这不适用于 Unreal 的 Cesium，它动态加载地形并有自己的系统来处理细节级别。

用户控制器将使用虚拟激光指示器在关卡中移动。用户将能够使用正确的控制器指向地形并按下按钮。只要他们按住那个按钮，他们就会在控制器指向的地方看到一个点。然后，他们可以按第二个按钮传送到那个地方。

![](https://prismic-io.s3.amazonaws.com/cesium/052f6234-a0bd-40b9-b363-8033971036a3_CesiumVRTutorial_085.gif)

Information

问：_我可以实施不同的移动方案，比如使用摇杆在地形上连续移动吗？_

答：是的，如果您已经使用过 Unreal，您可以制作自己的**Pawn**并添加自定义移动逻辑。如果您是 Unreal 的新手，下面的步骤将向您展示基础知识，您应该能够从那里修改**Pawn**。

本教程使用基于传送的运动方案，因为它最不可能导致用户晕车。

在下一步中，您将设置**Pawn**以允许用户在关卡中移动。如果您的项目使用 C++，请继续阅读**步骤 2a**。如果您希望使用蓝图，请跳至**步骤 2b**。

## 2a: [C++](https://cesium.com/learn/unreal/unreal-building-vr/#2a-c)

如果您不想使用 C++ 编写代码并希望改用蓝图事件图，请转至下面的**步骤 2b**。

1通过转至虚幻编辑器左上角的**文件 > 新建 C++ 类…**添加一个新的 C++ 类。

![](https://images.prismic.io/cesium/a2982fc7-12f0-4f6f-ae85-1e05e350875e_CesiumVRTutorial_090.png?auto=compress%2Cformat&w=263)

![](https://images.prismic.io/cesium/199e43d1-37ec-4176-bd2a-62bb85900360_CesiumVRTutorial_100.png?auto=compress%2Cformat&w=1187)

3单击**下一步**按钮。在接下来的页面上，将新类的名称设置为“VRPawn”。单击绿色的**创建类**按钮。

![](https://images.prismic.io/cesium/4d6bd6ee-6804-46ca-a993-92f035037239_CesiumVRTutorial_110.png?auto=compress%2Cformat&w=1187)

4 Visual Studio 应自动打开该文件。如果没有，请转到**File > Open Visual Studio**在 Visual Studio 中打开项目。

5在 Visual Studio 中，使用右侧的**解决方案资源管理器**查找并打开以下文件： `Source/<YourProjectName>/<YourProjectName>.Build.cs`.

6在该文件中，您会看到一行以字符串数组开头`PublicDependencyModuleNames.AddRange`并包含字符串数组。将 " `HeadMountedDisplay`" 添加到数组中，以告知 Unreal 我们要使用该模块。该行应如下所示：

```cpp
PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore", "HeadMountedDisplay" });

```

7让我们修改**Pawn**的头文件（`VRPawn.h`位于**Source/<YourProjectName>**文件夹中）以包含我们需要进行激光笔传送的字段。首先，在文件的顶部，您会看到如下所示的文本：

```cpp
#pragma once



#include "CoreMinimal.h"

#include "GameFramework/Pawn.h"

#include "VRPawn.generated.h"


```

我们需要对其进行编辑，以便我们在代码中使用一些必要的组件（例如相机和运动控制器）。  
修改它，使其看起来像这样：

```cpp
#pragma once



#include <Camera/CameraComponent.h>

#include <Components/SceneComponent.h>

#include <Components/StaticMeshComponent.h>

#include <CoreMinimal.h>

#include <GameFramework/Pawn.h>

#include <MotionControllerComponent.h>



#include "VRPawn.generated.h"


```

8再往下`VRPawn.h`，您会看到标有 的部分`protected`。在该部分中，添加这些组件，它们将作为物理耳机和控制器的虚拟对应物、用于指示用户指向位置的光标以及用于指向和传送的一些参数：

```cpp
  UMotionControllerComponent* LeftController;

  UMotionControllerComponent* RightController;

  UStaticMeshComponent* LineTraceCursor;

  float LineTraceLength = 1000000;

  float OffsetFromGround = 4000;


```

9我们需要用`UPROPERTY`宏标记这些字段，以便我们可以在蓝图编辑器中访问它们。我们可以通过添加`UPROPERTY`带有几个说明符的宏来做到这一点。添加宏，使最终结果如下所示：

```cpp
  UPROPERTY(BlueprintReadWrite, EditAnywhere)

  UMotionControllerComponent* LeftController;



  UPROPERTY(BlueprintReadWrite, EditAnywhere)

  UMotionControllerComponent* RightController;



  UPROPERTY(BlueprintReadWrite, EditAnywhere)

  UStaticMeshComponent* LineTraceCursor;



  UPROPERTY(BlueprintReadWrite, EditAnywhere)

  float LineTraceLength = 1000000;



  UPROPERTY(BlueprintReadWrite, EditAnywhere)

  float OffsetFromGround = 4000;


```

Information

如果您需要在上下文中查看任何代码片段，或者如果您根本不需要学习如何在 Unreal 中创建**Pawn**，请跳至本教程的底部。那里显示了完整的 C++ 代码。

10在文件底部（右大括号之前）添加另一个部分，标记为`private`，后跟一个冒号 ( `:`)。您在本节中声明的变量和函数将无法被其他类访问，如果其他类不需要看到它们，这是一个很好的做法。

11声明这些函数，这将允许用户指向一个位置并传送到它：

```cpp
private:

  void _startLineTracing();

  void _stopLineTracing();

  void _teleport();


```

12`private`在该部分中也添加以下字段。即使我们不需要在蓝图编辑器中修改`_root`或`_camera`，用宏标记它们仍然是一个很好的做法，`UPROPERTY`这样虚幻引擎就可以正确地跟踪它们。

```cpp
  UPROPERTY()

  USceneComponent* _root;



  UPROPERTY()

  UCameraComponent* _camera;



  bool _isLineTracing;

  bool _isLineTraceHitting;

  FVector _lineTraceHitLocation;


```

13现在，打开`VRPawn.cpp`。已经为我们添加了一个构造函数，但我们需要添加一些代码来初始化我们的组件。将以下行添加到构造函数（以 开头的函数`AVRPawn::AVRPawn()`）：

```cpp
  _root = CreateDefaultSubobject<USceneComponent>(TEXT("Root"));

  _camera = CreateDefaultSubobject<UCameraComponent>(TEXT("Camera"));

  LeftController = CreateDefaultSubobject<UMotionControllerComponent>(TEXT("LeftController"));

  RightController = CreateDefaultSubobject<UMotionControllerComponent>(TEXT("RightController"));

  LineTraceCursor = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("LineTraceCursor"));


```

14现在，当我们的**Pawn**被构建时，组件也将被构建。我们还需要在转换层次结构中定义它们之间的关系。在刚刚添加的代码下方，插入以下内容：

```cpp
  RootComponent = _root;

  _camera->SetupAttachment(_root);

  LeftController->SetupAttachment(_root);

  RightController->SetupAttachment(_root);

  LineTraceCursor->SetupAttachment(_root);


```

15此外，我们需要确保`UMotionController`组件跟踪 VR 设备物理控制器的位置和方向，并且它们在 VR 中可见。将这些行添加到构造函数中：

```cpp
  LeftController->MotionSource = FName("Left");

  RightController->MotionSource = FName("Right");

  LeftController->bDisplayDeviceModel = true;

  RightController->bDisplayDeviceModel = true;


```

16最后，让我们在构建**Pawn**时隐藏线迹光标，这样它就不会挡路。添加此行：

```
  LineTraceCursor->SetVisibility(false);
```

17让我们为在头文件中声明的两个新函数添加定义。这些本质上只是跟踪我们映射到线跟踪操作的控制器按钮是否被按下。松开按钮后，隐藏线迹光标（我们的激光笔的点）。

```cpp
void AVRPawn::_startLineTracing() { _isLineTracing = true; }



void AVRPawn::_stopLineTracing() {

  _isLineTracing = false;

  _isLineTraceHitting = false;

  LineTraceCursor->SetVisibility(false);

}


```

18对于我们的激光指示器，我们需要指定每一帧发生的情况，这取决于指示器是否打开——以及它是否击中了任何东西。我们在`Tick`函数中这样做。下面的代码处理这个。

```cpp
void AVRPawn::Tick(float deltaTime) {

  Super::Tick(deltaTime);



  // If the user is pressing the line trace button, do the line trace.

  if (_isLineTracing) {

    // Use the right controller to aim the line trace.

    FVector start = RightController->GetComponentLocation();

    FVector direction = RightController->GetForwardVector();

    FVector end = start + (direction * LineTraceLength);



    ECollisionChannel channel = ECollisionChannel::ECC_WorldStatic;

    FCollisionQueryParams params(FName(TEXT("")), true, this);

    FHitResult hit;



    if (GetWorld()->LineTraceSingleByChannel(OUT hit, start, end, channel, params)) {

      // If the line trace hit something, move the cursor to that spot.

      _isLineTraceHitting = true;

      _lineTraceHitLocation = hit.Location;

      LineTraceCursor->SetVisibility(true);

      LineTraceCursor->SetWorldLocation(_lineTraceHitLocation);

    } else {

      // If the line trace didn't hit anything, hide the cursor.

      _isLineTraceHitting = false;

      LineTraceCursor->SetVisibility(false);

    }

  }

}


```

19现在，让我们添加处理传送的函数。首先在 的底部添加一个空定义`VRPawn.cpp`：

```cpp
void AVRPawn::_teleport() {



}


```

20如果满足以下条件，我们只希望用户能够传送：(a) 他们的激光指示器打开并且 (b) 激光指示器击中地形上的一个点。下面的代码检查这两个条件，如果其中一个为假，函数将返回而不做任何事情。将此代码添加到函数的顶部：

```cpp
  if (!_isLineTracing || !_isLineTraceHitting) {

    return;

  }


```

21如果我们通过了这两项测试，我们就可以将用户传送到`_lineTraceHitLocation`——有一个警告。我们不想将它们传送到激光笔击中地面的确切位置，因为那样它们的视线就会与地面平齐。相反，让我们添加一个垂直偏移量，让用户离地面一定距离：

```cpp
  FVector newLocation = _lineTraceHitLocation;

  newLocation.Z += OffsetFromGround;

  SetActorLocation(newLocation);


```

22最后，我们需要修改`SetupPlayerInputComponent`函数，使其看起来像下面的代码。这会将我们的函数连接到我们在本教程第一部分中映射的`LineTrace`和`Teleport` **动作事件**，这些事件将由我们的 VR 设备的物理按钮触发。

```cpp
void AVRPawn::SetupPlayerInputComponent(UInputComponent* playerInputComponent) {

  Super::SetupPlayerInputComponent(playerInputComponent);



  InputComponent->BindAction("LineTrace", IE_Pressed, this, &AVRPawn::_startLineTracing);

  InputComponent->BindAction("LineTrace", IE_Released, this, &AVRPawn::_stopLineTracing);

  InputComponent->BindAction("Teleport", IE_Pressed, this, &AVRPawn::_teleport);

}


```

23单击**“文件”>“全部保存”**以保存我们刚刚修改的文件。

25返回虚幻编辑器。如果您在关卡预览窗口中没有看到 Cesium World Terrain，请关闭并重新打开编辑器。

26通过在屏幕底部的**内容浏览器中单击****添加/导入**来添加新的蓝图类。在**创建基本资产**下，单击**蓝图类**。

![](https://images.prismic.io/cesium/23d19950-995f-47fb-8fe2-69ffda19832b_CesiumVRTutorial_120.png?auto=compress%2Cformat&w=425)

27展开**所有类**并搜索“VRPawn”以找到您在 C++ 中创建的类。单击它并按**选择**。

![](https://images.prismic.io/cesium/6e31151f-2cad-4ec5-8e92-63dcad547195_CesiumVRTutorial_130.png?auto=compress%2Cformat&w=680)

29双击**BP\_VRPawn**在编辑器中将其打开。如果您看到文本**Open Full Blueprint Editor**，请单击它。

![](https://images.prismic.io/cesium/40cae193-cb17-4fab-8ccd-4ba3a4a0a745_CesiumVRTutorial_140.png?auto=compress%2Cformat&w=509)

您已经成功创建了**Pawn**的核心。现在，继续第 3 步。

## 2b：[蓝图](https://cesium.com/learn/unreal/unreal-building-vr/#2b-blueprint)

如果您不想使用蓝图事件图而是使用 C++ 代码，请转至上面的**步骤 2a**。

1通过在屏幕底部的**内容浏览器中单击****添加/导入**来添加一个新的蓝图类。在**创建基本资产**下，单击**蓝图类**。

![](https://images.prismic.io/cesium/23d19950-995f-47fb-8fe2-69ffda19832b_CesiumVRTutorial_120.png?auto=compress%2Cformat&w=425)

![](https://images.prismic.io/cesium/b058023e-3ed6-476b-9e54-1eacf5a8e439_CesiumVRTutorial_150.png?auto=compress%2Cformat&w=680)

5单击左上角的**添加组件按钮并搜索****相机**组件。单击它以将其添加到您的**Pawn**。给它起个名字（例如，“相机”）。

![](https://images.prismic.io/cesium/7bfb802d-880f-4c53-a186-5ab8af732559_CesiumVRTutorial_160.png?auto=compress%2Cformat&w=345)

6使用**添加组件**按钮添加两个**运动控制器**组件。给它们命名（例如，“LeftController”和“RightController”）。

![](https://images.prismic.io/cesium/efb3419c-77dc-4c48-95c7-d8098285e7f2_CesiumVRTutorial_170.png?auto=compress%2Cformat&w=345)

7使用**Add Component**按钮添加一个**静态网格**体组件，它将代表我们的激光笔的点。给它起一个名字（例如，“LineTraceCursor”）。

![](https://images.prismic.io/cesium/5264fd5b-01f9-404c-9274-7553f04816aa_CesiumVRTutorial_180.png?auto=compress%2Cformat&w=339)

8确保您添加的所有四个组件都是**DefaultSceneRoot**的直接子级。如果它们中的任何一个被添加为彼此的子项，请单击并将它们拖到**DefaultSceneRoot**上，然后单击**Attach**。

![](https://images.prismic.io/cesium/264e18ab-ff94-407a-9f15-b4e8a52b172f_CesiumVRTutorial_190.gif?auto=compress%2Cformat&w=337)

![](https://images.prismic.io/cesium/b8988954-aebf-4904-bd9f-afd8ddc913aa_CesiumVRTutorial_200.png?auto=compress%2Cformat&w=419)

9在**Components**窗口中选择**LeftController**，然后查看Editor 右侧的**Details选项卡。**向下滚动以找到**Visualization部分并选中****Display Device Model**旁边的框。还要找到**Motion Controller**部分并确保**Motion Source**值设置为**Left**。

![](https://images.prismic.io/cesium/b1e883c4-2dda-4dbc-a4db-ae217feb852d_CesiumVRTutorial_210.png?auto=compress%2Cformat&w=558)

10对**RightController**重复上述步骤。对于这一个，将**Motion Source**值设置为**Right**。

![](https://images.prismic.io/cesium/4e1b0e66-8859-4b02-bbee-5b292e18ed6d_CesiumVRTutorial_220.png?auto=compress%2Cformat&w=558)

11在左下角，找到**Variables**旁边的**+**符号。当您将光标悬停在它上面时，它会变成黄色，并且会出现**Variable一词。**单击五次以添加五个新变量。

![](https://images.prismic.io/cesium/04a4e5b2-bff9-4062-b420-f8d4954f4ace_CesiumVRTutorial_230.png?auto=compress%2Cformat&w=397)

12依次单击每个新变量，并在选中时查看右侧的**Details面板。**如下图所示设置它们。**在设置每个Default Value**之前，您需要单击左上角的**Compile**按钮。

![](https://images.prismic.io/cesium/3f208e6d-32bd-44d8-9d18-1fcc32bedd40_CesiumVRTutorial_240.png?auto=compress%2Cformat&w=946)

![](https://images.prismic.io/cesium/d88821c2-3ad2-4047-8d0f-dc09261c99e9_CesiumVRTutorial_250.png?auto=compress%2Cformat&w=946)

![](https://images.prismic.io/cesium/ccefc4df-53d1-4351-b299-f4bfb5858614_CesiumVRTutorial_260.png?auto=compress%2Cformat&w=473)

![](https://images.prismic.io/cesium/25ed5ac2-56ed-4fc9-951a-086be3db4aaf_CesiumVRTutorial_270.png?auto=compress%2Cformat&w=715)

14右键单击**事件图表**中的任意位置并搜索我们在**项目设置中创建的第一个****动作事件**——名为**LineTrace**的事件。点击它。将出现一个节点。

![](https://images.prismic.io/cesium/24a82125-df67-4bee-bd31-15ac956a8119_CesiumVRTutorial_290.png?auto=compress%2Cformat&w=513)

15对我们创建的第二个**动作事件**执行相同的操作\- 名为**Teleport**的事件。

![](https://images.prismic.io/cesium/c47c569c-6dae-4735-99f6-9d0a4d42f5dc_CesiumVRTutorial_300.png?auto=compress%2Cformat&w=513)

16现在图形应该具有我们需要的三个事件节点：**Event Tick**、**InputAction LineTrace**和**InputAction Teleport**。

17添加并连接节点（在**事件图表**中右键单击并搜索每个节点）以使从**Event Tick**节点流出的图表如下图所示。  

当您需要访问一个组件或变量（例如，**LineTraceCursor**）时，您可以正常搜索它或将它从左下角的**变量部分拖到****事件图中。**选择“获取”选项，除非屏幕截图中的节点标记为**SET**。

该图检查用户是否正在激活他们的激光指示器。如果是，它将点移动到他们指向的地形上的点。

![](https://prismic-io.s3.amazonaws.com/cesium/ba1125f6-94d4-4da2-9218-482ea0353991_CesiumVRTutorial_320.png)

使用**Break**节点访问**LineTraceByChannel**节点的**Out Hit**输出的**Location**值。

![](https://prismic-io.s3.amazonaws.com/cesium/a99b9710-2e9f-42fe-b5e7-ec63d06d9f8f_CesiumVRTutorial_325.gif)

18设置连接到**InputAction LineTrace**节点的图形，如下图所示。该图跟踪激光笔是打开还是关闭，具体取决于用户何时按下和释放与其关联的按钮。

![](https://images.prismic.io/cesium/38f3ae6d-801f-4940-829b-4f4a3ef66eaa_CesiumVRTutorial_330.png?auto=compress%2Cformat&w=831)

19设置连接到**InputAction Teleport**节点的图表，如下图所示。此图将用户移动到激光指示器的点 - 如果指示器打开并对准地面。

![](https://images.prismic.io/cesium/a0c176b2-067f-48bb-966a-d3394b4c6506_CesiumVRTutorial_340.png?auto=compress%2Cformat&w=580)

![](https://images.prismic.io/cesium/8b7c7905-9527-4389-8de7-e7f87e550ca5_CesiumVRTutorial_345.png?auto=compress%2Cformat&w=715)

21设置连接到**构造脚本**节点的图形，如下图所示。此图会在构建**Pawn时隐藏线迹光标，在需要时将其移开。**

![](https://images.prismic.io/cesium/8a99f59d-1631-447e-9ea5-00d82c04daee_CesiumVRTutorial_347.png?auto=compress%2Cformat&w=401)

![](https://images.prismic.io/cesium/299f2719-da04-4291-9453-fa08d4657b7f_CesiumVRTutorial_350.png?auto=compress%2Cformat&w=202)

## 3[配置线迹光标](https://cesium.com/learn/unreal/unreal-building-vr/#step-3-configure-the-line-trace-cursor)

无论您执行的是**步骤 2a**还是**步骤 2b** ，都请继续此处。

这些步骤将配置线迹光标的外观（即激光笔的点）。本教程使用内置网格和材质，但您可以随意创建自己的资产或根据需要调整这些设置。

1在编辑器中打开**BP\_VRPawn**后，单击**组件**列表中的**静态网格**体组件（即**LineTraceCursor**）。

2选择**Static** Mesh，在右侧的**Details**面板中找到**Static Mesh**部分。点击下拉菜单，点击**View Options**，然后确保选中**Show Engine Content**旁边的复选框。如果不是，请检查它。

![](https://images.prismic.io/cesium/cc17a4f2-8cce-46e0-80c6-0f5ee1fbc96a_CesiumVRTutorial_360.png?auto=compress%2Cformat&w=474)

3在**搜索资源**框中，搜索 1x1x1 球体。有多个球体，因此请确保您拥有正确的球体。

![](https://images.prismic.io/cesium/4bc2b7c1-df8a-4203-9ddf-f1544433bd70_CesiumVRTutorial_370.png?auto=compress%2Cformat&w=474)

4在**Details**面板中找到**Transform**部分并将**Scale**设置为 (500.0, 500.0, 500.0)。

![](https://images.prismic.io/cesium/1675b4fe-60dc-494f-8df2-9e49143331aa_CesiumVRTutorial_380.png?auto=compress%2Cformat&w=454)

5在**Details**面板中找到**Materials**部分并将**Element 0**设置为**WidgetMaterial\_Current**。（如果您愿意，请随意使用您自己的材料。）

![](https://images.prismic.io/cesium/a77eafde-4018-4e76-b443-06cb6d6b3476_CesiumVRTutorial_390.png?auto=compress%2Cformat&w=474)

## 4[整理和测试](https://cesium.com/learn/unreal/unreal-building-vr/#step-4-finishing-and-testing)

1在 BP\_VRPawn 的**Details**面板中，搜索**Auto** **Possess** Player并将其设置为**Player 0**。

![](https://images.prismic.io/cesium/bb0b528f-449d-4f9c-a856-a3d9fb3e1710_CesiumVRTutorial_400.png?auto=compress%2Cformat&w=474)

4将**BP\_VRPawn**拖入关卡并将其移动到您选择的起始位置。

5正如您在本教程的第一部分所做的那样，单击“**播放”**按钮旁边的小箭头或工具栏右侧的双箭头（工具提示 =**单击以展开工具栏**），然后选择**“活动播放模式”**。

6选择**VR 预览**。戴上您的 VR 耳机。你应该能够环顾你的关卡并看到你的控制器。

7如果您按下绑定到**LineTrace**操作的按钮（**右触发器**，如果您完全按照这些步骤操作），您应该能够使用右控制器指向地形并在您指向的位置看到一个球体。

8按住**LineTrace**按钮后，您应该能够按下绑定到**Teleport**操作的按钮（如果您完全按照步骤操作，则为**A**按钮）移动到所选位置正上方的位置。

![](https://images.prismic.io/cesium/034ed67b-2dcf-4b72-a842-b4cb934b100d_CesiumVRTutorial_405.gif?auto=compress%2Cformat&w=400)

## 5[作为独立应用程序部署](https://cesium.com/learn/unreal/unreal-building-vr/#step-5-deploying-as-a-standalone-application)

前面的步骤适用于任何系留 VR 设备。唯一因设备而异的是**动作映射**和使 Unreal 识别您的设备已插入的过程（例如，Oculus 应用、SteamVR 或其他）。

下一步是更特定于设备的。目前，唯一受支持的独立设备是 Oculus Quest 2。随着未来添加对其他独立 VR 设备的支持，此页面将更新每个设备的说明。

## [为 Oculus Quest 2 部署](https://cesium.com/learn/unreal/unreal-building-vr/#deploying-for-oculus-quest-2)

为了将应用程序的独立 (Android) 版本部署到 Oculus Quest 2，需要进行两种类型的调整。首先，Unreal 需要配置为针对 Android 构建。其次，需要调整 tileset 设置以使其更适合移动设备。

1确保您拥有最新版本的 Cesium for Unreal。要查找更新程序，请打开**Epic Games Launcher**并转到“**库**”选项卡。在您用于本教程的 Unreal 版本下方，单击**Installed Plugins**。

2按照[这些说明](https://developer.oculus.com/documentation/unreal/unreal-quick-start-guide-quest/)为 Oculus 开发设置 Unreal。由于您已经安装了虚幻引擎，请从名为“为 Android 开发设置虚幻引擎”的部分开始。跳过“创建项目”部分，然后使用您在本教程中使用的项目继续“为 Oculus 开发配置项目”部分。

3在**项目设置**中，转到**平台-Android > 构建**。选中**Support arm64**并取消选中**Support armv7**。

4在**Unreal Editor**中，点击右侧**World Outliner**中的**Cesium World Terrain 。**

5在**World Outliner下方的****Details**面板中，找到**Cesium > Level of Detail**并将其展开。将**Maximum Screen Space Error**设置为一个较大的数字，例如 256。这将使 tileset 对于移动设备更易于管理。

![](https://images.prismic.io/cesium/4bd7b946-21fa-428a-ae5a-f02febe09754_CesiumVRTutorial_410.png?auto=compress%2Cformat&w=407)

6在虚幻编辑器的顶部工具栏中，您可能会看到旁边有一个小箭头的**启动按钮。**如果它在那里，请单击小箭头。如果不存在，请单击工具栏右侧的双箭头（工具提示 =**单击以展开工具栏**），然后选择**启动选项**。

7在弹出的子菜单中，您应该看到“Quest\_2”后跟一些字母和数字。如果您没有立即看到它，请等待几秒钟，然后重复上一步。一旦 Unreal 检测到您的 Quest，它就会出现。

![](https://images.prismic.io/cesium/05357c66-891a-4eb0-83ca-608c5b52cce8_CesiumVRTutorial_420.png?auto=compress%2Cformat&w=675)

8看到“Quest\_2”后，单击它以将应用程序部署到 Quest 并启动它。您可以打开输出日志（**Window > Developer Tools > Output Log**）来跟踪进度。这将需要几分钟时间，Unreal 可能会在开始时冻结。部署完成后，您将看到文本“LogPlayLevel: Starting: Intent”和“LogTcpMessaging: Discovered node”。

9该过程完成后，您应该可以像在**VR 预览**中一样戴上耳机并使用它。

## [完整的源代码](https://cesium.com/learn/unreal/unreal-building-vr/#complete-source-code)

#### [VRPawn.h:](https://cesium.com/learn/unreal/unreal-building-vr/#vrpawnh)

注意：在此文件的顶部，`CESIUMVRTUTORIAL_API`应该是`YOUR-PROJECT-NAME-IN-ALL-CAPS_API`.

```c++
#pragma once



#include <Camera/CameraComponent.h>

#include <Components/SceneComponent.h>

#include <Components/StaticMeshComponent.h>

#include <CoreMinimal.h>

#include <GameFramework/Pawn.h>

#include <MotionControllerComponent.h>



#include "VRPawn.generated.h"



UCLASS()

class CESIUMVRTUTORIAL_API AVRPawn : public APawn {

  GENERATED_BODY()



public:

  AVRPawn();



protected:

  virtual void BeginPlay() override;



  UPROPERTY(BlueprintReadWrite, EditAnywhere)

  UMotionControllerComponent* LeftController;



  UPROPERTY(BlueprintReadWrite, EditAnywhere)

  UMotionControllerComponent* RightController;



  UPROPERTY(BlueprintReadWrite, EditAnywhere)

  UStaticMeshComponent* LineTraceCursor;



  UPROPERTY(BlueprintReadWrite, EditAnywhere)

  float LineTraceLength = 1000000;



  UPROPERTY(BlueprintReadWrite, EditAnywhere)

  float OffsetFromGround = 4000;



public:

  virtual void Tick(float deltaTime) override;



  virtual void SetupPlayerInputComponent(class UInputComponent* playerInputComponent) override;



private:

  void _startLineTracing();

  void _stopLineTracing();

  void _teleport();



  UPROPERTY()

  USceneComponent* _root;



  UPROPERTY()

  UCameraComponent* _camera;



  bool _isLineTracing;

  bool _isLineTraceHitting;

  FVector _lineTraceHitLocation;

};


```

#### [VRPawn.cpp:](https://cesium.com/learn/unreal/unreal-building-vr/#vrpawncpp)

```c++
#include "VRPawn.h"



AVRPawn::AVRPawn() {

  PrimaryActorTick.bCanEverTick = true;



  _root = CreateDefaultSubobject<USceneComponent>(TEXT("Root"));

  _camera = CreateDefaultSubobject<UCameraComponent>(TEXT("Camera"));

  LeftController = CreateDefaultSubobject<UMotionControllerComponent>(TEXT("LeftController"));

  RightController = CreateDefaultSubobject<UMotionControllerComponent>(TEXT("RightController"));

  LineTraceCursor = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("LineTraceCursor"));



  RootComponent = _root;

  _camera->SetupAttachment(_root);

  LeftController->SetupAttachment(_root);

  RightController->SetupAttachment(_root);

  LineTraceCursor->SetupAttachment(_root);



  LeftController->MotionSource = FName("Left");

  RightController->MotionSource = FName("Right");

  LeftController->bDisplayDeviceModel = true;

  RightController->bDisplayDeviceModel = true;



  LineTraceCursor->SetVisibility(false);

}



void AVRPawn::BeginPlay() { Super::BeginPlay(); }



void AVRPawn::Tick(float deltaTime) {

  Super::Tick(deltaTime);



  // If the user is pressing the line trace button, do the line trace.

  if (_isLineTracing) {

    // Use the right controller to aim the line trace.

    FVector start = RightController->GetComponentLocation();

    FVector direction = RightController->GetForwardVector();

    FVector end = start + (direction * LineTraceLength);



    ECollisionChannel channel = ECollisionChannel::ECC_WorldStatic;

    FCollisionQueryParams params(FName(TEXT("")), true, this);

    FHitResult hit;



    if (GetWorld()->LineTraceSingleByChannel(OUT hit, start, end, channel, params)) {

      // If the line trace hit something, move the cursor to that spot.

      _isLineTraceHitting = true;

      _lineTraceHitLocation = hit.Location;

      LineTraceCursor->SetVisibility(true);

      LineTraceCursor->SetWorldLocation(_lineTraceHitLocation);

    } else {

      // If the line trace didn't hit anything, hide the cursor.

      _isLineTraceHitting = false;

      LineTraceCursor->SetVisibility(false);

    }

  }

}



void AVRPawn::SetupPlayerInputComponent(UInputComponent* playerInputComponent) {

  Super::SetupPlayerInputComponent(playerInputComponent);



  InputComponent->BindAction("LineTrace", IE_Pressed, this, &AVRPawn::_startLineTracing);

  InputComponent->BindAction("LineTrace", IE_Released, this, &AVRPawn::_stopLineTracing);

  InputComponent->BindAction("Teleport", IE_Pressed, this, &AVRPawn::_teleport);

}



void AVRPawn::_startLineTracing() { _isLineTracing = true; }



void AVRPawn::_stopLineTracing() {

  _isLineTracing = false;

  _isLineTraceHitting = false;

  LineTraceCursor->SetVisibility(false);

}



void AVRPawn::_teleport() {

  if (!_isLineTracing || !_isLineTraceHitting) {

    return;

  }



  FVector newLocation = _lineTraceHitLocation;

  newLocation.Z += OffsetFromGround;

  SetActorLocation(newLocation);

}
```

## [下一步](https://cesium.com/learn/unreal/unreal-building-vr/#next-steps)

现在您可以添加自己的图块集并使用我们创建的**Pawn**在 VR 中探索它们。请注意，您可以修改**Pawn**`OffsetFromGround`的参数来更改用户的高度。

我们很乐意收到您对本教程的反馈。[请在Cesium 社区论坛](https://community.cesium.com/t/preview-the-upcoming-cesium-for-unreal-vr-tutorial/14813https://)上分享您的建议或报告问题。[](https://community.cesium.com/t/preview-the-upcoming-cesium-for-unreal-vr-tutorial/14813https://)