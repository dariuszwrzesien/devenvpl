---
date: 2017-04-23 23:30
title: "Auditor - O organizacji test�w automatycznych"
layout: post
description: "Organizacja test�w jednostkowych oraz integracyjnych dla projektu Auditor."
tags: apietka dsp2017-adrian auditor php tests symfony
category: dsp2017-adrian
author: apietka
comments: true
---

Aktualnie w projekcie *Auditor* wykorzystuj� dwa typy test�w automatycznych - testy jednostkowe oraz integracyjne. W obu przypadkach za uruchamianie test�w, ich uk�ad i wykorzystywane asercje odpowiada najpopularniejszy PHPowy test framework - **[PHPUnit](https://phpunit.de/)**. Jego autorem jest [Sebastian Bergmann](https://twitter.com/s_bergmann). Zreszt� bardzo spoko go�� - w 2015 roku mia�em okazj� uczestniczy� w warsztatach prowadzonych przez Sebastiana na konferencji [IPC Berlin](https://phpconference.com/en/). Poni�ej pami�tkowe zdj�cie ekipy *Future Processing* wraz z Sebastianem i Stefanem (thePHP.cc):

![Future Processing & thePHP.cc]({{ site.url }}/assets/images/2017/04/auditor-tests/ipc-2015.jpg)

## Testy jednostkowe (*unit test*)

Najpro�ciej m�wi�c **testy jednostkowe** to kod weryfikuj�cy poprawno�� zachowania kodu produkcyjnego. Sprawdzenie czy elementy systemu zachowuj� si� w spos�b jaki zaplanowa� programista. Co jest bardzo wa�ne - testuje si� jednostk� czyli mo�liwie najmniejszy element systemu - obiekt, a najlepiej konkretn� metod� lub funkcj�.

O testach jednostkowych rozpisa� si� ciekawie Maciej, polecam zatem jego artyku� - [Zapowied� minicyklu o testach](http://devstyle.pl/2011/08/08/ut-0-zapowiedz-minicyklu-o-testach/)

## Testy integracyjne (*integration test*)

**Testy integracyjne** odpowiadaj� za sprawdzenie kilku komponent�w, kt�re ze sob� wsp�pracuj�. Nie dotycz� konkretnej jednostki, a grupy jednostek realizuj�cych zadanie. To ten rodzaj test�w, kt�ry nie powinien posiada� mock�w na operacje bazodanowe czy I/O. Komunikacja z zewn�trznymi serwisami, baz� danych czy systemem plik�w s� naturaln� cz�ci� takich test�w.

W projekcie *Auditor* testy integracyjne wykorzystuj� osobn� baz� danych. Ich "wej�ciem" jest akcja kontrolera dla kt�rej dostarczam dane wej�ciowe (symulacja metody POST). Nast�pnie weryfikuj� stan bazy danych - czy wszystkie dane zosta�y zapisane w za�o�ony spos�b. Te testy b�d� pokrywa�y w projekcie jedynie �cie�k� *[Happy Path](https://en.wikipedia.org/wiki/Happy_path)*, przypadki brzegowe powinny zosta� pokryte testami jednostkowymi (uwaga - potencjalnie to podej�cie mo�e ulec zmianie).

Sposob�w na przeprowadzanie i weryfikacj� test�w integracyjnych jest wiele. Wszystko zale�y od kontekstu �rodowiska w kt�rym dzia�a aplikacja oraz prawd� m�wi�c - przyj�tego w zespole programistycznym za�o�enia (oby tylko popartego konkretnymi argumentami :)).

## Struktura

```
tests
|-- Common
|  |-- Mock
|  |-- Builder
|
|-- Integration
|  |-- CreateNewProjectTest.php
|  |-- phpunit.xml
|
|-- Unit
|  |-- AppBundle
|     |-- SymfonyCommandHandlerResolverTest.php
|  |-- phpunit.xml
```

**Common** - jest folderem zawieraj�cym wsp�dzielone pomi�dzy testami klasy. To tutaj znajdowa� b�d� si� *buildery*, *mocki*, *stuby* i *fake* wykorzystywane w testach.

**Integration** - przestrze� do umieszczania *test�w integracyjnych*. Bez podfolder�w. Spowodowane jest to sam� ilo�ci� test�w oraz faktem, �e dotycz� one jedynie ��da� (*[Command](/dsp2017-adrian/2017/03/19/auditor-cqrs-command.html)* czyli akcji zmieniaj�cych stan systemu) kt�re mo�e wykona� u�ytkownik aplikacji.

**Unit** - przestrze� dla *test�w jednostkowych*. Podfoldery odzwierciedlaj� *[bundle](/dsp2017-adrian/2017/03/26/auditor-bundles.html)* aplikacji.

Poszczeg�lne *Test Suite* to kolejne klasy (zawieraj�ce przypadki testowe - *Test Case*). Nie s� one r�wnoznaczne z bardzo cz�sto przyj�tym za�o�eniem (momentami b�ednym!), jedna klasa produkcyjna = jedna klasa testowa. Je�li potrzebuj� utworzy� kilka przypadk�w testowych dla metody - mog� wyodr�bni� je do osobnego *Test Suite*. Jednak warto zada� sobie wtedy pytanie: "*Czy na pewno metoda kt�r� chc� przetestowa� nie robi zbyt du�o? Mo�e lepiej wyodr�bni� j� jako osobn� klas�?*"

Ka�dy z typ�w test�w posiada osob� konfiguracj� *PHPUnit*, ze wzgl�du na fakt, �e przy najbli�szych zmianach zamierzam doda� *Code Coverage* dla *test�w jednostkowych*.

## Composer Scripts

Aby u�atwi� sobie uruchamianie test�w z konsoli, stworzy�em dwa aliasy na poziomie [Composer Scripts](https://getcomposer.org/doc/articles/scripts.md). Wpis w *composer.json* wygl�da nast�puj�co:

~~~json
{
  "scripts": {
    "unit": "php vendor/phpunit/phpunit/phpunit --testdox --configuration tests/Unit/phpunit.xml",
    "integration": "php vendor/phpunit/phpunit/phpunit --testdox --configuration tests/Integration/phpunit.xml"
  }
}
~~~

Dzi�ki takiemu rozwi�zaniu, do uruchomienia konkretnego zestawu test�w (jednostkowych lub integracyjnych) wystarczy, �e wydam polecenie:

```
$: composer unit

// lub

$: composer integration
```

W tym po�cie przedstawi�em podej�cie do struktury test�w w moim projekcie. Wi�cej szczeg��w (g��wnie implementacyjnych) postaram si� opisa� w osobnych artyku�ach.

Jak organizujecie swoje testy w aplikacjach PHP? Podzielcie si� swoimi do�wiadczeniami :-)