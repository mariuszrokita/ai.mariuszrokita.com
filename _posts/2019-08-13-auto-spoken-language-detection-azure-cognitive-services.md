---
title: "Czy Azure Cognitive Services pomoże nam porozmawiać z obcokrajowcem?"
date: 2019-08-13
categories: [azure, cognitive services]
tags: [azure, cognitive services, python]
excertp: "Wstępna ocena jakości usług automatycznego wykrywania języka mówionego wraz z transkrypcją oferowanych przez Azure Cognitive Services."
---

## Język angielski - nie zawsze jest to możliwe

Jest czas wakacji - ludzie podróżują po świecie, poznają nowe osoby. Zazwyczaj w komunikacji z innymi polegamy na języku angielskim, bo przecież jest to najpopularniejszy obecnie język świata. Ale niestety może się też zdarzyć tak, że nasz rozmówca zna tylko i wyłącznie swój język ojczysty; i wówczas potencjalnie możemy mieć kłopot. Nie ma co jednak załamywać rąk, wszak w wielu sytuacjach można skorzystać z dobroci modeli maszynowych dostępnych w naszych smartfonach - dostępnych jako aplikacja Google Translate, czy też strona internetowa deepl.com. Przykładów jest zdecydowanie więcej.

Jednocześnie zastanawiam się - jak na tle konkurencji wypadają usługi oferowane przez Microsoft i dostępne w ramach Azure Cognitive Services? Czy są one w stanie poprawnie wykryć język i przetłumaczyć wypowiedź na język dla nas zrozumiały?

## Automatyczna detekcja języka mówionego w Cognitive Services

Na dzień dzisiejszy, główna usługa poświęcona mowie -  [Speech Services](https://azure.microsoft.com/en-us/services/cognitive-services/speech-services/) - niestety nie oferuje opcji automatycznej detekcji języka mówionego. Zarówno [Speech SDK](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/python/console/speech_sample.py), jak i [REST API](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-speech-to-text) wymagają od nas podania informacji o języku źródłowym, który ma być użyty do transkrypcji lub do tłumaczenia na inny język.

Co ciekawe, Azure dostarcza usługę [Video Indexer](https://azure.microsoft.com/en-us/services/media-services/video-indexer/), która analizuje dostarczony materiał audio-video i zwraca różne informacje na ich temat - np. rozpoznawanie twarzy, rozpoznawanie emocji, wykrywanie różnych mówców itp. Jest też dostępna opcja automatycznego wykrywania języka mówionego - zarówno dla video, jak i dla plików z dźwiękiem. Usłudze tej będzie poświęcona dalsza część wpisu.

Warto też wspomnieć, że gdybyśmy mieli do czynienia tylko i wyłącznie z tekstem, wówczas bez problemu moglibyśmy skorzystać z usługi [Translator Text API](https://azure.microsoft.com/en-us/services/cognitive-services/translator-text-api/), która właśnie oferuje opcję automatycznego wykrywania języka.