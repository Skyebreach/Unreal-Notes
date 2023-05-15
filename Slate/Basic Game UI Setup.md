# Slate Setup
Live coding should be turned off when working with slate (and in general imo) as it can cause bugs, editor crashes, and can cause the ui elements to not be hot reloaded.  To do this follow `Edit > Editor Preferences > Live Coding` and make sure the "Enable Live Coding" button is turned off as such
![Live Coding Turn off](../Imgs/LiveCodingTurnOff.png)
Before you can work with Slate UI you must add the required dependancies,
```CSharp
	PrivateDependencyModuleNames.AddRange(new string[] {  
		"Slate",  
		"SlateCore"  
	});
```

Your build file may already include one or both of these, if this is the case only add the packages that are absent. 

# Basic Widget Setup
Here we will use the third person example project as a back bone for our hello world widget

## Creating your first slate class
First we will create a Slate Widget through the editor called "HUDWidget". Note that slate widgets do not use unreals reflection and as such do not show in the editors content drawer. 

Once the class has been created change the headers to be,
```CPP
	#include "SlateBasics.h"  
	#include "SlateExtras.h"  
	#include "Widgets/Layout/SConstraintCanvas.h"
```
The first two are the most commonly used for slate ui widgets and the third is a special canvas type we will talk about later. Next we need to add in a slate argument, these are arguments a slate widget can use in the construction process. These arguments are talked about in more detail in [[Slate Arguments]]. For most apps a pointer to the owning hud is required and as such that is the only argument that is included here (it wont be used for this app but is good practice). The slate arguments should look like,
```CPP
	SLATE_BEGIN_ARGS(SHUDWidget){}  
	  
	/** HUD class that owns the menu */  
	SLATE_ARGUMENT(TWeakObjectPtr<class AMainHUD>, OwningHud)  
	  
	SLATE_END_ARGS()
```
Here a weak pointer to the main hud class is used as the type, this class will be created later. As the OwningHUD is a weak pointer it forward declares its type, for more information on weak pointers head to [[Unreal Smart Pointers]]. As we will be wanting to use the OwningHUD outside of the constructor we will also create a constant weak pointer to it,
```CPP
	public:  
		/** Weak pointer to the HUD class that owns the menu */  
		TWeakObjectPtr<class AMainHUD> OwningHUD;
```
Moving onto the .cpp file we will now edit the constructor functions. First we will initialise our OwningHUD variable from the slate arguments passed in,
```CPP
	OwningHUD = InArgs._OwningHud;
```
And declare a font variable to be used for the "Hello World" text,
```CPP
	FSlateFontInfo font = FCoreStyle::Get().GetFontStyle("EmbossedText"); 
	font.Size = 20.f;
```
Now that we have done the basic setup for our constructor will want to populate the widget to do this we create a child slot with a constraint canvas inside it,
```CPP
	ChildSlot  
	.HAlign(HAlign_Fill)  
	.VAlign(VAlign_Fill)  
	[  
		SNew(SConstraintCanvas)  
	];  
```
Child slots must contain a widget inside them, here we use a constraint canvas as it is a very useful type of canvas that allows for the anchoring of widgets based on the viewports location. We will not fully go through the H/VAligns here as they are just used to ensure that the childslot fills the space its given. The SNew here creates a new widget, there are other forms of this function but this is the simplist form. To populate the canvas with child widgets we need to add a slot,
```CPP
	ChildSlot  
	.HAlign(HAlign_Fill)  
	.VAlign(VAlign_Fill)  
	[  
		SNew(SConstraintCanvas)  
		// Top Left Screen Alignment  
		+ SConstraintCanvas::Slot()  
		.AutoSize(true)  
		.Anchors(FAnchors(0, 0))  
		.Alignment(FVector2D(0,0))   
	];  
```
Slots are only used in conjugtion with SPanels, and they allow for the extension of a widget to have multiple parts. Here we add a slot and define its arguments, AutoSize is used to force the slot to use its childrens size over its normal computed size. The anchor and alignment defines its position on the viewport with (0,0) being located at the top left and (1,1) being located at the bottom right. Every SPanel slot has its own childslot so we can add a text block into this slot,
```CPP
	ChildSlot  
	.HAlign(HAlign_Fill)  
	.VAlign(VAlign_Fill)  
	[  
		SNew(SConstraintCanvas)  
		// Top Left Screen Alignment  
		+ SConstraintCanvas::Slot()  
		.AutoSize(true)  
		.Anchors(FAnchors(0, 0))  
		.Alignment(FVector2D(0,0))   
		[  
			SNew(STextBlock)  
			.Text(FText::FromString("Hello World"))  
			.Justification(ETextJustify::Center)  
			.Font(font)  
			.ColorAndOpacity(FColor::Red)  
		]  
	];  
```
Here we create a new text block inside the slot that prints hello world on the screen. The text blocks arguments are self explanitory so wont be expanded on here.

