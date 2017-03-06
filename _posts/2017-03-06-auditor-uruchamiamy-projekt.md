---
date: 2017-03-06 23:00
title: "Auditor - Uruchamiamy projekt"
layout: post
description: "Struktura projektu wraz z podstawow� konfiguracj�"
tags: apietka dsp2017-adrian auditor
category: dsp2017-adrian
author: apietka
---

Postanowi�em rozdzieli� interfejs u�ytkownika od cz�ci backendowej. Dlatego te�, b�d� tworzy� aplikacj� sk�adaj�c� si� z dw�ch element�w - *backend* oraz *frontend*. Pami�taj, �e tworz� aplikacj� typu **SPA** (*Single-page application*).

### Backend - API

Aplikacj� backendow� nap�dza framework **Symfony 3**, wykorzystuj�cy wzorzec architektoniczny **MVC** (*Model-View-Controller*). B�dzie to swoiste API dla aplikacji frontendowej. Ca�o�� oparta o zasad� (en. *priniciple*) - **CQRS** (*Command Query Responsibility Segregation*). Czyli w wielkim skr�cie m�wi�c - rozdzielam model zapisu od odczytu, a wszystko co dzieje si� w aplikacji mo�e by� przyporz�dkowane do jednego rodzaju akcji - odczytu (*Query*) lub zapisu (*Command*).

O samej implementacji, rozpisz� si� w dw�ch zupe�nie niezale�nych artyku�ach. Jednak b�dzie to om�wienie implementacji w j�zyku PHP. Na teraz polecam z zapoznaniem si� z ebookiem Ma�ka z [devstyle.pl](http://devstyle.pl/) - [CQRS Pragmatycznie](http://devstyle.pl/ksiazki/cqrs-pragmatycznie/).

### Frontend - WebUI

AngularJS jak wszystko, ma swoje wady i zalety. Jedn� z wad jest zdecydowanie bardzo ubogi routing. Dlatego te�, od pocz�tku b�d� u�ywa� rozbudowanej wersji, upsrawniaj�cej routing aplikacji - [AngularUI Router](https://github.com/angular-ui/ui-router). W utrzymaniu *Clean Code* pomog� mi regu�y spisane przez John Papa, cho� b�dzie to raczje wyb�r kilku z nich, ni�eli pr�ba implementacji ka�dej. Mo�esz o nich poczyta� tutaj - [Angular 1 Style Guide](https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md).

## Wymagania

Co jednak jest niezb�dne aby uruchomi� aplikacj�? Aktualnie po stronie backendowej, wymagany jest interpreter PHP w wersji 7.1 oraz PHPowy dependency manager - [composer](https://getcomposer.org/). That's all.

Aplikacja frontendowa jest nieco bardziej wymagaj�ca. Nale�y zainstalowa� [Node.js](https://nodejs.org) wraz z managerem pakiet�w *npm*. Nast�pnie doinstalowa� globalnie task runnera - [grunt](https://gruntjs.com/). Najlepiej wszystko w najnowszej, stabilnej wersji.

## Uruchomienie

Je�eli jeste�my pewni, �e wszystkie niezb�dne elementy uk�adanki zosta�y zainstalowane, pozostaje nam �ci�gni�cie repozytorium kodu.

```
$: git clone git@github.com:devenvpl/auditor.git
$: cd auditor
```

Uruchomienie backendu, jest banalnie proste.

```
$: cd api && composer install && php bin/console server:run
```

Pozostaje jedynie uruchomi� aplikacj� frontendow�.

```
$: cd webui && npm install && grunt build && grunt server
```

Od tej pory dwa elementy aplikacji dost�pne s� pod adresami:

- backend - [http://localhost:8000](http://localhost:8000)
- frontend - [http://localhost:3000](http://localhost:3000)

Na razie jednak pusto. Mam szkielet. Nic si� nie dzieje, ale to do czasu :)