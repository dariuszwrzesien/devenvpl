---
date: 2017-03-25 22:00
title: "Auditor - Bundles"
layout: post
description: "Podzia� projektu Auditor na Symfonowe bundle"
tags: apietka dsp2017-adrian auditor symfony bundle
category: dsp2017-adrian
author: apietka
comments: true
---

W mojej przygodzie programistycznej wspiera�em zespo�y projektowe w tworzeniu aplikacji w oparciu o r�ne platformy, a co za tym idzie r�wnie� r�ne j�zyki programowania i frameworki. W projektach .NET bardzo podoba mi si� uk�adanie struktury solucji. Dowolno�� w tworzeniu podfolder�w, podzia� na projekty. W ekosystemie Symfony mamy tzw. podzia� na Bundle. Jest on co prawda do�� restrykcyjnie opisany w dokumentacji - "modu�y kt�re mo�na reu�ywa� pomi�dzy projektami". Moje rowziw�zanie troszk� przeczy tej g��wnej zasadzie, ale mam ku temu pewne argumenty. O tym jednak za moment, najpierw rzu�cie okiem na to jak wygl�da struktura projektu.

# AppBundle

AppBundle to g��wny modu� - zawieraj�cy dost�pne endpointy (kontrolery) oraz implementacje niezb�dnych interfejs�w.

- Controller - definicja kontroler�w - czyli adres�w URL dost�pnych dla aplikacji frontendowej,
- EventListener	- implementacja zwi�zana z przechwytywaniem zdarze�, w przypadku aplikacji *Auditor* - funkcjonalno�ci wykonywanych po akcjach zdefiniowanych w �adaniach - *Commands*,
- Repository - implementacja repozytorium w oparciu o ORM *Doctrine*.

AppBudle posiada referencje do wszystkich pozosta�ych modu��w - spina je do *kupy* :)

# AuditorBundle

To mo�liwie najbardziej abstrakcyjny byt. Wszelka implementacja podlega zasadzie *Design by Contract* (poza zalezno�ci� od CqrsBundle). Wszystkie zewn�trzne zale�no�ci wykorzystuj� interfejsy. Po to by dostarczy� w fazie implementacji odpowiedni� klas�.

- Command - �adania, akcje zmieniaj�ce stan systemu (struktura danych oraz obs�uga ��dania - je�eli korzysta z zewn�trznych serwis�w, korzysta z zdefiniowanych interfejs�w),
- Entity - "pseudo" domain obejcts, w faktycznym stanie s� to encje dostarczone za pomoc� *ORM Doctrine*, przechowuj� dane, umo�liwiaj� ustawianie oraz odczytywanie danych,
- Event - definicja zdarzenia - co si� sta�o (nazwa klasy), jakie informacje nale�y przekazaj� (w�a�ciwo�ci klasy),
- Repository - interfejsy dost�pu do repozytorium, implementowane w *AppBudle*.

AuditorBundle posiada referencje do modu�u *CqrsBundle*.

# CqrsBundle

To najbardziej uniwersalny *modu�*. Nie jest w �aden spos�b skojarzonuy

- Commanding - 
- Eventing -
- Exception -

CqrsBundle nie posiada referencji do �adnych modu��w. Jedynie do podstawowych bibliotek PHP 7.x.

# Dlaczego?

Mam jasny podzia� pomi�dzy kontraktem (zdefiniowanym w AuditorBundle), a jego implementacj� (czyli implementacj� interfejs�w w AppBundle). Je�eli b�d� chcia� oprze� projekt o zupe�nie inny *model write* - wystarczy, �e podmieni� definicj� w konfiguracji DIC oraz dodam brakuj�ce implementacje adapter�w (dla *Repository*). Definicja  z zasad SOLID zostaje spe�niona - Open/Closed Principle.

Reu�ywalno�� (CqrsBundle), podmiana (implementacja interfejs�w z AuditorBundle) - �y� i nie umiera� :)