## Full Code
The full code for this widget is shown here
**SHUDWidget.h**
```CPP
	#include "SlateBasics.h"  
	#include "SlateExtras.h"  
	  
	#include "Widgets/Layout/SConstraintCanvas.h"
	
	class MYPROJECT_API SHUDWidget : public SCompoundWidget  
	{  
	public:  
		SLATE_BEGIN_ARGS(SHUDWidget){}  
		  
			/** HUD class that owns the menu */  
			SLATE_ARGUMENT(TWeakObjectPtr<class AMainHUD>, OwningHud)  
			
		SLATE_END_ARGS()  
	  
		/** Constructs this widget with InArgs */  
		void Construct(const FArguments& InArgs);  
	  
	public:  
		/** Weak pointer to the HUD class that owns the menu */  
		TWeakObjectPtr<class AMainHUD> OwningHud;
	};
```

**SHUDWidget.cpp**
```CPP
	#include "SHUDWidget.h"  
	#include "SlateOptMacros.h"

	BEGIN_SLATE_FUNCTION_BUILD_OPTIMIZATION  
	void SHUDWidget::Construct(const FArguments& InArgs)  
	{  
		OwningHUD = InArgs._OwningHud;  
		  
		FSlateFontInfo font = FCoreStyle::Get().GetFontStyle("EmbossedText");  
		font.Size = 20.f;  
		  
		ChildSlot  
		.HAlign(HAlign_Fill)  
		.VAlign(VAlign_Fill)  
		[  
			SNew(SConstraintCanvas)  
			// Top Left Screen Alignment  
			+ SConstraintCanvas::Slot()  
			.AutoSize(true)  
			.Anchors(FAnchors(0, 0))  
			.Alignment(FVector2D(0,0))  
			[  
				SNew(STextBlock)  
				.Text(FText::FromString("Hello World"))  
				.Justification(ETextJustify::Center)  
				.Font(font)  
				.ColorAndOpacity(FColor::Red)  
			]  
		];  
	}
	END_SLATE_FUNCTION_BUILD_OPTIMIZATION
```


## Structure

 
# HUD Setup
## Creating the HUD Class
- Creating the hud class
- Setting HUD Class as the defualt class
- Widget Ptr
- Container Ptr
- Creation of widget
- Assignment to viewport
- Input Mode
To Use slate within your game a HUD class is first required. HUD classes 

## Full Code
**AMainHUD.h**
```CPP	 
	#include "CoreMinimal.h"  
	#include "GameFramework/HUD.h"  
	  
	#include "SHUDWidget.h"  
	  
	#include "MainHUD.generated.h"

	UCLASS()  
	class MYPROJECT_API AMainHUD : public AHUD  
	{  
		GENERATED_BODY()  
	public:	  
		AMainHUD();
		
	protected:  
		virtual void BeginPlay() override;

	public:
		void DisplayMainHud();

	protected:  
		TSharedPtr<class SHUDWidget> HUDWidget;  
		TSharedPtr<class SWidget> HUDWidgetContainer;
	};
```

**AMainHUD.cpp**
```CPP
	#include "MainHUD.h"

	AMainHUD::AMainHUD() {}

	void AMainHUD::BeginPlay()  
	{  
		DisplayMainHud() 
	}

	void AMainHUD::DisplayMainHud() 
	{
		if (GEngine && GEngine->GameViewport)  
		{
			HUDWidget = SNew(SHUDWidget).OwningHud(this);  
			  
			GEngine->GameViewport->AddViewportWidgetContent(SAssignNew(HUDWidgetContainer, SWeakWidget).PossiblyNullContent(HUDWidget.ToSharedRef()));  
			  
			if (PlayerOwner)  
				PlayerOwner->SetInputMode(FInputModeGameAndUI());
		}
	}
```