---
title: "Czy Azure Cognitive Services pomoże nam porozmawiać z obcokrajowcem?"
date: 2019-08-13
categories: [azure, cognitive services]
tags: [azure, cognitive services, python]
excertp: "Wstępna ocena jakości usług automatycznego wykrywania języka mówionego wraz z transkrypcją oferowanych przez Azure Cognitive Services."
---

## Język angielski - nie zawsze jest to możliwe

Jest czas wakacji - ludzie podróżują po świecie, poznają nowe osoby. Zazwyczaj w komunikacji z innymi polegamy na języku angielskim, bo jest to obecnie *lingua franca* świata biznesu, edukacji, technologii i rozrywki. Ale niestety może się też zdarzyć tak, że nasz rozmówca zna tylko i wyłącznie swój język ojczysty; i wówczas potencjalnie możemy mieć kłopot. Nie ma co jednak załamywać rąk, wszak w wielu sytuacjach można skorzystać z usprawnień i dobroci modeli maszynowych dostępnych w naszych smartfonach - dostępnych jako aplikacja Google Translate, czy też strona internetowa deepl.com. Przykładów jest zdecydowanie więcej.

Jednocześnie można się zastanowić - jak na tle konkurencji wypadają usługi oferowane przez Microsoft dostępne w ramach Azure Cognitive Services? Czy są one w stanie poprawnie wykryć język i przetłumaczyć wypowiedź na język dla nas zrozumiały?

## Automatyczna detekcja języka mówionego w Cognitive Services

Na dzień dzisiejszy, główna usługa poświęcona mowie -  [Speech Services](https://azure.microsoft.com/en-us/services/cognitive-services/speech-services/) - niestety nie oferuje opcji automatycznej detekcji języka mówionego. Zarówno [Speech SDK](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/speech-sdk), jak i [Speech REST API](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/swagger-documentation) wymagają od nas podania informacji o języku źródłowym, który ma być użyty do transkrypcji lub do tłumaczenia na inny język. Jeśli tego nie zrobimy - albo dostaniemy [błąd HTTP 4xx](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-speech-to-text), albo zostanie użyty domyślny kod języka: [en-US](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/python/console/speech_sample.py).

Nie jest jednak tak źle, jak nam się wydaje. Azure dostarcza usługę [Video Indexer](https://azure.microsoft.com/en-us/services/media-services/video-indexer/), która pomoże nam rozwiązać nasz problem.

### Autodetekcja języka w tekście pisanym

Warto w tym momencie wspomnieć, że gdybyśmy mieli do czynienia tylko i wyłącznie z tekstem, wówczas bez problemu moglibyśmy skorzystać z usługi [Translator Text API](https://azure.microsoft.com/en-us/services/cognitive-services/translator-text-api/), która właśnie oferuje opcję automatycznego wykrywania języka. Zainteresowani mogą prześledzić [przykłady](https://docs.microsoft.com/en-us/azure/cognitive-services/translator/quickstart-detect?pivots=programming-language-csharp) wykorzystania tej usługi.

## Usługa Video Indexer

[Video Indexer](https://azure.microsoft.com/en-us/services/media-services/video-indexer/) to usługa, która analizuje dostarczone materiały audio-video i zwraca różne informacje i spostrzeżenia na ich temat - np. wiek, płeć oraz emocje na rozpoznanych twarzach, wykrywanie różnych mówców biorących udział w rozmowie, rozpoznawanie obiektów itp. Od ponad roku jest też dostępna opcja automatycznego wykrywania języka mówionego [Spoken Language Identification (LID)](https://azure.microsoft.com/en-us/blog/spoken-language-identification-in-video-indexer/), która jest dostępna  dla video, jak i dla plików z dźwiękiem.

Są dwa sposoby użycia funkcji LID. Korzystając z [portalu](https://vi.microsoft.com/en-us/) możemy wybrać opcję **Auto detect** z listy języków dostępnych w czasie wgrywania pliku audio (lub video). ![image-title-here](/assets/images/2019-08-13/VideoIndexerPortal.png)

Jeśli zaś skorzystamy z [REST API](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?), wówczas parametr wskazujący język powinien mieć wartość **auto**.

## Video Indexer - ocena jakości funkcji LID
