---
title: "Czy Azure Cognitive Services pomoże nam porozmawiać z obcokrajowcem?"
date: 2019-08-13
categories: [azure, cognitive services]
tags: [azure, cognitive services, python]
excerpt: "Wstępna ocena jakości usług automatycznego wykrywania języka mówionego wraz z transkrypcją oferowanych przez Azure Cognitive Services."
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

## Dane testowe

Do oceny jakości automatycznej detekcji tekstu mówionego wykorzystano ścieżki audio z wybranych materiałów video umieszczonych w serwisie Youtube.com:

1. [język arabski](https://www.youtube.com/watch?v=0pb0F4HOCN8)
1. [język francuski](https://www.youtube.com/watch?v=OJPt7Fa10rQ)
1. [język hiszpański](https://www.youtube.com/watch?v=i4q-YNQ9Efo)
1. [język niemiecki](https://www.youtube.com/watch?v=a7vokd6eFK4)
1. [język włoski](https://www.youtube.com/watch?v=U8Ar3VhHe-I)

Z każdego filmu video pobrano dokładnie pierwsze 75 sekund dźwięku. W ten sposób materiały audio są dłuższe o 25%, w stosunku do [minimalnych wymagań](https://docs.microsoft.com/en-us/azure/media-services/video-indexer/language-identification-model) dla przetwarzanych materiałów dźwiękowych.

## Video Indexer - ocena jakości funkcji LID

Poniżej przedstawiono wyniki otrzymane z usługi Video Indexer:

|  Materiał audio   | Rozpoznane jako | Rozpoznany tekst |
| ----------------  | --------------- | ---------------- |
| audio arabskie    | en-US           | I Salaam alaykum, Sibaja hired. Muscle higher. To spell a higher. OK, for her Luka. And I'll be hired. My smoker. Anna is me. Come on broker. I know let. Minena hunt. Aina Tuscan. Aina Tamil. Mahoya Duca. Mahu Haiwan, Oklahoma football. Serta will Aquatica mother felt Allium Malter stuff Alvin Hiatal swore. After near Tacoma closer at. And Azure air. And Alchon worried the harbor Ilham Mom. And at a bun, burritos, Thira or read one outlook at the Sotero Moussa at the city. A nurse at their own Azure.|
| audio francuskie  | fr-FR           | Aujourd'hui une vidéo avec beaucoup d'amour car on va voir comment les amoureux français s'appellent entre eux et surtout on va vous annoncer les fameuses les très attendues offre de l'été voilà donc vous allez avoir 20 pourcent de réduction sur tous les produits de français avec Pierre avec le code promo été 2019 sans accent d'accord comme ça alors c'est super intéressant parce que comme c'est surtout les produit avec si vous mettez 2 produit dans votre panier vous avez 20 pourcent 3 produit 30 pourcent. Le produit 40 pourcent et ça se cumule donc à la fin il va voir jusqu'à 70 pourcent bah c'est presque gratuit quoi alors profitez en et apprenez le français pendant l'été alors on commence mon canard aller ma puce on y va ma pupuce alors oui c'est l'été il était chaud c'est un peu la saison quand même de l'amour et donc on va voir comment les Français et là c'est vraiment incroyable à chaque fois qu'on parle de ça en classe les élèves sont mort de rire? Donc on va voir comment les Français s'appellent entre eux avec leur petit mot d'amour bien sûr.|
| audio hiszpańskie | it-IT           | Ora sì che la scheda lì come stai se todo bien la vita va bien bienvenidos bienvenidos a Este Canal che se già ma sei già m'hai espagnol con? È spagnolo con migo espagnol con on sig. giusto quanto gli Este un Canal donde puedes ver bivio se ne spagnol para aprender Frances no e no videos en espagnol per la prendere e espanyol i ohi tengo una pregunta tengo una pregunta tengo una presunta una estudiante de spagnoli che si ghe nuestra Chiavenna? Espagnol con quando mi appetito me ha perdido por favor PA per il per favore qua per favore explicar Como se usa se en espagnol se? Bueno bueno non ho il link o un problema non è un problema di Otello e io te l'ho x plico g Otello Act plico che ero pronunziare bianki ero pronunciar vien porche mean di ciò che che no pronunziò muy bien keeper?|
| audio niemieckie  | de-DE           | Hallo Leute Wir haben heute Besuch von Mo und Mohammed die beiden kommen aus dem Sudan studieren gerade in Deutschland und sie haben uns mit einem sehr interessanten Thema angesprochen wie ihr wisst gibt es im Moment Proteste im Sudan die Menschen protestieren gegen die militärregierung und für mehr Demokratie und deswegen Fragen wir heute die Menschen hier auf der Straße wart ihr schon mal bei einem Protest und wogegen protestiert ihr los geht's los geht's. Unsere Frage heute ist warst du schon mal bei einem Protest ja Ich war schon mal bei einem Protest und was für ein Protest war das das war eine Demonstration gegen die Massentierhaltung und gegen die agrarindustrie wie sie gerade im Moment irgendwie stattfindet vor ein paar Jahren auch in Berlin ja Ich war bei manchen Demos mal dabei erzähl mal was waren das für die einmal diese Friday Computers Demos und einmal eine Demo als gleichzeitig noch eine Demo von rechten war also die gegen den Vodafone. Waren sie schon mal auf einem Protest Na ja ich Glaub 9 kurz vor dem Irak Krieg ja Zweit 19 10098 Oder-So-Ähnlich 20 Jahre. |
| audio włoskie     | it-IT           | Ciao a tutti e bentornati sul mio canale spero che stiate tutti benissimo e che siate pronti per questo video oggi voglio consigliarvi 05:00 podcast italiani da ascoltare nel vostro tempo libero per migliorare le vostre capacità di ascolto in italiano so che molti di voi ascolta uno podcast quindi so che vi piacciono i podcast e per questo motivo ho deciso di documentarmi e di andare a cercare dei podcast nuovi? E soprattutto interessanti già in passato vi avevo parlato dei 5 podcast però adesso voglio fare un video aggiornato e quindi me ne consiglio altri 5 ma se volete vedere quel video che ho fatto in passato vi lascio il link nella descrizione del video qui sotto cominciamo con il primo podcast che si chiama racconti il titolo si spiega da solo si tratta di una serie di racconti di fantasia? Ispirati da fatti realmente accaduti ma anche da libri di romanzi e anche semplicemente dalla fantasia degli autori ogni racconto è narrato dall'autore del racconto. |

## Podsumowanie

Usługa automatycznego rozpoznawania dźwięku LID nie jest idealna. Potrafiła ona rozpoznać prawidłowo 3 języki (francuski, niemiecki, włoski), a pomyliła się w 2 przypadkach (arabski, hiszpański). Nie weryfikowałem jakości innych serwisów konkurencyjnych firm, ale jedno jest pewne - zespół Azure Cognitive Services musi jeszcze trochę popracować nad poprawą jakości modelu wykrywającego język mówiony w usłudze Video Indexer.

Warto też wspomnieć o jakości transkrypcji. Dla języków dobrze rozpoznanych (francuski, niemiecki, włoski), jakość rozpoznanego tekstu można uznać za wysoką. Wykryłem drobne pomyłki:

1. język niemiecki - (brakujące "*hier*" w "*auch [hier] in Berlin*", źle przetłumaczony rok 1998 jako "*Zweit 19 10098 Oder-So-Ähnlich 20*"),
1. język włoski - (nieprawidłowe "*“so che molti di voi ascolta uno podcas*" zamiast "*so que molti di voi ascoltano podcast*")

jednak są to drobne nieprawidłowości i nie mają aż tak wielkiego wpływu na ogólne zrozumienie przekazu. Będąc na wakacjach w obcym kraju moglibyśmy po prostu poprosić rozmówcę o powtórzenie informacji.
