---
date: "2023-04-15T13:15:00Z"
title: 'Hugging Face Transfomers: Q&A Modell'
description: "
Die Welt der künstlichen Intelligenz (KI) hat in den letzten Monaten erheblich an Bedeutung gewonnen, vor allem durch den beeindruckenden Erfolg von Large Language Models wie ChatGPT. Ein wesentlicher Akteur in diesem Bereich ist die Hugging Face Community, die eine Vielzahl an KI-Modellen als Open-Source-Lösungen anbietet. Ein herausragender Bestandteil ihres Portfolios ist die Transformers-Bibliothek, die den Einsatz dieser Modelle erheblich vereinfacht. In diesem Artikel demonstrieren wir am Beispiel eines deutschen Frage-Antwort-Modells, wie einfach die Entwicklung eines Python-Skripts für die natürliche Sprachverarbeitung zur Analyse von Webinhalten sein kann. Für Interessierte ist das Skript auf GitHub leicht zugänglich.
"
image: header.png
---

In den letzten Monaten haben KIs durch den Erfolg von Large Language Models (z.B. ChatGPT) viel Aufmerksamkeit erhalten.
Dabei gibt es schon seit längerer Zeit mit [Hugging Face](https://huggingface.co/) eine Community, die verschiedene KI Modelle als Open Source bereitstellt.
Hugging Face verfügt außerdem mit der [Transformers](https://huggingface.co/docs/transformers/index) über eine Bibliothek, die es erlaubt diese Modelle sehr einfach einzusetzen.
Dieser Artikel zeigt am Beispiel eines deutschen Question & Answer Modells, wie einfach ein Python Skript zum Durchsuchen einer Webseite mit natürlicher Sprache geschrieben werden kann.
Das entsprechende Skript liegt außerdem auf [GitHub](https://github.com/bit-jkraushaar/hugging-face-transformers-examples) als Quellcode vor.

# Schritt 1: Installation von Python, Transformers und PyTorch

Die Installation von Python wird im Folgenden nicht beschrieben.
Alle Beispiele sind jedoch getestet mit **Python 3.10.9** und **Windows 11**.

Transformers lässt sich sehr einfach über `pip` installieren:

```
pip install transfomers
```

Zusätzlich benötigt Transformers entweder TensorFlow oder Pytorch.
Über dieses Kommando wird PyTorch mit Cuda 11.7 installiert:

```
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu117
```

Weitere Installationsmöglichkeiten finden sich auf der PyTorch [Homepage](https://pytorch.org/).

# Schritt 2: Das Skript schreiben

Im Folgenden erkläre ich die einzelnen Schritte zum Schreiben des Skripts.
Das vollständige Skript ist auch auf [GitHub](https://github.com/bit-jkraushaar/hugging-face-transformers-examples) zu finden.

Zunächst importieren wir alle notwendigen Abhängigkeiten:

```python
from transformers import pipeline
import requests
from bs4 import BeautifulSoup
import sys
```

`Requests` und `BeautifulSoup` nutzen wir, um die Webseite zu laden und den Text zu extrahieren.
Als nächstes lesen wir die URL und die zu stellende Frage aus den Kommandozeilenparametern:

```python
url = sys.argv[1]
question = sys.argv[2]
```

Die URL nutzen wir gleich, um die Webseite abzufragen und den Text der Webseite zu extrahieren:

```python
response = requests.get(url)
html = response.content
soup = BeautifulSoup(html, 'html.parser')
text = soup.get_text()
```

Nun nutzen wir die [pipeline](https://huggingface.co/docs/transformers/pipeline_tutorial) Funktion aus der Tranformers Bibliothek, um das entsprechende Modell zu laden:

```python
model_name = 'deepset/gelectra-base-germanquad'
nlp = pipeline('question-answering', model=model_name, tokenizer=model_name)
```

Wir nutzen hier das Modell [deepset/gelectra-base-germanquad](https://huggingface.co/deepset/gelectra-base-germanquad), das speziell zum Durchsuchen von deutschen Texten trainiert wurde und unter MIT Lizenz zur Verfügung gestellt wird.
Der erste Parameter der Funktion besagt außerdem, dass wir das Modell für "question-answering" nutzen möchten.

Nun können wir dieses Modell nutzen, um eine Anfrage zu stellen:

```python
input = { 'question': question, 'context': text}
result = nlp(input)
print(f"{ result['answer'] } (Score: {result['score']})")
```

Die beiden Parameter sind jeweils die Frage aus dem Kommandozeilen-Parameter und der aus der Webseite extrahierte Text.
Das `result` ist ein Dictionary.
Aus diesem Dictionary nutzen wir `answer` für die Antwort und `score` um zu erfahren, wie wahrscheinlich es sich bei der Antwort um die richtige Antwort handelt (wie "sicher" sich das Modell ist).

# Schritt 3: Ausführen

Nun lässt sich das Skript einfach folgendermaßen ausführen (hier wird davon ausgegangen, dass das Skript "german-qa.py" heißt):

```
> python german-qa.py https://de.wikipedia.org/wiki/Fu%C3%9Fball-Bundesliga "Wer ist deutscher Rekordmeister?"
> FC Bayern München (Score: 0.7162473201751709)
```

Wird die Frage hingegen auf einer Seite gestellt, auf der die Antwort nicht zu finden ist, sieht das Ergebnis so aus:

```
> python german-qa.py https://de.wikipedia.org/wiki/Fu%C3%9Fball "Wer ist deutscher Rekordmeister?"
> Konrad Koch (Score: 0.07024901360273361)
```

# Fazit

Ich hoffe, ich konnte mit diesem Artikel zeigen, wie einfach die Anwendung von KI Modellen heutzutage geworden ist.
In der [Hugging Face Transformers Dokumentation](https://huggingface.co/docs/transformers/index) gibt es noch viele weitere Beispiele für Anwendungsfälle.

Der Quellcode zu diesem Artikel steht auf [GitHub](https://github.com/bit-jkraushaar/hugging-face-transformers-examples) bereit.