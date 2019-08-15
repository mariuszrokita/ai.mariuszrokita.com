---
title: "Który model ML jest lepszy? Czyli co nie co o testach istotności statystycznej."
date: 2019-04-17
#categories: [azure cli, devops]
#tags: [azure cli, docker, devops, python]
excerpt: "Wykorzystanie testów istotności statystycznej do porównania wyników modeli uczenia maszynowego."
---

Jednym z głównych zadań data scientist'a, czy też machine learning inżyniera jest modelowanie danych. Po długim etapie analizy zbioru danych, ich czyszczeniu, tworzeniu nowych atrybutów, wyborze najlepszych zmiennych, w końcu następuje faza modelowania. W dużym skrócie owa czynność polega na wyborze jednego lub kilku algorytmów, dostarczeniu do nich danych, a następnie na treningu - gdzie zadaniem algorytmu jest wykrycie relacji w dostarczonych danych i nauczenie się tej wiedzy. Model powinien być na tyle dobry, by poprawnie wykryć związki w danych treningowych, ale jednocześnie wykryta wiedza musi być na tyle ogólna, by model mógł ją skutecznie aplikować do nowych, dotychczas nieznanych danych.

W czasie modelowania powstaje wiele modeli uczenia maszynowego. Czasami dość łatwo wskazać model, który osiąga najlepsze wyniki i może być wdrożony na produkcję. Problem jednak jest wtedy, gdy mamy kilka modeli uzyskujących podobne wyniki i trzeba wskazać ten, który jest naprawdę najlepszy. Może to być szczególnie ważne w sytuacji, gdy przykładowo porównujemy ze sobą model Random Forest z siecią neuronową i nie wiemy, czy wyniki uzyskiwane przez sieć są istotnie lepsze (co też uzasadnia ponoszenie dużych kosztów związanych z trenowaniem sieci neuronowych).

Do porównania wyników uzyskiwanych przez modele uczenia maszynowego można wykorzystać testy istotności statystycznej. Po więcej szczegółów zapraszam na [Github'a](https://github.com/mariuszrokita/statistics/blob/master/notebooks/statistical-significance-tests-to-compare-ml-models.ipynb), gdzie umieściłem notatnik z konkretnym przykładem - porównanie wyników z dwóch modeli uczenia maszynowego.
