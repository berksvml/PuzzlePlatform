# PuzzlePlatform
In this project I learned Has Authority codes in unreal engine 5 
Has authority ile yazdığımız nesne,cisim vb. kodlarda mantık şu şekilde işliyor ana sunucunun bilgisayarında o cisim ne ise var olarak bulunuyor kodlarında belirttiğimiz şekilde hareketlerini yapıyor ama diğer pcde o cisim bulunmuyor veya cisim olarak gözükmüyor ve ya hareketlerini yapmıyor.

Moving Platform.cpp:

#include "MovingPlatform.h"
AMovingPlatform::AMovingPlatform()
{
PrimaryActorTick.bCanEverTick = true;

SetMobility(EComponentMobility::Movable);


}
void AMovingPlatform::Tick(float DeltaTime)
{
Super::Tick(DeltaTime);

if (HasAuthority()) {
	FVector Location = GetActorLocation();
	Location += FVector(Speed * DeltaTime, 0, 0);
	SetActorLocation(Location);
}


}

Moving Platform.h:

#pragma once
#include "CoreMinimal.h"
#include "Engine/StaticMeshActor.h"
#include "MovingPlatform.generated.h"
/**
*
*/
UCLASS()
class PUZZLEPLATFORMS_API AMovingPlatform : public AStaticMeshActor
{
GENERATED_BODY()
public:
AMovingPlatform();

virtual void Tick(float DeltaTime) override;

UPROPERTY(EditAnywhere)
	float Speed = 20;


};

Bunlardan sonra o moving plarform adını verdiğim cisim aynen şöyle gözükecek:
Ana sunucu bilgisayarında var ve hareket ediyor:
Ama oyuncuda ise sadece var bir işlevi yok
Kodlarda biraz değişiklik yapalım ve ana serverda olan değişikliği oyuncu bilgisayarına da yansıtalım.

void AMovingPlatform::BeginPlay()
{
Super::BeginPlay();
if (HasAuthority()) {
SetReplicates(true);
SetReplicateMovement(true);
}
Bunu yaptıktan göreceğiz ki platform serverde olduğu gibi oyuncuda da hareket edecek.
Peki sadece oyuncuda hareket ettirip serverde sabit tutabilir miyiz? Elbette!:
void AMovingPlatform::BeginPlay()
{
Super::BeginPlay();
if (!HasAuthority()) {
SetReplicates(true);
SetReplicateMovement(true);
}
fakat bunu yaptığımız zaman şöyle bir sorunla karşılaşacağız oyuncu ekranında cisim gözükmemesine rağmen serverde cisim aynı yerde durduğu için oyuncu karakteri o bölgede görünmez bir cisime takılacak ve hareket edemeyecek.
