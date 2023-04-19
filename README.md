# Menus

## Preparation
- in MenuButtons.build.cs:
  - Include "UMG" inside public dependencies
  - Uncomment where it sayas "incomment if using Slate UI"
  
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
                                    //TemplateFunction<widget type>(context, class of the widget I want to create)
        UUserWidget* CurrentWidget = CreateWidget<UUserWidget>(GetWorld(), MenuWidgetClass);

        if (CurrentWidget != nullptr)
        {
            //Display widget in my game by adding it to the viewport   
            CurrentWidget->AddToViewport();
        }
    }
}
```
 
