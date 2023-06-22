# Menus

## Preparation
- in MenuButtons.build.cs:
  - Include "UMG" inside PublicDependenciesModuleNames
  - Uncomment where it sayas "incomment if using Slate UI"
  - Create a custom PlayerController C++ class and a GameModeBase C++ class and convert them into blueprints
  
- GameModeBase.h
  - #include "Blueprint/UserWidget.h"
  - declare BeginPlay()
  - declare a class instance of a UUserWidget and expose it with UPROPERTY
  
```cpp
#include "Blueprint/UserWidget.h"
#include "CoreMinimal.h"
#include "GameFramework/GameModeBase.h"
#include "MenuButtonsGameModeBase.generated.h"

UCLASS()
class MENUBUTTONS_API AMenuButtonsGameModeBase : public AGameModeBase
{
	GENERATED_BODY()

protected:
	virtual void BeginPlay() override;

	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "MyWidget")
	TSubclassOf<UUserWidget> MenuWidgetClass;
};
```

- GameModeBase.cpp
  - On BeginPlay()
    - Create widget
    - Show widget (by adding it to the game viewport)
    
```cpp
void AMenuButtonsGameModeBase::BeginPlay()
{
    Super::BeginPlay();

    if (MenuWidgetClass != nullptr)
    {
        //Create the widget and store it inside a pointer to a UUserWidget object
                                    //TemplateFunction<widget class>(context, widget instance)
        UUserWidget* CurrentWidget = CreateWidget<UUserWidget>(GetWorld(), MenuWidgetClass);

        if (CurrentWidget != nullptr)
        {
            //Display widget in my game by adding it to the viewport   
            CurrentWidget->AddToViewport();
        }
    }
}
```
 
- PlayerController.h
  - Declare BeginPlay()
  - Declare a function to print the message to the log and expose it with UFUNCTION so that it can be called from a BP

```cpp
UCLASS()
class MENUBUTTONS_API AMenuButtonsPlayerController : public APlayerController
{
	GENERATED_BODY()

public:

	virtual void BeginPlay() override;

	//A function to print the message to the log window and gets called from a BP
	UFUNCTION(BlueprintCallable)
	void Speak();
};
```

- PlayerController.cpp
  - Allow player to interact with the screen / UI
  - Allow player to see mouse coursor
  - Print Message

```cpp
void AMenuButtonsPlayerController::BeginPlay()
{
    Super::BeginPlay();

    //Allow player to interact with the screen (UI) to click on buttons on the screen
    SetInputMode(FInputModeGameAndUI());

    //Allow player to see mouse coursor on the screen
    bShowMouseCursor = true;
}

void AMenuButtonsPlayerController::Speak()
{
    UE_LOG(LogTemp, Warning, TEXT("Hi!!!"));
}
```

- in Unreal Engine
  - Create a folder for Menues, include an image for the pressed and unpressed buttons
  - Create a WidgetBlueprint: Right click > UI > WidgetBlueprint
    - Add a button, resize and ancor, change the collors for when normal, hovered and pressed
  - On project settings > maps and modes: replace game mode base to my BP_MenuButtonsGameModeBase and my player controller with my BP_MenuButtonsPlayerController.
  - In BP_MenuButtonsGameModeBase select the Menu Widget in My Widget component

## Buttons functionality
- In my MenuWidget on buttons event, select OnClick event
  - In the OnClick Event graph
    - Get PlayerController > return an object and Cast it to BP_MenuButtonsPlayerControllerType (to use my custom player controller) / Execute OnClicked > call Speak with BP_MenuButtonsPlayerController as Target 





















